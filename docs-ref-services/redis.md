---
title: "適用於 Python 的 Azure Redis 程式庫"
description: "適用於 Redis 之 Python 用戶端程式庫的參考文件"
keywords: "Azure, Python, Redis, API, SDK, 資料庫, NoSQL"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: redis
ms.openlocfilehash: 3ba8d972e91fad229c1489800edeca08760254e6
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-redis-cache-libraries-for-python"></a>適用於 Python 的 Azure Redis 快取程式庫

## <a name="overview"></a>概觀

Azure Redis 快取是以常用的開放原始碼 Redis 專案作為基礎。 它可讓您從 Azure 應用程式存取由 Microsoft 管理的安全、專用 Redis 執行個體。

Redis 是一種進階的索引鍵值存放區，其中的索引鍵可以包含類似字串、雜湊、清單、集合和排序的集合等資料結構。 Redis 針對這些資料類型支援一組不可部分完成的操作。

深入了解 [Azure Redis 快取](https://docs.microsoft.com/azure/redis-cache/)。

## <a name="management-api"></a>管理 API

使用 Redis 管理 API 在訂用帳戶中建立和管理 Redis 資源。

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a>範例

下列範例會建立新的 Redis 快取：

```python
from azure.mgmt.redis import RedisManagementClient
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

redis_client = RedisManagementClient(
    credentials,
    subscription_id
)
group_name = 'myresourcegroup'
cache_name = 'mycachename'
redis_cache = redis_client.redis.create_or_update(
    group_name,
    cache_name,
    RedisCreateOrUpdateParameters(
        sku = Sku(name = 'Basic', family = 'C', capacity = '1'),
        location = "East US"
    )
)
# redis_cache is a RedisResourceWithAccessKey instance
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/redis/managementlibrary)

