---
title: "適用於 Python 的服務匯流排程式庫"
description: "適用於服務匯流排之 Python 用戶端和管理程式庫的參考文件"
keywords: "Azure, Python, SDK, API, 傳訊, pubsub, pub-sub, 訊息代理程式"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 6c0bc66fbe8194b5b8f34ee8e29945b03ba8c242
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/24/2018
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="a586f-104">適用於 Python 的服務匯流排程式庫</span><span class="sxs-lookup"><span data-stu-id="a586f-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="a586f-105">概觀</span><span class="sxs-lookup"><span data-stu-id="a586f-105">Overview</span></span>

<span data-ttu-id="a586f-106">Microsoft Azure 服務匯流排支援一組以雲端為基礎、訊息導向的中介軟體技術，包括可靠的訊息佇列和持久的發佈/訂閱訊息。</span><span class="sxs-lookup"><span data-stu-id="a586f-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="a586f-107">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="a586f-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="servicebus-queues"></a><span data-ttu-id="a586f-108">服務匯流排佇列</span><span class="sxs-lookup"><span data-stu-id="a586f-108">ServiceBus Queues</span></span>
<span data-ttu-id="a586f-109">服務匯流排佇列是儲存體佇列的替代方案，其可能適用於使用推送型傳遞 (使用長期輪詢) 而需要更進階傳訊功能的案例 (訊息大小較大、訊息順序、單一作業破壞性讀取、排程的傳遞)。</span><span class="sxs-lookup"><span data-stu-id="a586f-109">ServiceBus Queues are an alternative to Storage Queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

<span data-ttu-id="a586f-110">服務可以使用共用存取簽章驗證或 ACS 驗證。</span><span class="sxs-lookup"><span data-stu-id="a586f-110">The service can use Shared Access Signature authentication, or ACS authentication.</span></span>

<span data-ttu-id="a586f-111">在 2014 年 8 月後使用 Azure 入口網站所建立的服務匯流排命名空間不再支援 ACS 驗證。</span><span class="sxs-lookup"><span data-stu-id="a586f-111">Service bus namespaces created using the Azure portal after August 2014 no longer support ACS authentication.</span></span> <span data-ttu-id="a586f-112">您可以使用 Azure SDK 建立 ACS 相容命名空間。</span><span class="sxs-lookup"><span data-stu-id="a586f-112">You can create ACS compatible namespaces with the Azure SDK.</span></span>

### <a name="shared-access-signature-authentication"></a><span data-ttu-id="a586f-113">共用存取簽章驗證</span><span class="sxs-lookup"><span data-stu-id="a586f-113">Shared Access Signature Authentication</span></span>

<span data-ttu-id="a586f-114">若要使用共用存取簽章驗證，請使用下列程式碼建立服務匯流排服務：</span><span class="sxs-lookup"><span data-stu-id="a586f-114">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a><span data-ttu-id="a586f-115">ACS 驗證</span><span class="sxs-lookup"><span data-stu-id="a586f-115">ACS Authentication</span></span>

<span data-ttu-id="a586f-116">若要使用 ACS 驗證，請使用下列程式碼建立服務匯流排服務：</span><span class="sxs-lookup"><span data-stu-id="a586f-116">To use ACS authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a><span data-ttu-id="a586f-117">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="a586f-117">Sending and Receiving Messages</span></span>

<span data-ttu-id="a586f-118">**create\_queue** 方法可用來確保佇列存在：</span><span class="sxs-lookup"><span data-stu-id="a586f-118">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="a586f-119">**send\_queue\_message** 方法之後可透過呼叫來對佇列插入訊息：</span><span class="sxs-lookup"><span data-stu-id="a586f-119">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="a586f-120">**send\_queue\_message_batch** 方法之後可透過呼叫來一次傳送數則訊息：</span><span class="sxs-lookup"><span data-stu-id="a586f-120">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="a586f-121">然後，可以呼叫 **receive\_queue\_message** 方法對訊息清除佇列。</span><span class="sxs-lookup"><span data-stu-id="a586f-121">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a><span data-ttu-id="a586f-122">ServiceBus 主題</span><span class="sxs-lookup"><span data-stu-id="a586f-122">ServiceBus Topics</span></span>

