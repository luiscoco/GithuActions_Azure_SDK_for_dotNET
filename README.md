# GithubActions: How to create from Github an Azure ResourceGroup with Azure SDK for .NET

With this simple C# application, I would like to show you how to create a **IaC (Infrastructure as Code)** solution combining **Github actions** workflow and **Azure SDK for .NET**

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

We press "**New client secret**"  in  for creating a new Secret:

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/0fa9e059-4f8c-4eb6-a0aa-f8af2a629343)

We also have to obtin the **subscriptionId**, in the Azure Portal we navigate to the Subscription service

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/1141663f-b614-4ac9-86ca-c9c117797ffb)

## 3. We create the Github repository secret for storing the AZURE_CREDENTIALS

In the Github repo select "**Settings**" option and then "**Secrets and variables**" 

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/9c620941-61bb-40c0-8fe0-84e77aa84ff3)

We create a new **Secret** for storing the **AZURE_CREDENTIALS**:

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/4578995c-40f5-4420-a35c-f28544c44434)

```json
{
  "clientId": "b47e48b4-895f-4bd7-818e-2da0deb8cd0c",
  "clientSecret": "Bhd8Q~OY7xS.C0gQ5~yXQu4xjZZQChyKL9yqnb0I",
  "subscriptionId": "846901e6-da09-45c8-98ca-7cca2353ff0e",
  "tenantId": "e099cebd-5eea-41a3-88db-bcb9a9cba83e"
}
```

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/9e8bf2af-317e-492c-a3c1-03f354a30eff)

## 4. Github actions main.yaml file

We select the **Actions** button in the Github repository and we enter the **yaml**

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/a074e10b-7836-4631-80e3-7391562cb7f7)

Now we enter the **main.yml** file source code

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

We confirm the main.yaml fiel execution was succeeded

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/1fba1cc7-9ebd-435e-a806-6bdbc9b0fc11)

## 5. Navigate to ResourceGroups in Azure Portal

We verify the ResourceGroup was created

![image](https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET/assets/32194879/b0b69365-923c-4435-868c-8799cb67948f)
