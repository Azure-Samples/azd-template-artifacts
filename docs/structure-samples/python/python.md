# Python sample guidelines

All Python samples should follow these guidelines.

* [Project folder structure](#project-folder-structure)
* [Security](#security)
* [Dependencies](#dependencies)
* [Version Compatibility](#version-compatibility)
* [Code Quality](#code-quality)
* [Testing](#testing)
* [Monitoring](#monitoring)
* [Package-specific guidelines](#package-specific-guidelines)


## Project folder structure

The following guidelines and the folders included, represent the conventional structure for a Python standard application.

```bash

~root
│
├── .devcontainer
│    ├── devcontainer.json
│    └── post-create-command.sh              
├── azure.yaml                      
│
├── .github/
│   └── workflows/
│       ├── workflow1.yml           
│       ├── workflow2.yml           
│       └── ...                     
│
├── infra/
│   ├── main.bicep                  
│   ├── main.parameters.bicep       
│   ├── abbreviations.json          
│   └── ...                         
│
├── src/                            
│   ├── projectname/                
│        ├── __init__.py             
│        ├── core/                            
│        └── ...
│                      
│
├── tests/                          
│   └── ...                         
│
├── *docs/                           
│   └── ...                         
│
├── requirements-dev.txt                
├── README.md                       
└── .gitignore                      
             

```
* optional additional docs folder for extended documentation files

## Security

### Handling Sensitive Data

Both Django and Flask have a SECRET_KEY setting used for cryptographic signing.

- SECRET_KEY must be generated and stored in Key Vault. See [main.parameters.json](https://github.com/Azure-Samples/azure-django-postgres-flexible-appservice/blob/9a9eed8a65d6b64323e67e9eb9405c903510e5d3/infra/main.parameters.json#L21), [main.bicep](https://github.com/Azure-Samples/azure-django-postgres-flexible-appservice/blob/9a9eed8a65d6b64323e67e9eb9405c903510e5d3/infra/main.bicep#L95)
- SECRET_KEY should *not* have a default value in production settings (to make sure developers don't accidentally use the default insecure secret key).

## Dependencies

You can choose to either generate requirements files using pip-tools with `.in` files or you can write them directly, as long as they follow these guidelines.

- The root should contain requirements-dev.txt for non-prod packages (linting, formatting, and testing). The package versions don't need to be specified. See [requirements-dev.txt](https://github.com/Azure-Samples/azure-fastapi-postgres-flexible-aca/blob/main/requirements-dev.txt) 
- The requirements-dev.txt file should recursively include a requirements.txt file with the production dependencies.
- The deployed code should contain `requirements.txt` for production dependencies or `pyproject.toml` if installed as an editable app. See [requirements.txt](https://github.com/Azure-Samples/azure-django-postgres-flexible-aca/blob/main/src/requirements.txt)
- The requirements.txt file should pin exact versions for each package. See [requirements.txt](https://github.com/Azure-Samples/azure-search-openai-demo/blob/main/app/backend/requirements.txt), [pyproject.toml](https://github.com/Azure-Samples/azure-flask-postgres-flexible-appservice/blob/main/src/pyproject.toml)
- The repo must be enabled to use dependabot and include a dependabot.yaml file. It should include at least two sections, one for "pip" and one for "github-actions". See [dependabot.yaml](https://github.com/Azure-Samples/azure-search-openai-demo/blob/main/.github/dependabot.yaml) Optionally, the repo can include a [dependabot-bot.yaml](https://github.com/pamelafox/flask-surveys-container-app/blob/main/.github/dependabot-bot.yml) for automatic merging of updates by [Depend-a-lot-bot](https://github.com/apps/depend-a-lot-bot).
  - The `dependabot.yaml` file must point at the directory which contains `requirements-dev.txt`. Dependabot should follow the recursive instruction and find `requirements.txt` as well.

## Version Compatibility

- Default to Python 3.11/3.12/3.13 for the deployed artifact and for the devcontainer. 
- Don't use features introduced after 3.9, unless there's a good reason to do so. 
- In `README.md` and elsewhere, use `python` to run commands.
    - Instead of `pip install –r requirements.txt`,  use `python -m pip install –r requirements.txt` 
    - The latter is more likely to work in more environments than the former `

## Code Quality

For an example of a repo that follows these guidelines, see [python-sample-template](https://github.com/Azure-Samples/python-sample-template).

- For variable naming, use [PEP8 conventions](https://pep8.org/#prescriptive-naming-conventions). Don't use the conventions of other languages, like JS/Java.
- Use `flake8` or `ruff` with 'E' and 'F' options (default). Warnings related to docstrings can be ignored. Include in `requirements-dev.txt` and configure in [pyproject.toml](https://github.com/Azure-Samples/python-sample-template/blob/f69fa84b6fa44e1a6df4b219e998618d186de48a/pyproject.toml#L1)
  - Run on pre-commit. See [.pre-commit-config.yaml](https://github.com/Azure-Samples/python-sample-template/blob/main/.pre-commit-config.yaml)
  - Run in GitHub action. See [python.yaml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/.github/workflows/python.yaml#L23)
- Use `isort` or `ruff` with 'I' option. Include in `requirements-dev.txt` and configure in [pyproject.toml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/pyproject.toml#L4)
  - Run on pre-commit. See [.pre-commit-config.yaml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/.pre-commit-config.yaml#L11)
  - Run in GitHub action. See [python.yaml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/.github/workflows/python.yaml#L23)
- Use `pyupgrade` or `ruff` with `UP` option. Include in `requirements-dev.txt` and configure in [pyproject.toml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/pyproject.toml#L4)
  - Run on pre-commit. See [.pre-commit-config.yaml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/.pre-commit-config.yaml#L11)
  - Run in GitHub action. See [python.yaml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/.github/workflows/python.yaml#L23)
- Use `black` for formatting or `ruff format`, with a reasonable line length (<= 200). Include in `requirements-dev.txt` and configure in [pyproject.toml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/pyproject.toml#L7)
- Avoid ternary operators when possible, as they often lead to unreadable code.
- For string formatting, prefer `.format()` when arguments are expressions and f-string for simple variable names. Your code should almost never use string concatenation. Do *not* use f-strings when logging.

## Testing

- The repo should use either `pytest` or the framework-specific testing module.
- The repo should include the coverage library (typically via `pytest-cov`).
- The pyproject.toml file should configure pytest to check coverage for the application code, using --cov= option. See [pyproject.toml](https://github.com/Azure-Samples/python-sample-template/blob/a2c4ae17dc15d9102b3a11e37459dcac1fb48dde/pyproject.toml#L12)
- The pyproject.toml file or CI workflow should configure coverage to fail if the coverage is below 100% threshold. See [pyproject.toml](https://github.com/pamelafox/simple-fastapi-container/blob/f0111034355c733fa55382a5af49f3193ed074cc/pyproject.toml#L21), [python.yaml](https://github.com/Azure-Samples/azure-search-openai-demo/blob/aa02563ff18ce4f5f0cca15eaa59eb6155672f8e/.github/workflows/python-test.yaml#L51)
- A CI/CD workflow should run all the unit tests, and fail if they fail.
- Tests should use matrix to run against all supported Python versions (3.8+).
- Tests should use matrix to run against all supported OSes (Windows, Mac, Linux).

### API tests

If the repo includes an API of some sort:

- Either [schemathesis](https://pypi.org/project/schemathesis/) or [hypothesis](https://pypi.org/project/hypothesis/) should be used to perform property-based testing of the API parameters. You may want to run those separately from unit tests for speed reasons. See [property_based.py](https://github.com/pamelafox/flask-charts-api-container-app/blob/main/src/tests/property_based.py), [python-check.yaml](https://github.com/pamelafox/flask-charts-api-container-app/blob/befdfa1bcaf7a1afd48874c4de22a9094f40ee3e/.github/workflows/python-check.yaml#L35)

### Frontend tests

If the repo includes a frontend (HTML/CSS):

- Playwright tests should be used for end-to-end user flow tests. See [playwright.py](https://github.com/Azure-Samples/azure-django-postgres-aca/blob/main/demo-code/relecloud/playwright.py)
- Playwright tests should be run in CI/CD (possibly in a different workflow from unit tests). See [playwright.yaml](https://github.com/Azure-Samples/azure-django-postgres-aca/blob/main/.github/workflows/playwright.yaml)
- Axe should be used to check for accessibility issues. Errors should either be uploaded to Code Scanning tab, displayed in the pull request, or break the workflow.

### Load Tests

- If not using Azure Load Testing, use [Locust](https://locust.io/) to define the load tests.

## Monitoring

- Prefer using `logging` or `logger` calls (framework-dependant) instead of `print()`. General guidance is to use` print()` for scripts, logging for web apps. The log level defaults to warning in production (generally), so `logging.info()` statements won’t show up in logs unless the app logger is configured otherwise. 

## Package-specific guidelines

#### GUnicorn

- There should be a gunicorn.conf.py file that specifies max_requests, max_requests_jitter, log_file, bind. See [FastAPI: gunicorn.conf.py](https://github.com/Azure-Samples/azure-fastapi-postgres-flexible-aca/blob/main/src/gunicorn.conf.py) or [Flask: gunicorn.conf.py](https://github.com/Azure-Samples/azure-flask-postgres-flexible-appservice/blob/main/src/gunicorn.conf.py)
  - The gunicorn.conf.py should calculate the optimal number of workers and threads based on CPU count. (If the worker class is uvicorn, threads cannot be specified).
- There should be a test to check that the gunicorn configuration file is valid. See [gunicorn_test.py](https://github.com/pamelafox/simple-fastapi-container/blob/main/src/gunicorn_test.py)

#### Flask

- The README command should use the Flask module to run the server, not call the Python file directly. See [README#running-locally](https://github.com/Azure-Samples/azure-flask-postgres-flexible-appservice#running-locally)
- The README command for running the local server should include `--debug`. See [README#local-dev](https://github.com/pamelafox/simple-flask-server-example#local-development)
- The README command for running the local server should include `--port=NNNN` (where NNNN is not 5000) to avoid collisions with built-in Mac application. See [README#running-locally](https://github.com/Azure-Samples/azure-flask-postgres-flexible-appservice#running-locally)
  - The devcontainer.json should expose port NNNN. See [devcontainer.json](https://github.com/Azure-Samples/azure-flask-postgres-flexible-appservice/blob/1857bfe14adf08f04a0c3865927097a42580764a/.devcontainer/devcontainer.json#L14)

#### Django

- The admin URL should either be disabled or be randomly generated, to prevent drive-by Django admin login attempts. See [main.bicep:ADMIN_URL](https://github.com/pamelafox/django-quiz-app/blob/main/infra/main.bicep#L76) and [production.py:ADMIN_URL](https://github.com/pamelafox/django-quiz-app/blob/main/quizsite/production.py#L9)
