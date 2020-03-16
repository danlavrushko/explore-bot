# Explore MS bot framework

[dotnet new templates](https://github.com/microsoft/BotBuilder-Samples/tree/master/generators/dotnet-templates)

1. `dotnet new echobot -n ExploreBot.RootBot`

## Deployment

https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-deploy-az-cli

1. `az ad app create --display-name "displayName" --password "AtLeastSixteenCharacters_0" --available-to-other-tenants -o table`
1. 
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
1. `az bot prepare-deploy --lang Csharp --code-dir "." --proj-file-path ".\ExploreBot.RootBot.csproj"`
1. `dotnet publish -c release`
1. `Get-ChildItem -Path ".\bin\release\netcoreapp2.2\publish" | Compress-Archive -DestinationPath "code.zip" -Force`
1. `az webapp deployment source config-zip --resource-group "<resource-group-name>" --name "<name-of-web-app>" --src <project-zip-path>`
