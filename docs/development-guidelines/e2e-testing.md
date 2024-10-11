# e2e testing to validate application and API endpoints

While the validation pipeline automates the process of testing standards compliance for documentation, including the README.md files, the repository configuration and some security compliance aspects, like the implementation of [Microsoft Identity](https://learn.microsoft.com/entra/identity-platform/v2-overview), there are specifics to an application user interface and business logic, that can only be known to the sample author.

## e2e tests as a requirement

When submitting a new template, make sure to have a minimum coverage of the following concerns, using [Playwright](https://playwright.dev/). Our validation pipeline will run the tests and issue a warning when tests fail. 

> [!WARNING]
> Please note that over time, failing tests may result in a template's removal from a collection.

## Seting up Playwright

### Devcontainer 

Requires [Playwright VS Code Extension](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright) to be installed via the `.devcontainer` cofiguration.

```
"customizations": { 

    "vscode": { 

      "extensions": [ 

        "ms-playwright.playwright" 

      ] 

    } 

  }, 
```
 
### Playwright as an application dependency 

Depending on the language, you will need to follow the corresponding instructions. 

- JavaScript - https://playwright.dev/docs/intro 

- Java - https://playwright.dev/java/docs/intro 

- Python - https://playwright.dev/python/docs/intro 

- .NET - https://playwright.dev/dotnet/docs/intro 

**You will also need to follow any other language specific guidelines.**

## Testing an application deployment to Azure

We do not require extensive coverage of the application, although the more you’re testing and the higher the coverage, the more reliable your application will be. 

We do require you make sure the application was correctly deployed and is accessible, and that you test any API endpoints to return the correct data payload. 

For example, 

- any and all frontend applications that will be deployed to a hosting service as part of your template
- any and all request to AI Services or databases, that return data. 

### Getting the provisioned URLs 

To effectively test an application after provisioning and deployment, you will need to get the provisioned URLs and run the tests after `azd up` has completed.

Consider the following GitHub Action configuration, to retrieve a :

```yaml
name: Run Playwright Tests

# This workflow will run automatically after azd up is run but before down is run
on:
  workflow_run:
    workflows: 'pipeline_validation_up_done' 
    types:
      - completed

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '20'
      
      # We need the Azure Developer CLI to be available to get and set the variables
      - name: Install Azure Developer CLI
        run: |
          curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
          npm install -g azure-dev-cli

      - name: Login to Azure
        uses: azure/login@v1
        with:
          azcliversion: 'latest'
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          client-secret: ${{ secrets.AZURE_CLIENT_SECRET }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
      
      # We get the provisioned URLs for the web app hosting and the service endpoint
      - name: Get and Save WEBAPP_URL and API_URL to use on the fly
        run: |
          # Fetching WEBAPP_URL and API_URL from the environment
          WEBAPP_URL=$(azd env get-value WEBAPP_URL)
          API_URL=$(azd env get-value API_URL)

      - name: Install Dependencies
        run: npm install

      - name: Run Playwright Tests
        run: npm run test
        env:
          WEBAPP_URL: ${{ env.WEBAPP_URL }}
          API_URL: ${{ env.API_URL }}

```

The example above installs the necessary dependencies, including the Azure Developer CLI, in order to run `azd env get-value` and get the provisioned urls.

### Testing the application UI code example with Node.js

#### Step 1: Make sure the variable is set

```javascript
// playwright.config.ts or playwright.config.js
import { defineConfig } from '@playwright/test';

export default defineConfig({
  use: {
    baseURL: process.env.WEBAPP_URL || 'http://localhost:3000', // Fallback URL in case WEBAPP_URL is not set
  },
});
```

#### Step 2: Run the test 

The following test checks for the application to be accessible in the provisioned URL and have a title with value "Welcome to the Azure Chatbot template"

```javascript
// tests/app.spec.ts (for TypeScript)
// or tests/app.spec.js (for JavaScript)

import { test, expect } from '@playwright/test';

test('should display the correct heading on the home page', async ({ page }) => {
  // Navigate to the URL provided in the environment variable WEBAPP_URL
  await page.goto(process.env.WEBAPP_URL!);

  // Check that the <h1> element contains the correct text
  const h1 = await page.locator('h1');
  await expect(h1).toHaveText('Welcome to the Azure Chatbot template');
});
```

#### Testing a chat endpoint authenticated with MI

Assume this is a chatGPT like bot, that answers what the weather is like today.

```javascript
// tests/api.spec.ts (for TypeScript)
// or tests/api.spec.js (for JavaScript)

import { test, expect, request } from '@playwright/test';
import axios from 'axios';

// Function to get an access token using Managed Identity
async function getAccessToken(resource: string): Promise<string> {
  const url = `http://169.254.169.254/metadata/identity/oauth2/token?api-version=2019-08-01&resource=${encodeURIComponent(resource)}`;
  const response = await axios.get(url, {
    headers: {
      'Metadata': 'true',
    },
  });

  return response.data.access_token;
}

test('should get a ChatGPT-like response from the API', async () => {
  // API URL from environment variable
  const apiUrl = process.env.API_URL!;
  
  // Resource for Azure AD token (this would typically be the API's App ID URI or Resource URI)
  const resource = process.env.AZURE_API_RESOURCE || apiUrl;

  // Get the access token using Managed Identity
  const token = await getAccessToken(resource);

  // Make the POST request to the API using the access token for authentication
  const response = await request.newContext().post(apiUrl, {
    headers: {
      'Authorization': `Bearer ${token}`, // Add the Bearer token for authentication
      'Content-Type': 'application/json',
    },
    data: {
      prompt: "What's the weather like?", // The prompt we are sending to the API
    },
  });

  // Check that the request was successful (status code 200)
  expect(response.status()).toBe(200);

  // Parse the JSON response
  const responseData = await response.json();

  // Assuming the API returns a 'response' field with the ChatGPT-like answer
  expect(responseData).toHaveProperty('response');
  console.log('ChatGPT-like Response:', responseData.response);

  // Example assertion, modify based on actual API response structure
  expect(responseData.response).toContain('weather');
});
```

These are examples and you will need to update them to meet your usecase and business logic, as well as your constraints. 

We strongly advise you to test all user interfaces and all API endpoints in your application.