---
title: "適用於 Python 的 Azure SQL Database 程式庫"
description: 
keywords: "Azure, Python, SDK, API, SQL, 資料庫, pyodbc"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: b580c5011412bc77fd8fd55b709a305be07e2316
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-sql-database-libraries-for-python"></a>適用於 Python 的 Azure SQL Database 程式庫

## <a name="overview"></a>概觀

透過 Microsoft ODBC 驅動程式與 pyodbc，從 Python 使用儲存在 [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) 的資料。 

## <a name="client-odbc-driver-and-pyodbc"></a>Client ODBC 驅動程式和 pyodbc

```bash
pip install pyodbc
```
可在[這裡](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)找到更多關於安裝 Python 和資料庫通訊程式庫的詳細資料。

### <a name="example"></a>範例

連線到 SQL Database 並選取資料表中的所有記錄。

```python
import pyodbc 

SERVER = 'YOUR_SERVER_NAME.database.windows.net'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_DB_USERNAME'
PASSWORD = 'YOUR_DB_PASSWORD'

DRIVER= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER=' + DRIVER + ';PORT=1433;SERVER=' + SERVER +
    ';PORT=1443;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>管理 API

使用管理 API 在訂用帳戶中建立和管理 Azure SQL Database 資源。 

```bash
pip install azure-mgmt-sql
```

### <a name="example"></a>範例

建立 SQL Database 資源，並使用防火牆規則限制只能存取某個 IP 位址範圍。

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/sql/managementlibrary)

## <a name="samples"></a>範例

* [建立和管理 SQL 資料庫][1]    
* [使用 Python 連線並查詢資料][2]   

[1]: https://github.com/Azure-Samples/sql-database-python-manage
[2]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python

檢視 Azure SQL Database 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL)。 