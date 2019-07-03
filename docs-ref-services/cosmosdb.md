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
# <a name="azure-cosmos-db-libraries-for-python"></a>適用於 Python 的 Azure Cosmos DB 程式庫

## <a name="overview"></a>概觀

在 Python 應用程式中使用 Azure Cosmos DB 來儲存和查詢 NoSQL 資料存放區中的 JSON 文件。

深入了解 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)。

## <a name="client-library"></a>用戶端程式庫
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>管理程式庫
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>範例

使用類似 SQL 的查詢介面在 Azure CosmosDB 中尋找相符的文件：

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
> [探索管理 API](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a>範例

* [開發 Python 應用程式來存取和管理 Azure Cosmos DB SQL API 帳戶中儲存的資料](https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git)

* [開發 Python 應用程式來存取和管理 Azure Cosmos DB MongoDB API 帳戶中儲存的資料](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git)

* [開發 Python 應用程式來存取和管理 Azure Cosmos DB Gremlin API 帳戶中儲存的資料](https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git)

* [開發 Python 應用程式來存取和管理 Azure Cosmos DB Cassandra API 帳戶中儲存的資料](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git)

* [開發 Python 應用程式來存取和管理 Azure Cosmos DB 資料表 API 帳戶中儲存的資料](https://github.com/Azure-Samples/storage-python-getting-started.git)


