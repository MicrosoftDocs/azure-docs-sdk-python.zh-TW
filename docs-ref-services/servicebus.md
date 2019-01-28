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
ms.openlocfilehash: 540a2a248f7a2abcc83517bc4a4ef96ef03e8a9f
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747738"
---
# <a name="service-bus-libraries-for-python"></a>適用於 Python 的服務匯流排程式庫

Microsoft Azure 服務匯流排支援一組以雲端為基礎、訊息導向的中介軟體技術，包括可靠的訊息佇列和持久的發佈/訂閱訊息。

* [SDK 原始程式碼](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [SDK 參考文件](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a>v0.50.0 的新功能為何？
從 0.50.0 版起，新的 AMQP 型 API 可用於傳送及接收訊息。 此更新涉及**重大變更**。
請閱讀[從 v0.21.1 遷移至 v0.50.0](#migration-from-v0211-to-v0500) 以判斷您在這個階段是否適合升級。

新的 AMQP 型 API 今後會提供改善的訊息傳遞可靠性、效能及擴充功能。
新的 API 也支援非同步作業 (根據 asyncio) 來傳送、接收和處理訊息。

如需舊版 HTTP 型作業的文件，請參閱[使用舊版 API 的 HTTP 型作業](#using-http-based-operations-of-the-legacy-api)。


## <a name="prerequisites"></a>必要條件

* Azure 訂用帳戶 - [建立免費帳戶](https://azure.microsoft.com/free/)
* Azure [服務匯流排命名空間和管理認證](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)
* [Python 2.7-3.7](https://www.python.org/downloads/)


## <a name="installation"></a>安裝
```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a>連線到 Azure 服務匯流排

### <a name="get-credentials"></a>取得認證
使用以下 Azure CLI 程式碼片段，以服務匯流排連接字串填入環境變數 (您也可以在 Azure 入口網站中找到此值)。 此程式碼片段會針對 Bash Shell 加以格式化。

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

### <a name="create-client"></a>建立用戶端
填入 `SB_CONN_STR` 環境變數後，即可建立 ServiceBusClient。
```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```
如果想要使用非同步作業，請使用 `azure.servicebus.aio` 命名空間。
```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```


## <a name="service-bus-queues"></a>服務匯流排佇列
服務匯流排佇列是儲存體佇列的替代方案，其可能適用於使用推送型傳遞 (使用長期輪詢) 而需要更進階傳訊功能的案例 (訊息大小較大、訊息順序、單一作業破壞性讀取、排程的傳遞)。

### <a name="create-queue"></a>建立佇列
這會在服務匯流排命名空間內建立新的佇列。 如果命名空間內已經存在同名的佇列，則會引發錯誤。 
```python
sb_client.create_queue("MyQueue")
```
您也可指定用以設定佇列行為的選擇性參數。
```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a>取得佇列用戶端
QueueClient 可用來傳送和接收佇列中的訊息，以及其他作業。
```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a>傳送訊息
佇列用戶端可以一次傳送一個或多則訊息：
```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```
每次呼叫 QueueClient.send 都會建立新的服務連線。 若要對多次傳送呼叫重複使用相同的連線，您可以開啟傳送端：
```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```
如果您使用非同步用戶端，則上述作業會使用非同步語法：
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


### <a name="receiving-messages"></a>接收訊息
您可從作為連續迭代器的佇列接收訊息。 訊息接收的預設模式是 [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read)，其要求明確完成每則訊息，以便將它從佇列中移除。
```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```
服務連線會針對整個迭代器維持開啟狀態。
如果您發現自己僅部分逐一查看訊息資料流，則應該在 `with` 陳述式中執行接收端，確保連線已關閉：
```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
如果您使用非同步用戶端，則上述作業會使用非同步語法：
```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a>服務匯流排主題和訂用帳戶

服務匯流排主題和訂用帳戶是服務匯流排佇列的抽象概念，其使用發佈/訂閱模式提供一對多的通訊形式。 訊息會傳送至主題並傳遞給一或多個相關聯的訂用帳戶，這適合於調整為大量收件者。

### <a name="create-topic"></a>建立主題
這會在服務匯流排命名空間內建立新的主題。 如果已經存在同名的主題，則會引發錯誤。 
```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a>取得主題用戶端
TopicClient 可用來將訊息傳送到主題，以及其他作業。
```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a>建立訂用帳戶
這會在服務匯流排命名空間內，針對指定的主題建立新訂用帳戶。
```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a>取得訂用帳戶用戶端
SubscriptionClient 可用來接收來自主題的訊息，以及其他作業。
```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a>從 v0.21.1 遷移至 v0.50.0
0.50.0 版引進了重大變更。
原始 HTTP 型 API 仍適用於 v0.50.0，不過現在存在於新的命名空間之下：`azure.servicebus.control_client`。

### <a name="should-i-upgrade"></a>我應該升級嗎？
新套件 (v0.50.0) 不會提供任何超過 v0.21.1 的 HTTP 型作業改善。 除了現在存在於新的命名空間之下以外，HTTP 型 API 完全相同。 基於這個理由，如果您只想要使用 HTTP 型作業 (`create_queue`、`delete_queue` 等等)，在此時升級不會有任何額外的好處。


### <a name="how-do-i-migrate-my-code-to-the-new-version"></a>如何將我的程式碼遷移至新版本？
只要變更匯入命名空間，即可將針對 v0.21.0 撰寫的程式碼移植到 0.50.0 版：

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a>使用舊版 API 的 HTTP 型作業
下列文件描述舊版 API，且應適用於希望在不進行任何其他變更的情形下，將現有程式碼移植到 v0.50.0 的使用者。 使用 v0.21.1 的使用者也可以將此參考當作指導方針。
如需撰寫新的程式碼，我們建議使用上述的新 API。

### <a name="service-bus-queues"></a>服務匯流排佇列

#### <a name="shared-access-signature-sas-authentication"></a>共用存取簽章 (SAS) 驗證

若要使用共用存取簽章驗證，請使用下列程式碼建立服務匯流排服務：

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a>存取控制服務 (ACS) 驗證
新服務匯流排命名空間不再支援 ACS。 我們建議[將應用程式遷移至 SAS 驗證](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)。
若要在舊版服務匯流排命名空間內使用 ACS 驗證，請使用下列程式碼建立 ServiceBusService：

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
#### <a name="sending-and-receiving-messages"></a>傳送和接收訊息

**create\_queue** 方法可用來確保佇列存在：

```python
sbs.create_queue('taskqueue')
```
**send\_queue\_message** 方法之後可透過呼叫來對佇列插入訊息：

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
**send\_queue\_message_batch** 方法之後可透過呼叫來一次傳送數則訊息：

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
然後，可以呼叫 **receive\_queue\_message** 方法對訊息清除佇列。

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a>服務匯流排主題

**create\_topic** 方法可用來建立伺服器端主題：

```python
sbs.create_topic('taskdiscussion')
```
**傳送\_主題\_訊息**方法可以用來將訊息傳送至主題：

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

**send\_topic\_message_batch** 方法可用來一次傳送數則訊息：

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

請考慮到在 Python 3 中，str 訊息會以 utf-8 編碼，而且您應該要在 Python 2 中自行管理您的編碼。

然後，用戶端可以藉由先後呼叫 **create\_subscription** 方法和 **receive\_subscription\_message** 方法，來建立訂用帳戶和開始取用訊息。 請注意，您不會收到任何在訂用帳戶建立之前所傳送的訊息。

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a>事件中樞

事件中樞可在高輸送量情況下收集來自各種不同裝置與服務的事件資料流。

**create\_event\_hub** 方法可用來建立事件中樞：

```python
sbs.create_event_hub('myhub')
```
若要傳送事件：

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
事件內容是事件訊息或包含多個訊息的 JSON 編碼字串。

### <a name="advanced-features"></a>進階功能

#### <a name="broker-properties-and-user-properties"></a>訊息代理程式屬性和使用者屬性

本節說明如何使用[這裡](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)所定義的訊息代理程式和使用者屬性：

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```
您可以使用日期時間、整數、浮點或布林值

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
為了與此程式庫的舊版本相容，`broker_properties` 也可定義為 JSON 字串。
若為此情況，您必須負責撰寫有效的 JSON 字串，Python 在將字串傳送至 RestAPI 之前，不會先進行檢查。

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a>後續步驟
* [服務匯流排文件](https://docs.microsoft.com/azure/service-bus-messaging)
* [SDK 原始程式碼](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [SDK 參考文件](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [其他範例](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
