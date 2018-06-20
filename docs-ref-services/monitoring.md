---
title: 適用於 Python 的 Azure 監視器程式庫
description: 適用於 Python 的 Azure 監視器程式庫參考
keywords: Azure, python, SDK, API, 監視器
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 6a073f9943b1f5af962546931e9d13372720e193
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479061"
---
# <a name="azure-monitoring-libraries-for-python"></a><span data-ttu-id="368d3-104">適用於 Python 的 Azure 監視器程式庫</span><span class="sxs-lookup"><span data-stu-id="368d3-104">Azure Monitoring libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="368d3-105">概觀</span><span class="sxs-lookup"><span data-stu-id="368d3-105">Overview</span></span> 
<span data-ttu-id="368d3-106">監視會提供資料，以確保應用程式持續運作並以健全的狀態執行。</span><span class="sxs-lookup"><span data-stu-id="368d3-106">Monitoring provides data to ensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="368d3-107">它也可協助您預防潛在問題，或是針對過去所發生的問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="368d3-107">It also helps you to stave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="368d3-108">除此之外，您還可以使用監視資料來取得應用程式的深入解析。</span><span class="sxs-lookup"><span data-stu-id="368d3-108">In addition, you can use monitoring data to gain deep insights about your application.</span></span> <span data-ttu-id="368d3-109">這些知識可協助您提升應用程式效能或維護性，或是將原本需要手動介入的動作自動化。</span><span class="sxs-lookup"><span data-stu-id="368d3-109">That knowledge can help you to improve application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>

<span data-ttu-id="368d3-110">在[這裡](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor)深入了解 Azure 監視器。</span><span class="sxs-lookup"><span data-stu-id="368d3-110">Learn more about Azure Monitor [here](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor).</span></span> 

## <a name="installation"></a><span data-ttu-id="368d3-111">安裝</span><span class="sxs-lookup"><span data-stu-id="368d3-111">Installation</span></span>
```bash
pip install azure-mgmt-monitor
```

## <a name="example---metrics"></a><span data-ttu-id="368d3-112">範例 - 計量</span><span class="sxs-lookup"><span data-stu-id="368d3-112">Example - Metrics</span></span>
<span data-ttu-id="368d3-113">這個範例會取得 Azure 上的資源計量 (VM 等)。</span><span class="sxs-lookup"><span data-stu-id="368d3-113">This sample obtains the metrics of a resource on Azure (VMs, etc.).</span></span> <span data-ttu-id="368d3-114">這個範例需要至少 0.4.0 版的 Python 套件。</span><span class="sxs-lookup"><span data-stu-id="368d3-114">This sample requires version 0.4.0 of the Python package at least.</span></span>

