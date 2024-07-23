---
title: 'Quickstart: Use the Azure CLI to deploy PostgreSQL on Virtual Machines applying the best practices'
description: In this quickstart, you learn how to use the Azure CLI to deploy PostgreSQL on VMs(IaaS) based on best practices.
author: belginceran
ms.service: virtual-machines
ms.collection: linux
ms.topic: quickstart
ms.date: 23/07/2024
ms.author: belginceran
ms.custom: mvc, devx-track-azurecli, mode-api, innovation-engine, linux-related-content
---

# Quickstart: Use the Azure CLI to create an Ubuntu Virtual Machine and attach an Azure Data Disk

## Define Environment Variables

The First step in this tutorial is to define environment variables.

```bash

export RANDOM_ID="$(openssl rand -hex 3)"
export MY_RESOURCE_GROUP_NAME="myPostgresResourceGroup$RANDOM_ID"
export REGION=EastUS
export MY_USERNAME=azureuser
export MY_VM_IMAGE="Ubuntu2204"
export MY_VNET_NAME="myVNet$RANDOM_ID"
export NETWORK_PREFIX="$(($RANDOM % 254 + 1))"
export MY_VNET_PREFIX="10.$NETWORK_PREFIX.0.0/16"
export MY_VM_SN_NAME="myVMSN$RANDOM_ID"
export MY_VM_SN_PREFIX="10.$NETWORK_PREFIX.0.0/24"
```
## Login to Azure using the CLI

In order to run commands against Azure using the CLI you need to login. This is done, very simply, though the `az login` command:

## Create a resource group

A resource group is a container for related resources. All resources must be placed in a resource group. We will create one for this tutorial. The following command creates a resource group with the previously defined $MY_RESOURCE_GROUP_NAME and $REGION parameters.

```bash
az group create --name $MY_RESOURCE_GROUP_NAME --location $REGION -o JSON
```

Results:

<!-- expected_similarity=0.3 -->
```json   
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myPostgresResourceGroupxxxxxx",
  "location": "eastus",
  "managedBy": null,
  "name": "myPostgresResourceGroupxxxxxx",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

## Create Network Resources 

You need to create network resources before you proceed the VMSS steps. In this step you're going to create a VNET, 2 subnets 1 for Application Gateway and 1 for VMs. You also need to have a public IP to attach your Application Gateway to be able to reach your web application from internet. 


### Create Virtual Network (VNET) and VM Subnet

```bash
az network vnet create  --name $MY_VNET_NAME  --resource-group $MY_RESOURCE_GROUP_NAME --location $REGION  --address-prefix $MY_VNET_PREFIX  --subnet-name $MY_VM_SN_NAME --subnet-prefix $MY_VM_SN_PREFIX -o JSON
```

Results:

<!-- expected_similarity=0.3 -->
```json   
{
  "newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.X.0.0/16"
      ]
    },
    "enableDdosProtection": false,
    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myPostgresResourceGroupxxxxxx/providers/Microsoft.Network/virtualNetworks/myVNetxxxxxx",
    "location": "eastus",
    "name": "myVNetxxxxxx",
    "provisioningState": "Succeeded",
    "resourceGroup": "myPostgresResourceGroupxxxxxx",
    "resourceGuid": "f00034be-612e-4462-a711-93d0bb263e46",
    "subnets": [
      {
        "addressPrefix": "10.X.0.0/24",
        "delegations": [],
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myPostgresResourceGroupxxxxxx/providers/Microsoft.Network/virtualNetworks/myVNetxxxxxx/subnets/myVMSNxxxxxx", 
        "name": "myVMSNxxxxxx",
        "privateEndpointNetworkPolicies": "Disabled",
        "privateLinkServiceNetworkPolicies": "Enabled",
        "provisioningState": "Succeeded",
        "resourceGroup": "myPostgresResourceGroupxxxxxx",
        "type": "Microsoft.Network/virtualNetworks/subnets"
      }
    ],
    "type": "Microsoft.Network/virtualNetworks",
    "virtualNetworkPeerings": []
  }
}
```





## Next Steps

* [VM Documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/)
* [Use Cloud-Init to initialize a Linux VM on first boot](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-automate-vm-deployment)
* [Create custom VM images](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-custom-images)
* [Load Balance VMs](https://learn.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-cli)