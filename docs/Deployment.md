# Deployment

https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-deploy-az-cli

### Create the app registration
```
az ad app create --display-name "displayName" --password "AtLeastSixteenCharacters_0" --available-to-other-tenants -o table`
```

### Create a bot app service in Azure
```
az deployment create `
    --template-file ".\ExploreBot.RootBot\DeploymentTemplates\template-with-new-rg.json" `
    -l <region-location-name> `
    --parameters `
    groupName="<new-group-name>" `
    groupLocation="<region-location-name>" `
    appId="<app-id-from-previous-step>" `
    appSecret="<password-from-previous-step>" 
```

### Prepare project for deployment
```
az bot prepare-deploy --lang Csharp --code-dir "." --proj-file-path ".\ExploreBot.RootBot.csproj"
```

### Publish the project
```
dotnet publish -c release
```

### Zip the publish content
```
Get-ChildItem -Path ".\bin\release\netcoreapp2.2\publish" | Compress-Archive -DestinationPath "code.zip" -Force
```

### Deploy to Azure web app
```
az webapp deployment source config-zip --resource-group "<resource-group-name>" --name "<name-of-web-app>" --src <project-zip-path>
```
