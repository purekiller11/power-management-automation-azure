# ANS Power Management Automation for Azure

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fans-cloud%2Fpower-management-automation-azure%2Fmain%2Fazure-deploy.json)

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
