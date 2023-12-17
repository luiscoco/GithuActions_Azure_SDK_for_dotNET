# GithubActions: How to create from Github an Azure ResourceGroup with Azure SDK for .NET

## 1. Create Azure Resource Group with Azure SDK for .NET

This section is explained in the following Github repo: https://github.com/luiscoco/Azure_SDK_Sample1_CreateResourceGroup

## 2. Creating the Service Principal and Obtaining Azure Credentials

Run this command for getting the Azure Credentials:

```
az ad sp create-for-rbac --name "myserviceprincipalluis" ^
--role contributor ^
--scopes /subscriptions/846901e6-da09-45c8-98ca-7cca2353ff0e ^
--sdk-auth
```

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET_/assets/32194879/4cb352d6-79de-420b-9087-07520bc754c7)

We verify the above command in Azure Portal: 

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET_/assets/32194879/0dfd3327-4593-493d-b80b-0736e1a6ee7b)

We click in the application name we registered "myserviceprincipalluis"

We can see the "Application **(client) ID**" and the "Directory **(tenant) ID**"

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET_/assets/32194879/ca75212a-438b-4b05-a560-887e15b67133)

For creating a new Secret we click in the "**Client Credential**" link

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET_/assets/32194879/b5d93270-74dd-451e-a724-b0db2bb6838e)





## 3. We create the Github repository secret for storing the AZURE_CREDENTIALS





## 4. Github actions main.yaml file

```yaml 
name: Azure C# Application

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v2

    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '8.0.x' # specify your .NET version

    - name: Install dependencies
      run: dotnet restore

    - name: Build
      run: dotnet build --configuration Release --no-restore

    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Run script
      run: dotnet run
```






