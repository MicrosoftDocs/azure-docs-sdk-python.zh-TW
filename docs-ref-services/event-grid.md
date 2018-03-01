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
ms.openlocfilehash: 299b50ce8366d0c49ade28dfece98d6696a4f9ef
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="3c936-103">適用於 Python 的 Event Grid 程式庫</span><span class="sxs-lookup"><span data-stu-id="3c936-103">Event Grid libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="3c936-104">概觀</span><span class="sxs-lookup"><span data-stu-id="3c936-104">Overview</span></span>
<span data-ttu-id="3c936-105">Azure Event Grid 是完全受控的智慧型事件路由服務，可讓統一事件耗用量使用發佈-訂閱模型。</span><span class="sxs-lookup"><span data-stu-id="3c936-105">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

## <a name="management-api"></a><span data-ttu-id="3c936-106">管理 API</span><span class="sxs-lookup"><span data-stu-id="3c936-106">Management API</span></span>
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="3c936-107">範例</span><span class="sxs-lookup"><span data-stu-id="3c936-107">Example</span></span>
<span data-ttu-id="3c936-108">下列項目可建立自訂主題、訂閱主題，並觸發事件以檢視結果。</span><span class="sxs-lookup"><span data-stu-id="3c936-108">The following creates a custom topic, subscribes to the topic, and triggers the event to view the result.</span></span> <span data-ttu-id="3c936-109">RequestBin 是一個開放原始碼的第三方工具，可讓您建立端點，以及檢視傳送給它的要求。</span><span class="sxs-lookup"><span data-stu-id="3c936-109">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="3c936-110">移至 [RequestBin](https://requestb.in/)，然後按一下 [建立 RequestBin]。</span><span class="sxs-lookup"><span data-stu-id="3c936-110">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="3c936-111">複製 bin URL，因為您在訂閱主題時需要用到它。</span><span class="sxs-lookup"><span data-stu-id="3c936-111">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

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
<span data-ttu-id="3c936-112">瀏覽至先前所建立的 RequestBin URL 可看到剛傳送的事件。</span><span class="sxs-lookup"><span data-stu-id="3c936-112">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="3c936-113">清除資源</span><span class="sxs-lookup"><span data-stu-id="3c936-113">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3c936-114">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="3c936-114">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

