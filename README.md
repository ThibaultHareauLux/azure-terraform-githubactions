# azure-terraform-githubactions

> How to prepare Azure to host an application deployed with Github Actions ?

## Setup

This section describes how to setup Azure and Github to create the connection between the two elements.
Follow the instructions of the documentations below and use the script below if anything is unclear.

### Documentation

[Microsoft Documentation](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-cli%2Clinux#use-the-azure-login-action-with-openid-connect)

[Github Documentation](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-azure)

### Script

```sh
az login
az ad app create --display-name myApp
$appId=<appId>
az ad sp create --id $appId
$subscriptionId=<Subscription ID>
$resourceGroupName="myApp"
$assigneeObjectId=<Assignee Object ID>
az ad app federated-credential create --id <APPLICATION-OBJECT-ID> --parameters '{ \"name\":\"<environment>-credentials\", \"issuer\": \"https://token.actions.githubusercontent.com\", \"subject\": \"repo:<organization/repo>:environment:<environment>\", \"description\": \"Credentials to operate in <environment>\",\"audiences\": [ \"api://AzureADTokenExchange\" ] }'
az ad app federated-credential create --id <APPLICATION-OBJECT-ID> --parameters '{ \"name\":\"PR-credentials\", \"issuer\": \"https://token.actions.githubusercontent.com\", \"subject\": \"repo:<organization/repo>:pull_request\", \"description\": \"Credentials to operate during a PR\",\"audiences\": [ \"api://AzureADTokenExchange\" ] }'
```

