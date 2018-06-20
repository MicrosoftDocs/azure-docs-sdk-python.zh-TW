---
title: 適用於 Python 的 Azure PostgreSQL 程式庫
description: ''
keywords: Azure, Python, SDK, API, SQL, 資料庫, Postgres, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: cad5995072d5040764986765d9a900f46f5141ec
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478931"
---
#<a name="azure-postgresql-libraries-for-python"></a>適用於 Python 的 Azure PostgreSQL 程式庫

## <a name="overview"></a>概觀
使用 ODBC 驅動程式和 pyodbc 來連線到資料庫，並直接執行 SQL 陳述式。

深入了解[適用於 PostgreSQL 的 Azure 資料庫](https://docs.microsoft.com/azure/postgresql/)。

## <a name="client-odbc-driver-and-pyodbc"></a>Client ODBC 驅動程式和 pyodbc
用於存取 Azure Database for PostgreSQL 的建議用戶端程式庫是 Microsoft [ODBC 驅動程式和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)

### <a name="example"></a>範例 

連線到 Azure Database for PostgreSQL 並選取 `SALES` 資料表中的所有記錄。 您可以從 Azure 入口網站取得資料庫的 ODBC 連接字串。

```python
import pyodbc

SERVER = 'YOUR_SERVER_NAME.postgres.database.azure.com'
DATABASE = 'YOUR_DB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

DRIVER = '{PostgreSQL ODBC Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=5432;SERVER=' + SERVER +
    ';PORT=5432;DATABASE=' + DATABASE + ';UID=' + USERNAME +
    ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES" # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>管理 API
### <a name="requirements"></a>需求
您必須安裝適用於 Python 的 PostgreSQL 管理程式庫。
```bash
pip install azure-mgmt-rdbms
```

如需取得認證以驗證管理用戶端的詳細資料，請參閱 [Python SDK 驗證](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)頁面。

### <a name="example"></a>範例
在此範例中，我們將在現有的 Postgres 伺服器上建立新的 Postgres 資料庫。
```python
from azure.mgtm.rdbms.postgresql import PostgreSQLManagementClient

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE_GROUP_WITH_POSTGRES"
POSTGRES_SERVER = "YOUR_POSTGRES_SERVER_NAME"
DB_NAME = "YOUR_DESIRED_DATABASE_NAME"

client = PostgreSQLManagementClient(credentials, SUBSCRIPTION_ID)

db_creation_poller = client.databases.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=POSTGRES_SERVER, database_name=DB_NAME)
db = db_creation_poller.result()
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/postgresql/management)

