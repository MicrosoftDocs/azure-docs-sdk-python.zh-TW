---
title: "適用於 Python 的 Azure CosmosDB 程式庫"
description: "適用於 CosmosDB 之 Python 用戶端程式庫的參考文件"
keywords: "Azure, Python, SDK, API, SQL, 資料庫, PostGres, CosmosDB, NoSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/11/2017
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: d56dd69f4fc4513034046f9f721608ad94ff5cfe
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="azure-cosmosdb-libraries-for-python"></a><span data-ttu-id="919c5-104">適用於 Python 的 Azure CosmosDB 程式庫</span><span class="sxs-lookup"><span data-stu-id="919c5-104">Azure CosmosDB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="919c5-105">概觀</span><span class="sxs-lookup"><span data-stu-id="919c5-105">Overview</span></span>

<span data-ttu-id="919c5-106">在 Python 應用程式中使用 CosmosDB 來儲存和查詢 NoSQL 資料存放區中的 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="919c5-106">Use CosmosDB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="919c5-107">深入了解 [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction)。</span><span class="sxs-lookup"><span data-stu-id="919c5-107">Learn more about [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="919c5-108">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="919c5-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="919c5-109">管理程式庫</span><span class="sxs-lookup"><span data-stu-id="919c5-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="919c5-110">範例</span><span class="sxs-lookup"><span data-stu-id="919c5-110">Example</span></span>

<span data-ttu-id="919c5-111">使用類似 SQL 的查詢介面在 CosmosDB 中尋找相符的文件：</span><span class="sxs-lookup"><span data-stu-id="919c5-111">Find matching documents in CosmosDB using a SQL-like query interface:</span></span>

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python DocumentDB client
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
> [<span data-ttu-id="919c5-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="919c5-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a><span data-ttu-id="919c5-113">範例</span><span class="sxs-lookup"><span data-stu-id="919c5-113">Samples</span></span>

[<span data-ttu-id="919c5-114">使用 Azure Cosmos DB 的 DocumentDB API 開發 Python 應用程式</span><span class="sxs-lookup"><span data-stu-id="919c5-114">Develop a Python app using Azure Cosmos DB's DocumentDB API</span></span>](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


