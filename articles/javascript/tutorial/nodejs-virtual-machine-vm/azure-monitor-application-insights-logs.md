---
title: View virtual machine logs in Azure portal
description: Learn how to see your logging with Azure Monitor and Application Insights for your Linux virtual machine.
ms.topic: how-to
ms.date: 01/18/2022
ms.custom: devx-track-js, devx-track-azurecli 
---

# 6. View virtual machine logs

In this section of the tutorial, learn how to see your logging with Azure Monitor and Application Insights. 

## View virtual machine traces with Azure CLI

Use Azure CLI command, [az monitor app-insights metrics show](/cli/azure/monitor/app-insights/metrics#az-monitor-app-insights-metrics-show), to quickly see important pieces of your logs. For example, use the following command to see how many traces are in the logs. 

Remember the trace was added in the `/trace` route only. Calls to the root of your web app will not produce any trace logs. 

```azurecli
az monitor app-insights metrics show \
    --resource-group rg-demo-vm-eastus \
    --app demoWebAppMonitor \
    --metric traces/count
```

The response looks something like the following, with this example having a total count of 2 traces: 

```console
{
  "value": {
    "end": "2020-11-11T21:33:40.311000+00:00",
    "interval": null,
    "segments": null,
    "start": "2020-11-11T20:33:40.311000+00:00",
    "traces/count": {
      "sum": 2
    }
  }
}
```

<a name="view-application-traces-in-azure-portal"></a>

## View virtual machine traces in Azure portal

To view your traces as a list, the easiest method is to use the Azure portal. 

1. Open the [Azure portal](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAll) in a web browser.
1. Filter the list of resource by the resource group, `rg-demo-vm-eastus`. 
1. Select the `demoWebAppMonitor` resource. 
1. Select the **Monitoring** section's **Logs** item. If a pop-up displays queries you can select from, select **X** in the corner to dismiss the pop-up.
1. Select the **Application Insights** item named **traces** by double-clicking on it. That adds the name to the query window. 
1. Run the query by selecting the **Run** button.
1. The Azure Monitor Application Insights custom traces, from the web app, appear in a list.

    :::image type="content" source="../../media/tutorial-vm/azure-portal-application-insights-custom-trace.png" alt-text="The Azure Monitor Application Insights custom traces, from the web app, appear in a list.":::

## Next step

> [!div class="nextstepaction"]
> [Clean up your Azure resources](clean-up-resources.md) 
