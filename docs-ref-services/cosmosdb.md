---
title: 適用於 Python 的 Azure Cosmos DB 程式庫
description: 適用於 Azure Cosmos DB 之 Python 用戶端程式庫的參考文件
keywords: Azure, Python, SDK, API, SQL, 資料庫, PostGres, Cosmos DB, NoSQL
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: bb5e2af6a28d9543cce0c1e80fab1575b78f8cfa
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534319"
---
# <a name="azure-cosmos-db-libraries-for-python"></a><span data-ttu-id="e4828-104">適用於 Python 的 Azure Cosmos DB 程式庫</span><span class="sxs-lookup"><span data-stu-id="e4828-104">Azure Cosmos DB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="e4828-105">概觀</span><span class="sxs-lookup"><span data-stu-id="e4828-105">Overview</span></span>

<span data-ttu-id="e4828-106">在 Python 應用程式中使用 Azure Cosmos DB 來儲存和查詢 NoSQL 資料存放區中的 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="e4828-106">Use Azure Cosmos DB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="e4828-107">深入了解 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)。</span><span class="sxs-lookup"><span data-stu-id="e4828-107">Learn more about [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="e4828-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="e4828-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="e4828-109">管理程式庫</span><span class="sxs-lookup"><span data-stu-id="e4828-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="e4828-110">範例</span><span class="sxs-lookup"><span data-stu-id="e4828-110">Example</span></span>

<span data-ttu-id="e4828-111">使用類似 SQL 的查詢介面在 Azure CosmosDB 中尋找相符的文件：</span><span class="sxs-lookup"><span data-stu-id="e4828-111">Find matching documents in Azure CosmosDB using a SQL-like query interface:</span></span>

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python Azure Cosmos DB client
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
# Create a database
db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })

# Create collection options
options = {
    'offerEnableRUPerMinuteThroughput': True,
    'offerVersion': "V2",
    'offerThroughput': 400
}

# Create a collection
collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)

# Create some documents
document1 = client.CreateDocument(collection['_self'],
    { 
        'id': 'server1',
        'Web Site': 0,
        'Cloud Service': 0,
        'Virtual Machine': 0,
        'name': 'some' 
    })

# Query them in SQL
query = { 'query': 'SELECT * FROM server s' }    

options = {} 
options['enableCrossPartitionQuery'] = True
options['maxItemCount'] = 2

result_iterable = client.QueryDocuments(collection['_self'], query, options)
results = list(result_iterable)

print(results)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="e4828-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="e4828-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a><span data-ttu-id="e4828-113">範例</span><span class="sxs-lookup"><span data-stu-id="e4828-113">Samples</span></span>

* [<span data-ttu-id="e4828-114">開發 Python 應用程式來存取和管理 Azure Cosmos DB SQL API 帳戶中儲存的資料</span><span class="sxs-lookup"><span data-stu-id="e4828-114">Develop a Python app to access and manage data stored in Azure Cosmos DB SQL API account</span></span>](https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git)

* [<span data-ttu-id="e4828-115">開發 Python 應用程式來存取和管理 Azure Cosmos DB MongoDB API 帳戶中儲存的資料</span><span class="sxs-lookup"><span data-stu-id="e4828-115">Develop a Python app to access and manage data stored in Azure Cosmos DB MongoDB API account</span></span>](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git)

* [<span data-ttu-id="e4828-116">開發 Python 應用程式來存取和管理 Azure Cosmos DB Gremlin API 帳戶中儲存的資料</span><span class="sxs-lookup"><span data-stu-id="e4828-116">Develop a Python app to access and manage data stored in Azure Cosmos DB Gremlin API account</span></span>](https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git)

* [<span data-ttu-id="e4828-117">開發 Python 應用程式來存取和管理 Azure Cosmos DB Cassandra API 帳戶中儲存的資料</span><span class="sxs-lookup"><span data-stu-id="e4828-117">Develop a Python app to access and manage data stored in Azure Cosmos DB Cassandra API account</span></span>](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git)

* [<span data-ttu-id="e4828-118">開發 Python 應用程式來存取和管理 Azure Cosmos DB 資料表 API 帳戶中儲存的資料</span><span class="sxs-lookup"><span data-stu-id="e4828-118">Develop a Python app to access and manage data stored in Azure Cosmos DB Table API account</span></span>](https://github.com/Azure-Samples/storage-python-getting-started.git)


