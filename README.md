# ANS Power Management Automation for Azure

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fans-cloud%2Fpower-management-automation-azure%2Fmain%2Fazure-deploy.json)


## How to deploy

Click the blue "Deploy to Azure" button

Or if you prefer commandline;
1. Clone the repository `git clone https://github.com/ans-cloud/power-management-automation-azure.git`
2. Ensure you have [az cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) installed
3. Run `az login`
4. Switch to the correct subscription
5. Create a resource group (optional)
```bash
az group create \
  --name "name-for-resource-group" \
  --location "uksouth or another region of your choice"
```
6. Update the values in example.parameters.json (Optional)
6. Run the following command to deploy the ARM template
```bash
az deployment group create \
  --name "deploy-power-management-automation" \
  --resource-group "name-of-existing-resource-group" \
  --template-file "azure-deploy.json" \
  --parameters "example.parameters.json"
```


## What will this deploy?

This [ARM (Azure Resource Manager)](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/overview) template will deploy a [logicapp](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview), upon creation you can choose whether to use an existing or new [resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview#resource-groups)

## What do I need to do post deployment?

1. Navigate to your root management group (or each subscription manually)
2. Assign `Virtual Machine Contributor` rights to the logicapp's [managed identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/howto-assign-access-portal)

## How will this know which VMs to Power On or Off?

- You will need to add the `AutoOn` and `AutoOff` tags to your virtual machines.
- These tags follow the format [HH:mm](https://en.wikipedia.org/wiki/ISO_8601#Times) (Note: These must be in increments of 15 minutes)

## How will this handle different timezones

- This script is built to use UTC as the primary timezone, with a check for BST (British Summer Time) which adds an hour during the summer months.
- For this to work with other timezones, once deployed, remove the actions `Are_we_in_BST`, `If_are_we_in_BST` and change the timezone on the trigger to your timezone of choice
