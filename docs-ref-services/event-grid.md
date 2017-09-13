---
title: "適用於 Python 的 Azure 事件格線程式庫"
description: 
keywords: "Azure, Python, SDK, API, 事件格線"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: 81c4e74b00ac59c789c5a0b83eaa10652ec6d8ac
ms.sourcegitcommit: db4608e494cb4340649bce98ba9fb4504d3686bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="37c98-103">適用於 Python 的服務匯流排程式庫</span><span class="sxs-lookup"><span data-stu-id="37c98-103">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="37c98-104">概觀</span><span class="sxs-lookup"><span data-stu-id="37c98-104">Overview</span></span>
<span data-ttu-id="37c98-105">Azure Event Grid 是完全受管的智慧型事件路由服務，可讓統一事件耗用量使用發佈-訂閱模型。</span><span class="sxs-lookup"><span data-stu-id="37c98-105">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

## <a name="management-api"></a><span data-ttu-id="37c98-106">管理 API</span><span class="sxs-lookup"><span data-stu-id="37c98-106">Management API</span></span>
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="37c98-107">範例</span><span class="sxs-lookup"><span data-stu-id="37c98-107">Example</span></span>
<span data-ttu-id="37c98-108">下列項目可建立自訂主題、訂閱主題，並觸發事件以檢視結果。</span><span class="sxs-lookup"><span data-stu-id="37c98-108">The following creates a custom topic, subscribes to the topic, and triggers the event to view the result.</span></span> <span data-ttu-id="37c98-109">RequestBin 是一個開放原始碼的第三方工具，可讓您建立端點，以及檢視傳送給它的要求。</span><span class="sxs-lookup"><span data-stu-id="37c98-109">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="37c98-110">移至 [RequestBin](https://requestb.in/)，然後按一下 [建立 RequestBin]。</span><span class="sxs-lookup"><span data-stu-id="37c98-110">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="37c98-111">複製 bin URL，因為您在訂閱主題時需要用到它。</span><span class="sxs-lookup"><span data-stu-id="37c98-111">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.eventgrid import EventGridManagementClient
import requests

RESOURCE_GROUP_NAME = 'gridResourceGroup'
TOPIC_NAME = 'gridTopic1234'
LOCATION = 'westus2'
SUBSCRIPTION_ID = 'YOUR_SUBSCRIPTION_ID'
SUBSCRIPTION_NAME = 'gridSubscription'
REQUEST_BIN_URL = 'YOUR_REQUEST_BIN_URL'

# create resource group
resource_client = ResourceManagementClient(credentials, SUBSCRIPTION_ID)
resource_client.resource_groups.create_or_update(
    RESOURCE_GROUP_NAME,
    {
        'location': LOCATION
    }
)

event_client = EventGridManagementClient(credentials, SUBSCRIPTION_ID)

# create a custom topic
event_client.topics.create_or_update(RESOURCE_GROUP_NAME, TOPIC_NAME, LOCATION)

# subscribe to a topic
scope = '/subscriptions/'+SUBSCRIPTION_ID+'/resourceGroups/'+RESOURCE_GROUP_NAME+'/providers/Microsoft.EventGrid/topics/'+TOPIC_NAME
event_client.event_subscriptions.create(scope, SUBSCRIPTION_NAME,
    {
        'destination': {
            'endpoint_url': REQUEST_BIN_URL
        }
    }
)

# send an event to topic
# get endpoint url
url = event_client.event_subscriptions.get_full_url(scope, SUBSCRIPTION_NAME).endpoint_url
# get key
key = event_client.topics.list_shared_access_keys(RESOURCE_GROUP_NAME,TOPIC_NAME).key1
headers = {'aeg-sas-key': key}
s = requests.get('https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json')
r = requests.post(url, data=s, headers=headers)
print(r.status_code)
print(r.content)
```
<span data-ttu-id="37c98-112">瀏覽至先前所建立的 RequestBin URL 可看到剛傳送的事件。</span><span class="sxs-lookup"><span data-stu-id="37c98-112">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="37c98-113">清除資源</span><span class="sxs-lookup"><span data-stu-id="37c98-113">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="37c98-114">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="37c98-114">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/managementlibrary)

