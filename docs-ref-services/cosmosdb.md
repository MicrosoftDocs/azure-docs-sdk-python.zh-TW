---
title: 適用於 Python 的 Azure Cosmos DB 程式庫
description: 適用於 Azure Cosmos DB 之 Python 用戶端程式庫的參考文件
keywords: Azure, Python, SDK, API, SQL, 資料庫, PostGres, Cosmos DB, NoSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 391b556ece7d818406fa501763814eb7f0d50d22
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "30204135"
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

[使用 Azure Cosmos DB 開發 Python 應用程式](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


