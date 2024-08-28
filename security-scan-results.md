<!-- This is meant to be a document generated and pushed by the validation pipeline bot, with the following format

IMPORTANT: THIS IS ONLY AN EXAMPLE DOCUMENT! -->

# Extended Security Recommendations 

To make your deployment more secure, consider following the recommendations below. Please note that this is not an exhaustive list. Additional measures may need to be taken. 

<!-- Example recommendations, when getting errors, for example
error: TA-000001 
error: AZR-000030 
-->
- Consider enabling auditing of diagnostic logs on the app. This enables you to recreate activity trails for investigation purposes if a security incident occurs or your network is compromised. You may do this with [Azure Monitor Platform Logs](#link) 

- Consider restricting authorized IP addresses for the API server, by configuring [Azure Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/) 

Please note that these features may increase the cost of your infrastructure. For more information [visit our pricing calculator for Azure](https://azure.microsoft.com/pricing/calculator/) 

<!-- This document is pending creation and link -->
For a more comprehensive list of best practices and security recommendations for Intelligent Applications, visit our [official documentation](#link) -->