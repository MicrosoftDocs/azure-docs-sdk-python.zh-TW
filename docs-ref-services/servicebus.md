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
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="0ffe6-104">適用於 Python 的服務匯流排程式庫</span><span class="sxs-lookup"><span data-stu-id="0ffe6-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="0ffe6-105">概觀</span><span class="sxs-lookup"><span data-stu-id="0ffe6-105">Overview</span></span>

<span data-ttu-id="0ffe6-106">Microsoft Azure 服務匯流排支援一組以雲端為基礎、訊息導向的中介軟體技術，包括可靠的訊息佇列和持久的發佈/訂閱訊息。</span><span class="sxs-lookup"><span data-stu-id="0ffe6-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="0ffe6-107">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="0ffe6-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a><span data-ttu-id="0ffe6-108">範例</span><span class="sxs-lookup"><span data-stu-id="0ffe6-108">Example</span></span>
<span data-ttu-id="0ffe6-109">將訊息傳送至佇列。</span><span class="sxs-lookup"><span data-stu-id="0ffe6-109">Send messages to a queue.</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="0ffe6-110">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="0ffe6-110">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/managementlibrary)

