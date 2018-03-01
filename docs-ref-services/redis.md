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
ms.openlocfilehash: c2f8ebcbcbd7b71c1fa96e46a8148a3c0b88877f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="azure-redis-cache-libraries-for-python"></a><span data-ttu-id="b9219-104">適用於 Python 的 Azure Redis 快取程式庫</span><span class="sxs-lookup"><span data-stu-id="b9219-104">Azure Redis Cache libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="b9219-105">概觀</span><span class="sxs-lookup"><span data-stu-id="b9219-105">Overview</span></span>

<span data-ttu-id="b9219-106">Azure Redis 快取是以常用的開放原始碼 Redis 專案作為基礎。</span><span class="sxs-lookup"><span data-stu-id="b9219-106">Azure Redis Cache is based on the popular open source Redis project.</span></span> <span data-ttu-id="b9219-107">它可讓您從 Azure 應用程式存取由 Microsoft 管理的安全、專用 Redis 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b9219-107">It gives you access to a secure, dedicated Redis instance, managed by Microsoft and accessible from your Azure apps.</span></span>

<span data-ttu-id="b9219-108">Redis 是一種進階的索引鍵值存放區，其中的索引鍵可以包含類似字串、雜湊、清單、集合和排序的集合等資料結構。</span><span class="sxs-lookup"><span data-stu-id="b9219-108">Redis is an advanced key-value store, where keys can contain data structures such as strings, hashes, lists, sets, and sorted sets.</span></span> <span data-ttu-id="b9219-109">Redis 針對這些資料類型支援一組不可部分完成的操作。</span><span class="sxs-lookup"><span data-stu-id="b9219-109">Redis supports a set of atomic operations on these data types.</span></span>

<span data-ttu-id="b9219-110">深入了解 [Azure Redis 快取](https://docs.microsoft.com/azure/redis-cache/)。</span><span class="sxs-lookup"><span data-stu-id="b9219-110">Learn more about [Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/).</span></span>

## <a name="management-api"></a><span data-ttu-id="b9219-111">管理 API</span><span class="sxs-lookup"><span data-stu-id="b9219-111">Management API</span></span>

<span data-ttu-id="b9219-112">使用 Redis 管理 API 在訂用帳戶中建立和管理 Redis 資源。</span><span class="sxs-lookup"><span data-stu-id="b9219-112">Create and manage your Redis resources in your subscription with the Redis management API.</span></span>

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a><span data-ttu-id="b9219-113">範例</span><span class="sxs-lookup"><span data-stu-id="b9219-113">Example</span></span>

<span data-ttu-id="b9219-114">下列範例會建立新的 Redis 快取：</span><span class="sxs-lookup"><span data-stu-id="b9219-114">The following example creates a new Redis cache:</span></span>

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
> [<span data-ttu-id="b9219-115">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="b9219-115">Explore the Management APIs</span></span>](/python/api/overview/azure/redis/management)

