---
title: 適用於 Python 的服務匯流排程式庫
description: 適用於服務匯流排之 Python 用戶端和管理程式庫的參考文件
keywords: Azure, Python, SDK, API, 傳訊, pubsub, pub-sub, 訊息代理程式
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 02c172ff1a54d060c6af36a5a5daa5dcbff8795c
ms.sourcegitcommit: e35ec475d4b9d8061d0528a93aa8e1c4b7db6c0d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2018
ms.locfileid: "39418953"
---
# <a name="service-bus-libraries-for-python"></a>適用於 Python 的服務匯流排程式庫

## <a name="overview"></a>概觀

Microsoft Azure 服務匯流排支援一組以雲端為基礎、訊息導向的中介軟體技術，包括可靠的訊息佇列和持久的發佈/訂閱訊息。 

## <a name="install-the-libraries"></a>安裝程式庫
```bash
pip install azure-servicebus
```

## <a name="servicebus-queues"></a>服務匯流排佇列
服務匯流排佇列是儲存體佇列的替代方案，其可能適用於使用推送型傳遞 (使用長期輪詢) 而需要更進階傳訊功能的案例 (訊息大小較大、訊息順序、單一作業破壞性讀取、排程的傳遞)。

服務可以使用共用存取簽章驗證或 ACS 驗證。

在 2014 年 8 月後使用 Azure 入口網站所建立的服務匯流排命名空間不再支援 ACS 驗證。 您可以使用 Azure SDK 建立 ACS 相容命名空間。

### <a name="shared-access-signature-authentication"></a>共用存取簽章驗證

若要使用共用存取簽章驗證，請使用下列程式碼建立服務匯流排服務：

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a>ACS 驗證

若要使用 ACS 驗證，請使用下列程式碼建立服務匯流排服務：

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a>傳送和接收訊息

**create\_queue** 方法可用來確保佇列存在：

```python
sbs.create_queue('taskqueue')
```
**send\_queue\_message** 方法之後可透過呼叫來對佇列插入訊息：

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
**send\_queue\_message_batch** 方法之後可透過呼叫來一次傳送數則訊息：

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
然後，可以呼叫 **receive\_queue\_message** 方法對訊息清除佇列。

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a>ServiceBus 主題

ServiceBus 主題是以 ServiceBus 佇列為基礎的抽象概念，可讓您輕鬆地實作發行/訂閱案例。

**create\_topic** 方法可用來建立伺服器端主題：

```python
sbs.create_topic('taskdiscussion')
```
**傳送\_主題\_訊息**方法可以用來將訊息傳送至主題：

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

**send\_topic\_message_batch** 方法可用來一次傳送數則訊息：

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

請考慮到在 Python 3 中，str 訊息會以 utf-8 編碼，而且您應該要在 Python 2 中自行管理您的編碼。

然後，用戶端可以藉由先後呼叫 **create\_subscription** 方法和 **receive\_subscription\_message** 方法，來建立訂用帳戶和開始取用訊息。 請注意，您不會收到任何在訂用帳戶建立之前所傳送的訊息。

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a>事件中樞

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

## <a name="advanced-features"></a>進階功能

### <a name="broker-properties-and-user-properties"></a>訊息代理程式屬性和使用者屬性

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

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/servicebus/management)