<span data-ttu-id="a586f-123">ServiceBus 主題是以 ServiceBus 佇列為基礎的抽象概念，可讓您輕鬆地實作發行/訂閱案例。</span><span class="sxs-lookup"><span data-stu-id="a586f-123">ServiceBus topics are an abstraction on top of ServiceBus Queues that make pub/sub scenarios easy to implement.</span></span>

<span data-ttu-id="a586f-124">**create\_topic** 方法可用來建立伺服器端主題：</span><span class="sxs-lookup"><span data-stu-id="a586f-124">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="a586f-125">**傳送\_主題\_訊息**方法可以用來將訊息傳送至主題：</span><span class="sxs-lookup"><span data-stu-id="a586f-125">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="a586f-126">**send\_topic\_message_batch** 方法可用來一次傳送數則訊息：</span><span class="sxs-lookup"><span data-stu-id="a586f-126">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="a586f-127">請考慮到在 Python 3 中，str 訊息會以 utf-8 編碼，而且您應該要在 Python 2 中自行管理您的編碼。</span><span class="sxs-lookup"><span data-stu-id="a586f-127">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="a586f-128">然後，用戶端可以藉由先後呼叫 **create\_subscription** 方法和 **receive\_subscription\_message** 方法，來建立訂用帳戶和開始取用訊息。</span><span class="sxs-lookup"><span data-stu-id="a586f-128">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="a586f-129">請注意，您不會收到任何在訂用帳戶建立之前所傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="a586f-129">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a><span data-ttu-id="a586f-130">事件中樞</span><span class="sxs-lookup"><span data-stu-id="a586f-130">Event Hub</span></span>

<span data-ttu-id="a586f-131">事件中樞可在高輸送量情況下收集來自各種不同裝置與服務的事件資料流。</span><span class="sxs-lookup"><span data-stu-id="a586f-131">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="a586f-132">**create\_event\_hub** 方法可用來建立事件中樞：</span><span class="sxs-lookup"><span data-stu-id="a586f-132">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="a586f-133">若要傳送事件：</span><span class="sxs-lookup"><span data-stu-id="a586f-133">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="a586f-134">事件內容是事件訊息或包含多個訊息的 JSON 編碼字串。</span><span class="sxs-lookup"><span data-stu-id="a586f-134">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

## <a name="advanced-features"></a><span data-ttu-id="a586f-135">進階功能</span><span class="sxs-lookup"><span data-stu-id="a586f-135">Advanced features</span></span>

### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="a586f-136">訊息代理程式屬性和使用者屬性</span><span class="sxs-lookup"><span data-stu-id="a586f-136">Broker Properties and User Properties</span></span>

<span data-ttu-id="a586f-137">本節說明如何使用[這裡](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)所定義的訊息代理程式和使用者屬性：</span><span class="sxs-lookup"><span data-stu-id="a586f-137">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                    broker_properties={'Label': 'M3'},
                    custom_properties={'Priority': 'Medium',
                                        'Customer': 'ABC'}
            )
```
<span data-ttu-id="a586f-138">您可以使用日期時間、整數、浮點或布林值</span><span class="sxs-lookup"><span data-stu-id="a586f-138">You can use datetime, int, float or boolean</span></span>

```python
props = {'hello': 'world',
            'number': 42,
            'active': True,
            'deceased': False,
            'large': 8555111000,
            'floating': 3.14,
            'dob': datetime(2011, 12, 14),
            'double_quote_message': 'This "should" work fine',
            'quote_message': "This 'should' work fine"}
sent_msg = Message(b'message with properties', custom_properties=props)
```
<span data-ttu-id="a586f-139">為了與此程式庫的舊版本相容，`broker_properties` 也可定義為 JSON 字串。</span><span class="sxs-lookup"><span data-stu-id="a586f-139">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="a586f-140">若為此情況，您必須負責撰寫有效的 JSON 字串，Python 在將字串傳送至 RestAPI 之前，不會先進行檢查。</span><span class="sxs-lookup"><span data-stu-id="a586f-140">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                    broker_properties = broker_properties
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="a586f-141">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="a586f-141">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/management)