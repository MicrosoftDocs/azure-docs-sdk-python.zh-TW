---
title: 適用於 Python 的 Azure 事件格線程式庫
description: ''
keywords: Azure, Python, SDK, API, 事件格線
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: e5df1078116f13f959923eac3e0c7b5789545278
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534291"
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="024be-103">適用於 Python 的 Event Grid 程式庫</span><span class="sxs-lookup"><span data-stu-id="024be-103">Event Grid libraries for Python</span></span>


<span data-ttu-id="024be-104">Azure Event Grid 是完全受控的智慧型事件路由服務，可讓統一事件耗用量使用發佈-訂閱模型。</span><span class="sxs-lookup"><span data-stu-id="024be-104">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

<span data-ttu-id="024be-105">[進一步了解 ](/azure/event-grid/overview)Azure Event Grid 的相關資訊，並參考 [Azure Blob 儲存體教學課程](/azure/storage/blobs/storage-blob-event-quickstart)開始使用。</span><span class="sxs-lookup"><span data-stu-id="024be-105">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="publish-sdk"></a><span data-ttu-id="024be-106">發行 SDK</span><span class="sxs-lookup"><span data-stu-id="024be-106">Publish SDK</span></span>

<span data-ttu-id="024be-107">使用 Azure Event Grid 發佈 SDK 來驗證、建立、處理事件，並將事件發佈至主題。</span><span class="sxs-lookup"><span data-stu-id="024be-107">Authenticate, create, handle, and publish events to topics using the Azure Event Grid publish SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="024be-108">安裝</span><span class="sxs-lookup"><span data-stu-id="024be-108">Installation</span></span> 

<span data-ttu-id="024be-109">使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 安裝套件：</span><span class="sxs-lookup"><span data-stu-id="024be-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-eventgrid
```

### <a name="example"></a><span data-ttu-id="024be-110">範例</span><span class="sxs-lookup"><span data-stu-id="024be-110">Example</span></span> 

<span data-ttu-id="024be-111">下列程式碼會將事件發佈至主題。</span><span class="sxs-lookup"><span data-stu-id="024be-111">The following code publishes an event to a topic.</span></span> <span data-ttu-id="024be-112">您可以透過 Azure 入口網站或 Azure CLI，擷取主題金鑰及端點：</span><span class="sxs-lookup"><span data-stu-id="024be-112">You can retrieve the topic key and endpoint through the Azure Portal or through the Azure CLI:</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

```python
from datetime import datetime
from azure.eventgrid import EventGridClient
from msrest.authentication import TopicCredentials

def publish_event(self):

        credentials = TopicCredentials(
            self.settings.EVENT_GRID_KEY
        )
        event_grid_client = EventGridClient(credentials)
        event_grid_client.publish_events(
            "your-endpoint-here",
            events=[{
                'id' : "dbf93d79-3859-4cac-8055-51e3b6b54bea",
                'subject' : "Sample subject",
                'data': {
                    'key': 'Sample Data'
                },
                'event_type': 'SampleEventType',
                'event_time': datetime(2018, 5, 2),
                'data_version': 1
            }]
        )
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="024be-113">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="024be-113">Explore the Client APIs</span></span>](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="024be-114">管理 SDK</span><span class="sxs-lookup"><span data-stu-id="024be-114">Management SDK</span></span>

<span data-ttu-id="024be-115">透過管理 SDK 來建立、更新或刪除 Event Grid 執行個體、主題和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="024be-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the management SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="024be-116">安裝</span><span class="sxs-lookup"><span data-stu-id="024be-116">Installation</span></span> 

<span data-ttu-id="024be-117">使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 安裝套件：</span><span class="sxs-lookup"><span data-stu-id="024be-117">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="024be-118">範例</span><span class="sxs-lookup"><span data-stu-id="024be-118">Example</span></span>

<span data-ttu-id="024be-119">下列程式碼會建立一個自訂主題，並訂閱主題端點。</span><span class="sxs-lookup"><span data-stu-id="024be-119">The following creates a custom topic and subscribes an endpoint to the topic.</span></span> <span data-ttu-id="024be-120">接著，程式碼會透過 HTTPS 將事件傳送至主題。</span><span class="sxs-lookup"><span data-stu-id="024be-120">The code then sends an event to the topic through HTTPS.</span></span>
<span data-ttu-id="024be-121">RequestBin 是一個開放原始碼的第三方工具，可讓您建立端點，以及檢視傳送給它的要求。</span><span class="sxs-lookup"><span data-stu-id="024be-121">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="024be-122">移至 [RequestBin](https://requestbin.com)，然後按一下 [建立 RequestBin]  。</span><span class="sxs-lookup"><span data-stu-id="024be-122">Go to [RequestBin](https://requestbin.com), and click **Create a RequestBin**.</span></span> <span data-ttu-id="024be-123">複製 bin URL，因為您在訂閱主題時需要用到它。</span><span class="sxs-lookup"><span data-stu-id="024be-123">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

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
<span data-ttu-id="024be-124">瀏覽至先前所建立的 RequestBin URL 可看到剛傳送的事件。</span><span class="sxs-lookup"><span data-stu-id="024be-124">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="024be-125">清除資源</span><span class="sxs-lookup"><span data-stu-id="024be-125">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="024be-126">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="024be-126">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a><span data-ttu-id="024be-127">深入了解</span><span class="sxs-lookup"><span data-stu-id="024be-127">Learn more</span></span>

[<span data-ttu-id="024be-128">使用 Event Grid SDK 接收事件</span><span class="sxs-lookup"><span data-stu-id="024be-128">Receive events using the Event Grid SDK</span></span>](/azure/event-grid/receive-events)
