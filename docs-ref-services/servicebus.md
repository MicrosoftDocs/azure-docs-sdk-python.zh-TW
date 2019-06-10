---
title: 適用於 Python 的服務匯流排程式庫
description: 適用於服務匯流排之 Python 用戶端和管理程式庫的參考文件
keywords: Azure, Python, SDK, API, 傳訊, pubsub, pub-sub, 訊息代理程式
author: annatisch
ms.author: antisch
manager: mayurid
ms.date: 01/15/2019
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 014f34b6ba3d3a2a8ce15afcd826195d6051f98a
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376760"
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="21425-104">適用於 Python 的服務匯流排程式庫</span><span class="sxs-lookup"><span data-stu-id="21425-104">Service Bus libraries for Python</span></span>

<span data-ttu-id="21425-105">Microsoft Azure 服務匯流排支援一組以雲端為基礎、訊息導向的中介軟體技術，包括可靠的訊息佇列和持久的發佈/訂閱訊息。</span><span class="sxs-lookup"><span data-stu-id="21425-105">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span>

* [<span data-ttu-id="21425-106">SDK 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="21425-106">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="21425-107">SDK 參考文件</span><span class="sxs-lookup"><span data-stu-id="21425-107">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a><span data-ttu-id="21425-108">v0.50.0 的新功能為何？</span><span class="sxs-lookup"><span data-stu-id="21425-108">What's new in v0.50.0?</span></span>

<span data-ttu-id="21425-109">從 0.50.0 版起，新的 AMQP 型 API 可用於傳送及接收訊息。</span><span class="sxs-lookup"><span data-stu-id="21425-109">As of version 0.50.0 a new AMQP-based API is available for sending and receiving messages.</span></span> <span data-ttu-id="21425-110">此更新涉及**重大變更**。</span><span class="sxs-lookup"><span data-stu-id="21425-110">This update involves **breaking changes**.</span></span>

<span data-ttu-id="21425-111">請閱讀[從 v0.21.1 遷移至 v0.50.0](#migration-from-v0211-to-v0500) 以判斷您在這個階段是否適合升級。</span><span class="sxs-lookup"><span data-stu-id="21425-111">Please read [Migration from v0.21.1 to v0.50.0](#migration-from-v0211-to-v0500) to determine if upgrading is right for you at this time.</span></span>

<span data-ttu-id="21425-112">新的 AMQP 型 API 今後會提供改善的訊息傳遞可靠性、效能及擴充功能。</span><span class="sxs-lookup"><span data-stu-id="21425-112">The new AMQP-based API offers improved message passing reliability, performance and expanded feature support going forward.</span></span>
<span data-ttu-id="21425-113">新的 API 也支援非同步作業 (根據 asyncio) 來傳送、接收和處理訊息。</span><span class="sxs-lookup"><span data-stu-id="21425-113">The new API also offers support for asynchronous operations (based on asyncio) for sending, receiving and handling messages.</span></span>

<span data-ttu-id="21425-114">如需舊版 HTTP 型作業的文件，請參閱[使用舊版 API 的 HTTP 型作業](#using-http-based-operations-of-the-legacy-api)。</span><span class="sxs-lookup"><span data-stu-id="21425-114">For documentation on the legacy HTTP-based operations please see [Using HTTP-based operations of the legacy API](#using-http-based-operations-of-the-legacy-api).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21425-115">必要條件</span><span class="sxs-lookup"><span data-stu-id="21425-115">Prerequisites</span></span>

* <span data-ttu-id="21425-116">Azure 訂用帳戶 - [建立免費帳戶](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="21425-116">Azure subscription - [Create a free account](https://azure.microsoft.com/free/)</span></span>
* <span data-ttu-id="21425-117">Azure [服務匯流排命名空間和管理認證](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span><span class="sxs-lookup"><span data-stu-id="21425-117">Azure [Service Bus namespace and management credentials](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span></span>
* [<span data-ttu-id="21425-118">Python 2.7-3.7</span><span class="sxs-lookup"><span data-stu-id="21425-118">Python 2.7-3.7</span></span>](https://www.python.org/downloads/)

## <a name="installation"></a><span data-ttu-id="21425-119">安裝</span><span class="sxs-lookup"><span data-stu-id="21425-119">Installation</span></span>

```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a><span data-ttu-id="21425-120">連線到 Azure 服務匯流排</span><span class="sxs-lookup"><span data-stu-id="21425-120">Connect to Azure Service Bus</span></span>

### <a name="get-credentials"></a><span data-ttu-id="21425-121">取得認證</span><span class="sxs-lookup"><span data-stu-id="21425-121">Get credentials</span></span>

<span data-ttu-id="21425-122">使用以下 Azure CLI 程式碼片段，以服務匯流排連接字串填入環境變數 (您也可以在 Azure 入口網站中找到此值)。</span><span class="sxs-lookup"><span data-stu-id="21425-122">Use the Azure CLI snippet below to populate an environment variable with the Service Bus connection string (you can also find this value in the Azure portal).</span></span> <span data-ttu-id="21425-123">此程式碼片段會針對 Bash Shell 加以格式化。</span><span class="sxs-lookup"><span data-stu-id="21425-123">The snippet is formatted for the Bash shell.</span></span>

```Bash
RES_GROUP=<resource-group-name>
NAMESPACE=<servicebus-namespace>

export SB_CONN_STR=$(az servicebus namespace authorization-rule keys list \
 --resource-group $RES_GROUP \
 --namespace-name $NAMESPACE \
 --name RootManageSharedAccessKey \
 --query primaryConnectionString \
 --output tsv)
```

### <a name="create-client"></a><span data-ttu-id="21425-124">建立用戶端</span><span class="sxs-lookup"><span data-stu-id="21425-124">Create client</span></span>

<span data-ttu-id="21425-125">填入 `SB_CONN_STR` 環境變數後，即可建立 ServiceBusClient。</span><span class="sxs-lookup"><span data-stu-id="21425-125">Once you've populated the `SB_CONN_STR` environment variable, you can create the ServiceBusClient.</span></span>

```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

<span data-ttu-id="21425-126">如果想要使用非同步作業，請使用 `azure.servicebus.aio` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="21425-126">If you wish to use asynchronous operations, please use the `azure.servicebus.aio` namespace.</span></span>

```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

## <a name="service-bus-queues"></a><span data-ttu-id="21425-127">服務匯流排佇列</span><span class="sxs-lookup"><span data-stu-id="21425-127">Service Bus queues</span></span>

<span data-ttu-id="21425-128">服務匯流排佇列是儲存體佇列的替代方案，其可能適用於使用推送型傳遞 (使用長期輪詢) 而需要更進階傳訊功能的案例 (訊息大小較大、訊息順序、單一作業破壞性讀取、排程的傳遞)。</span><span class="sxs-lookup"><span data-stu-id="21425-128">Service Bus queues are an alternative to Storage queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

### <a name="create-queue"></a><span data-ttu-id="21425-129">建立佇列</span><span class="sxs-lookup"><span data-stu-id="21425-129">Create queue</span></span>

<span data-ttu-id="21425-130">這會在服務匯流排命名空間內建立新的佇列。</span><span class="sxs-lookup"><span data-stu-id="21425-130">This creates a new queue within the Service Bus namespace.</span></span> <span data-ttu-id="21425-131">如果命名空間內已經存在同名的佇列，則會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="21425-131">If a queue of the same name already exists within the namespace an error will be raised.</span></span>

```python
sb_client.create_queue("MyQueue")
```

<span data-ttu-id="21425-132">您也可指定用以設定佇列行為的選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="21425-132">Optional parameters to configure the queue behavior can also be specified.</span></span>

```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a><span data-ttu-id="21425-133">取得佇列用戶端</span><span class="sxs-lookup"><span data-stu-id="21425-133">Get a queue client</span></span>

<span data-ttu-id="21425-134">QueueClient 可用來傳送和接收佇列中的訊息，以及其他作業。</span><span class="sxs-lookup"><span data-stu-id="21425-134">A QueueClient can be used to send and receive messages from the queue, along with other operations.</span></span>

```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a><span data-ttu-id="21425-135">傳送訊息</span><span class="sxs-lookup"><span data-stu-id="21425-135">Sending messages</span></span>

<span data-ttu-id="21425-136">佇列用戶端可以一次傳送一個或多則訊息：</span><span class="sxs-lookup"><span data-stu-id="21425-136">The queue client can send one or more messages at a time:</span></span>

```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```

<span data-ttu-id="21425-137">每次呼叫 QueueClient.send 都會建立新的服務連線。</span><span class="sxs-lookup"><span data-stu-id="21425-137">Each call to QueueClient.send will create a new service connection.</span></span> <span data-ttu-id="21425-138">若要對多次傳送呼叫重複使用相同的連線，您可以開啟傳送端：</span><span class="sxs-lookup"><span data-stu-id="21425-138">To reuse the same connection for multiple send calls, you can open a sender:</span></span>

```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```

<span data-ttu-id="21425-139">如果您使用非同步用戶端，則上述作業會使用非同步語法：</span><span class="sxs-lookup"><span data-stu-id="21425-139">If you are using an asynchronous client, the above operations will use async syntax:</span></span>

```python
from azure.servicebus.aio import Message

message = Message("Hello World")
await queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
async with queue_client.get_sender() as sender:
    await sender.send(message_one)
    await sender.send(message_two)
```

### <a name="receiving-messages"></a><span data-ttu-id="21425-140">接收訊息</span><span class="sxs-lookup"><span data-stu-id="21425-140">Receiving messages</span></span>

<span data-ttu-id="21425-141">您可從作為連續迭代器的佇列接收訊息。</span><span class="sxs-lookup"><span data-stu-id="21425-141">Messages can be received from a queue as a continuous iterator.</span></span> <span data-ttu-id="21425-142">訊息接收的預設模式是 [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read)，其要求明確完成每則訊息，以便將它從佇列中移除。</span><span class="sxs-lookup"><span data-stu-id="21425-142">The default mode for message receiving is [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), which requires each message to be explicitly completed in order that it be removed from the queue.</span></span>

```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```

<span data-ttu-id="21425-143">服務連線會針對整個迭代器維持開啟狀態。</span><span class="sxs-lookup"><span data-stu-id="21425-143">The service connection will remain open for the entirety of the iterator.</span></span> <span data-ttu-id="21425-144">如果您發現自己僅部分逐一查看訊息資料流，則應該在 `with` 陳述式中執行接收端，確保連線已關閉：</span><span class="sxs-lookup"><span data-stu-id="21425-144">If you find yourself only partially iterating the message stream, you should run the receiver in a `with` statement to ensure the connection is closed:</span></span>

```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
<span data-ttu-id="21425-145">如果您使用非同步用戶端，則上述作業會使用非同步語法：</span><span class="sxs-lookup"><span data-stu-id="21425-145">If you are using an asynchronous client, the above operations will use async syntax:</span></span>

```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a><span data-ttu-id="21425-146">服務匯流排主題和訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="21425-146">Service Bus topics and subscriptions</span></span>

<span data-ttu-id="21425-147">服務匯流排主題和訂用帳戶是服務匯流排佇列的抽象概念，其使用發佈/訂閱模式提供一對多的通訊形式。</span><span class="sxs-lookup"><span data-stu-id="21425-147">Service Bus topics and subscriptions are an abstraction on top of Service Bus queues that provide a one-to-many form of communication, in a publish/subscribe pattern.</span></span> <span data-ttu-id="21425-148">訊息會傳送至主題並傳遞給一或多個相關聯的訂用帳戶，這適合於調整為大量收件者。</span><span class="sxs-lookup"><span data-stu-id="21425-148">Messages are sent to a topic and delivered to one or more associated subscriptions, which is useful for scaling to large numbers of recipients.</span></span>

### <a name="create-topic"></a><span data-ttu-id="21425-149">建立主題</span><span class="sxs-lookup"><span data-stu-id="21425-149">Create topic</span></span>

<span data-ttu-id="21425-150">這會在服務匯流排命名空間內建立新的主題。</span><span class="sxs-lookup"><span data-stu-id="21425-150">This creates a new topic within the Service Bus namespace.</span></span> <span data-ttu-id="21425-151">如果已經存在同名的主題，則會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="21425-151">If a topic of the same name already exists an error will be raised.</span></span>

```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a><span data-ttu-id="21425-152">取得主題用戶端</span><span class="sxs-lookup"><span data-stu-id="21425-152">Get a topic client</span></span>

<span data-ttu-id="21425-153">TopicClient 可用來將訊息傳送到主題，以及其他作業。</span><span class="sxs-lookup"><span data-stu-id="21425-153">A TopicClient can be used to send messages to the topic, along with other operations.</span></span>

```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a><span data-ttu-id="21425-154">建立訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="21425-154">Create subscription</span></span>

<span data-ttu-id="21425-155">這會在服務匯流排命名空間內，針對指定的主題建立新訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="21425-155">This creates a new subscription for the specified topic within the Service Bus namespace.</span></span>

```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a><span data-ttu-id="21425-156">取得訂用帳戶用戶端</span><span class="sxs-lookup"><span data-stu-id="21425-156">Get a subscription client</span></span>

<span data-ttu-id="21425-157">SubscriptionClient 可用來接收來自主題的訊息，以及其他作業。</span><span class="sxs-lookup"><span data-stu-id="21425-157">A SubscriptionClient can be used to receive messages from the topic, along with other operations.</span></span>

```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a><span data-ttu-id="21425-158">從 v0.21.1 遷移至 v0.50.0</span><span class="sxs-lookup"><span data-stu-id="21425-158">Migration from v0.21.1 to v0.50.0</span></span>

<span data-ttu-id="21425-159">0.50.0 版引進了重大變更。</span><span class="sxs-lookup"><span data-stu-id="21425-159">Major breaking changes were introduced in version 0.50.0.</span></span> <span data-ttu-id="21425-160">原始 HTTP 型 API 仍適用於 v0.50.0，不過現在存在於新的命名空間之下：`azure.servicebus.control_client`。</span><span class="sxs-lookup"><span data-stu-id="21425-160">The original HTTP-based API is still available in v0.50.0 - however it now exists under a new namesapce: `azure.servicebus.control_client`.</span></span>

### <a name="should-i-upgrade"></a><span data-ttu-id="21425-161">我應該升級嗎？</span><span class="sxs-lookup"><span data-stu-id="21425-161">Should I upgrade?</span></span>

<span data-ttu-id="21425-162">新套件 (v0.50.0) 不會提供任何超過 v0.21.1 的 HTTP 型作業改善。</span><span class="sxs-lookup"><span data-stu-id="21425-162">The new package (v0.50.0) offers no improvements in HTTP-based operations over v0.21.1.</span></span> <span data-ttu-id="21425-163">除了現在存在於新的命名空間之下以外，HTTP 型 API 完全相同。</span><span class="sxs-lookup"><span data-stu-id="21425-163">The HTTP-based API is identical except that it now exists under a new namespace.</span></span> <span data-ttu-id="21425-164">基於這個理由，如果您只想要使用 HTTP 型作業 (`create_queue`、`delete_queue` 等等)，在此時升級不會有任何額外的好處。</span><span class="sxs-lookup"><span data-stu-id="21425-164">For this reason if you only wish to use HTTP-based operations (`create_queue`, `delete_queue` etc) - there will be no additional benefit in upgrading at this time.</span></span>

### <a name="how-do-i-migrate-my-code-to-the-new-version"></a><span data-ttu-id="21425-165">如何將我的程式碼遷移至新版本？</span><span class="sxs-lookup"><span data-stu-id="21425-165">How do I migrate my code to the new version?</span></span>

<span data-ttu-id="21425-166">只要變更匯入命名空間，即可將針對 v0.21.0 撰寫的程式碼移植到 0.50.0 版：</span><span class="sxs-lookup"><span data-stu-id="21425-166">Code written against v0.21.0 can be ported to version 0.50.0 by simply changing the import namespace:</span></span>

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a><span data-ttu-id="21425-167">使用舊版 API 的 HTTP 型作業</span><span class="sxs-lookup"><span data-stu-id="21425-167">Using HTTP-based operations of the legacy API</span></span>

<span data-ttu-id="21425-168">下列文件描述舊版 API，且應適用於希望在不進行任何其他變更的情形下，將現有程式碼移植到 v0.50.0 的使用者。</span><span class="sxs-lookup"><span data-stu-id="21425-168">The following documentation describes the legacy API and should be used for those wishing to port existing code to v0.50.0 without making any additional changes.</span></span> <span data-ttu-id="21425-169">使用 v0.21.1 的使用者也可以將此參考當作指導方針。</span><span class="sxs-lookup"><span data-stu-id="21425-169">This reference can also be used as guidance by those using v0.21.1.</span></span>
<span data-ttu-id="21425-170">如需撰寫新的程式碼，我們建議使用上述的新 API。</span><span class="sxs-lookup"><span data-stu-id="21425-170">For those writing new code, we recommend using the new API described above.</span></span>

### <a name="service-bus-queues"></a><span data-ttu-id="21425-171">服務匯流排佇列</span><span class="sxs-lookup"><span data-stu-id="21425-171">Service Bus queues</span></span>

#### <a name="shared-access-signature-sas-authentication"></a><span data-ttu-id="21425-172">共用存取簽章 (SAS) 驗證</span><span class="sxs-lookup"><span data-stu-id="21425-172">Shared Access Signature (SAS) authentication</span></span>

<span data-ttu-id="21425-173">若要使用共用存取簽章驗證，請使用下列程式碼建立服務匯流排服務：</span><span class="sxs-lookup"><span data-stu-id="21425-173">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a><span data-ttu-id="21425-174">存取控制服務 (ACS) 驗證</span><span class="sxs-lookup"><span data-stu-id="21425-174">Access Control Service (ACS) authentication</span></span>

<span data-ttu-id="21425-175">新服務匯流排命名空間不支援 ACS。</span><span class="sxs-lookup"><span data-stu-id="21425-175">ACS is not supported on new Service Bus namespaces.</span></span> <span data-ttu-id="21425-176">我們建議[將應用程式遷移至 SAS 驗證](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)。</span><span class="sxs-lookup"><span data-stu-id="21425-176">We recommend [migrating applications to SAS authentication](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span></span> <span data-ttu-id="21425-177">若要在舊版服務匯流排命名空間內使用 ACS 驗證，請使用下列程式碼建立 ServiceBusService：</span><span class="sxs-lookup"><span data-stu-id="21425-177">To use ACS authentication within an older Service Bus namesapce, create the ServiceBusService with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```

#### <a name="sending-and-receiving-messages"></a><span data-ttu-id="21425-178">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="21425-178">Sending and receiving messages</span></span>

<span data-ttu-id="21425-179">**create\_queue** 方法可用來確保佇列存在：</span><span class="sxs-lookup"><span data-stu-id="21425-179">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```

<span data-ttu-id="21425-180">**send\_queue\_message** 方法之後可透過呼叫來對佇列插入訊息：</span><span class="sxs-lookup"><span data-stu-id="21425-180">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="21425-181">**send\_queue\_message_batch** 方法之後可透過呼叫來一次傳送數則訊息：</span><span class="sxs-lookup"><span data-stu-id="21425-181">The **send\_queue\_message_batch** method can then be called to  send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```

<span data-ttu-id="21425-182">然後，可以呼叫 **receive\_queue\_message** 方法對訊息清除佇列。</span><span class="sxs-lookup"><span data-stu-id="21425-182">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a><span data-ttu-id="21425-183">服務匯流排主題</span><span class="sxs-lookup"><span data-stu-id="21425-183">Service Bus topics</span></span>

<span data-ttu-id="21425-184">**create\_topic** 方法可用來建立伺服器端主題：</span><span class="sxs-lookup"><span data-stu-id="21425-184">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```

<span data-ttu-id="21425-185">**傳送\_主題\_訊息**方法可以用來將訊息傳送至主題：</span><span class="sxs-lookup"><span data-stu-id="21425-185">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="21425-186">**send\_topic\_message_batch** 方法可用來一次傳送數則訊息：</span><span class="sxs-lookup"><span data-stu-id="21425-186">The **send\_topic\_message_batch** method can be used to send  several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="21425-187">請考慮到在 Python 3 中，str 訊息會以 utf-8 編碼，而且您應該要在 Python 2 中自行管理您的編碼。</span><span class="sxs-lookup"><span data-stu-id="21425-187">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="21425-188">然後，用戶端可以藉由先後呼叫 **create\_subscription** 方法和 **receive\_subscription\_message** 方法，來建立訂用帳戶和開始取用訊息。</span><span class="sxs-lookup"><span data-stu-id="21425-188">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="21425-189">請注意，您不會收到任何在訂用帳戶建立之前所傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="21425-189">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a><span data-ttu-id="21425-190">事件中樞</span><span class="sxs-lookup"><span data-stu-id="21425-190">Event Hub</span></span>

<span data-ttu-id="21425-191">事件中樞可在高輸送量情況下收集來自各種不同裝置與服務的事件資料流。</span><span class="sxs-lookup"><span data-stu-id="21425-191">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="21425-192">**create\_event\_hub** 方法可用來建立事件中樞：</span><span class="sxs-lookup"><span data-stu-id="21425-192">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```

<span data-ttu-id="21425-193">若要傳送事件：</span><span class="sxs-lookup"><span data-stu-id="21425-193">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```

<span data-ttu-id="21425-194">事件內容是事件訊息或包含多個訊息的 JSON 編碼字串。</span><span class="sxs-lookup"><span data-stu-id="21425-194">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

### <a name="advanced-features"></a><span data-ttu-id="21425-195">進階功能</span><span class="sxs-lookup"><span data-stu-id="21425-195">Advanced features</span></span>

#### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="21425-196">訊息代理程式屬性和使用者屬性</span><span class="sxs-lookup"><span data-stu-id="21425-196">Broker properties and user properties</span></span>

<span data-ttu-id="21425-197">本節說明如何使用[這裡](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)所定義的訊息代理程式和使用者屬性：</span><span class="sxs-lookup"><span data-stu-id="21425-197">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```

<span data-ttu-id="21425-198">您可以使用日期時間、整數、浮點或布林值</span><span class="sxs-lookup"><span data-stu-id="21425-198">You can use datetime, int, float or boolean</span></span>

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

<span data-ttu-id="21425-199">為了與此程式庫的舊版本相容，`broker_properties` 也可定義為 JSON 字串。</span><span class="sxs-lookup"><span data-stu-id="21425-199">For compatibility reason with old version of this library,  `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="21425-200">若為此情況，您必須負責撰寫有效的 JSON 字串，Python 在將字串傳送至 RestAPI 之前，不會先進行檢查。</span><span class="sxs-lookup"><span data-stu-id="21425-200">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a><span data-ttu-id="21425-201">後續步驟</span><span class="sxs-lookup"><span data-stu-id="21425-201">Next Steps</span></span>

* [<span data-ttu-id="21425-202">服務匯流排文件</span><span class="sxs-lookup"><span data-stu-id="21425-202">Service Bus documentation</span></span>](https://docs.microsoft.com/azure/service-bus-messaging)
* [<span data-ttu-id="21425-203">SDK 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="21425-203">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="21425-204">SDK 參考文件</span><span class="sxs-lookup"><span data-stu-id="21425-204">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [<span data-ttu-id="21425-205">其他範例</span><span class="sxs-lookup"><span data-stu-id="21425-205">Additional samples</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