<span data-ttu-id="368d3-115">[這裡](https://msdn.microsoft.com/library/azure/mt743622.aspx)提供篩選條件的可用關鍵字完整清單。</span><span class="sxs-lookup"><span data-stu-id="368d3-115">A complete list of available keywords for filters is available [here](https://msdn.microsoft.com/library/azure/mt743622.aspx).</span></span>

<span data-ttu-id="368d3-116">[這裡](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics)提供每個資源類型支援的計量。</span><span class="sxs-lookup"><span data-stu-id="368d3-116">Supported metrics per resource type is available [here](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics).</span></span>

```python
import datetime
from azure.mgmt.monitor import MonitorManagementClient

# Get the ARM id of your resource. You might chose to do a "get"
# using the according management or to build the URL directly
# Example for a ARM VM
resource_id = (
    "subscriptions/{}/"
    "resourceGroups/{}/"
    "providers/Microsoft.Compute/virtualMachines/{}"
).format(subscription_id, resource_group_name, vm_name)

# create client
client = MonitorManagementClient(
    credentials,
    subscription_id
)

# You can get the available metrics of this specific resource
for metric in client.metric_definitions.list(resource_id):
    # azure.monitor.models.MetricDefinition
    print("{}: id={}, unit={}".format(
        metric.name.localized_value,
        metric.name.value,
        metric.unit
    ))

# Example of result for a VM:
# Percentage CPU: id=Percentage CPU, unit=Unit.percent
# Network In: id=Network In, unit=Unit.bytes
# Network Out: id=Network Out, unit=Unit.bytes
# Disk Read Bytes: id=Disk Read Bytes, unit=Unit.bytes
# Disk Write Bytes: id=Disk Write Bytes, unit=Unit.bytes
# Disk Read Operations/Sec: id=Disk Read Operations/Sec, unit=Unit.count_per_second
# Disk Write Operations/Sec: id=Disk Write Operations/Sec, unit=Unit.count_per_second

# Get CPU total of yesterday for this VM, by hour

today = datetime.datetime.now().date()
yesterday = today - datetime.timedelta(days=1)

metrics_data = client.metrics.list(
    resource_id,
    timespan="{}/{}".format(yesterday, today),
    interval='PT1H',
    metric='Percentage CPU',
    aggregation='Total'
)

for item in metrics_data.value:
    # azure.mgmt.monitor.models.Metric
    print("{} ({})".format(item.name.localized_value, item.unit.name))
    for timeserie in item.timeseries:
        for data in timeserie.data:
            # azure.mgmt.monitor.models.MetricData
            print("{}: {}".format(data.time_stamp, data.total))

# Example of result:
# Percentage CPU (percent)
# 2016-11-16 00:00:00+00:00: 72.0
# 2016-11-16 01:00:00+00:00: 90.59
# 2016-11-16 02:00:00+00:00: 60.58
# 2016-11-16 03:00:00+00:00: 65.78
# 2016-11-16 04:00:00+00:00: 43.96
# 2016-11-16 05:00:00+00:00: 43.96
# 2016-11-16 06:00:00+00:00: 114.9
# 2016-11-16 07:00:00+00:00: 45.4
```

## <a name="example---alerts"></a><span data-ttu-id="368d3-117">範例 - 警示</span><span class="sxs-lookup"><span data-stu-id="368d3-117">Example - Alerts</span></span>
<span data-ttu-id="368d3-118">這個範例示範如何在建立警示以確保正確監視所有資源時，自動在您的資源上設定警示。</span><span class="sxs-lookup"><span data-stu-id="368d3-118">This example shows how to automatically set up alerts on your resources when they are created to ensure that all resources are monitored correctly.</span></span>

<span data-ttu-id="368d3-119">在 VM 上建立資料來源，以警示 CPU 使用量：</span><span class="sxs-lookup"><span data-stu-id="368d3-119">Create a data source on a VM to alert on CPU usage:</span></span>
```python
from azure.mgmt.monitor import MonitorMgmtClient
from azure.mgmt.monitor.models import RuleMetricDataSource

resource_id = (
    "subscriptions/{}/"
    "resourceGroups/MonitorTestsDoNotDelete/"
    "providers/Microsoft.Compute/virtualMachines/MonitorTest"
).format(self.settings.SUBSCRIPTION_ID)

# create client
client = MonitorMgmtClient(
    credentials,
    subscription_id
)

# I need a subclass of "RuleDataSource"
data_source = RuleMetricDataSource(
    resource_uri = resource_id,
    metric_name = 'Percentage CPU'
)
```
<span data-ttu-id="368d3-120">建立 VM 在最後 5 分鐘的平均 CPU 使用率高於 90% (使用上述的資料來源) 時會加以觸發的臨界值條件：</span><span class="sxs-lookup"><span data-stu-id="368d3-120">Create a threshold condition that triggers when the average CPU usage of a VM for the last 5 minutes is above 90% (using the preceding data source):</span></span>
```python
from azure.mgmt.monitor.models import ThresholdRuleCondition

# I need a subclasses of "RuleCondition"
rule_condition = ThresholdRuleCondition(
    data_source = data_source,
    operator = 'GreaterThanOrEqual',
    threshold = 90,
    window_size = 'PT5M',
    time_aggregation = 'Average'
)
```

<span data-ttu-id="368d3-121">建立電子郵件動作：</span><span class="sxs-lookup"><span data-stu-id="368d3-121">Create an email action:</span></span>
```python
from azure.mgmt.monitor.models import RuleEmailAction

# I need a subclass of "RuleAction"
rule_action = RuleEmailAction(
    send_to_service_owners = True,
    custom_emails = [
        'monitoringemail@microsoft.com'
    ]
)
```

<span data-ttu-id="368d3-122">建立警示：</span><span class="sxs-lookup"><span data-stu-id="368d3-122">Create the alert:</span></span>
```python
rule_name = 'MyPyTestAlertRule'
my_alert = client.alert_rules.create_or_update(
    group_name,
    rule_name,
    {
        'location': 'westus',
        'alert_rule_resource_name': rule_name,
        'description': 'Testing Alert rule creation',
        'is_enabled': True,
        'condition': rule_condition,
        'actions': [
            rule_action
        ]
    }
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="368d3-123">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="368d3-123">Explore the Management APIs</span></span>](/python/api/overview/azure/monitoring/management)
