---
title: "適用於 Python 的服務匯流排程式庫"
description: "適用於服務匯流排之 Python 用戶端和管理程式庫的參考文件"
keywords: "Azure, Python, SDK, API, 傳訊, pubsub, pub-sub, 訊息代理程式"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: bf7be945f2c7a3daea93ff4e5b770459c00632c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-libraries-for-python"></a>適用於 Python 的服務匯流排程式庫

## <a name="overview"></a>概觀

Microsoft Azure 服務匯流排支援一組以雲端為基礎、訊息導向的中介軟體技術，包括可靠的訊息佇列和持久的發佈/訂閱訊息。 

## <a name="install-the-libraries"></a>安裝程式庫
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a>範例
將訊息傳送至佇列。

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/servicebus/managementlibrary)

