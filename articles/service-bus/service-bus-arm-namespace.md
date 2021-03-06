<properties
    pageTitle="Create a Service Bus namespace using an ARM template | Microsoft Azure"
    description="Use Azure Resource Manager template to create a Service Bus namespace"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="04/15/2016"
    ms.author="sethm;shvija"/>

# Create a Service Bus namespace using an ARM template

This article shows how to use an Azure Resource Manager (ARM) template that creates a Service Bus "Messaging" namespace with a Standard/Basic SKU. The article also defines the parameters that are specified for the execution of the deployment. You can use this template for your own deployments, or customize it to meet your requirements.

For more information about creating templates, please see [Authoring Azure Resource Manager Templates][].

For the complete template, see the [Service Bus namespace template][] on GitHub.

>[AZURE.NOTE] The following ARM templates are available for download and deployment. 
>
>-    [Create a Service Bus namespace with an Event Hub and consumer group](service-bus-arm-namespace-event-hub.md)
>-    [Create a Service Bus namespace with queue](service-bus-arm-namespace-queue.md)
>-    [Create a Service Bus namespace with topic and subscription](service-bus-arm-namespace-topic.md)
>-    [Create a Service Bus namespace with queue and authorization rule](service-bus-arm-namespace-auth-rule.md)
>
>To check for the latest templates, see the [Azure Quickstart Templates][] and search for Service Bus.

## What will you deploy?

With this template, you will deploy a Service Bus namespace with a [Basic or Standard](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

To run the deployment automatically, click the following button:

[![Deploy to Azure](./media/service-bus-arm-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## Parameters

With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed. The template includes a section called `Parameters` that contains all of the parameter values. You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to. Do not define parameters for values that will always stay the same. Each parameter value is used in the template to define the resources that are deployed.

We will describe each parameter in the template.

### serviceBusNamespaceName

The name of the Service Bus namespace to create.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### serviceBusSKU

The name of the Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) to create.

```
"serviceBusSKU": {
"type": "string",
"allowedValues": ["Basic","Standard"],
"defaultValue": "Standard"
}
```

The template defines the values that are permitted for this parameter (Basic or Standard) and assigns a default value (Standard) if no value is specified.

There is a Standard tier base charge of $10 per month, which enables you to perform up to 12.5 million operations per month at no additional cost. The Basic tier costs $0.05 per million operations.

For more information about Service Bus pricing, please see [Service Bus pricing and billing][].

### serviceBusApiVersion

The Service Bus API version of the template.

```
"serviceBusApiVersion": {

"type": "string"

}
```

## Resources to deploy

### Service Bus namespace

Creates a standard Service Bus namespace of type **Messaging**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## Commands to run deployment

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## Next steps

Now that you've created and deployed resources using ARM, learn how to manage these resources by viewing these articles:

- [Manage Azure Service Bus using Azure Automation](service-bus-automation-manage.md)
- [Manage Service Bus with PowerShell](service-bus-powershell-how-to-provision.md)
- [Manage Service Bus resources with the Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Authoring Azure Resource Manager Templates]: ../resource-group-authoring-templates.md
  [Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/
  [Service Bus pricing and billing]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
