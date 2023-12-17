# GithubActions: How to create from Github an Azure ResourceGroup with Azure SDK for .NET

## 1. Create Azure Resource Group with Azure SDK for .NET

This section is explained in the following Github repo: https://github.com/luiscoco/Azure_SDK_Sample1_CreateResourceGroup

## 2. Creating the Service Principal and Obtaining Azure Credentials

Run this command for getting the Azure Credentials:

```
az ad sp create-for-rbac --name "myserviceprincipalluis" --role contributor --scopes /subscriptions/846901e6-da09-45c8-98ca-7cca2353ff0e --sdk-auth
```

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET_/assets/32194879/4cb352d6-79de-420b-9087-07520bc754c7)



## 3. 

