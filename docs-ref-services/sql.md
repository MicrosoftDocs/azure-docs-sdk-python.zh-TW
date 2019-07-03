---
title: 適用於 Python 的 Azure SQL Database 程式庫
description: 使用管理 API 透過 JDBC 驅動程式或管理 Azure SQL 執行個體來連線到 Azure SQL 資料庫。
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 01/09/2018
ms.topic: reference
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: 9b8a5b120425fc600f34c1e4c4456b0888814fe8
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534204"
---
# <a name="azure-sql-database-libraries-for-python"></a>適用於 Python 的 Azure SQL Database 程式庫

## <a name="overview"></a>概觀

透過 [ODBC 資料庫驅動程式](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers)從 Python 使用儲存在 [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) 的資料。 檢視我們的[快速入門](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python)以便連線至 Azure SQL 資料庫，並使用 Transact-SQL 陳述式來查詢資料並開始使用 pyodbc [範例](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)。

## <a name="install-odbc-driver-and-pyodbc"></a>安裝 ODBC 驅動程式和 pyodbc

```bash
pip install pyodbc
```
可在[這裡](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#prerequisites)找到更多關於安裝 Python 和資料庫通訊程式庫的詳細資料。

## <a name="connect-and-execute-a-sql-query"></a>連線和執行 SQL 查詢

### <a name="connect-to-a-sql-database"></a>連線到 SQL Database

```python
import pyodbc

server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'

cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
```

### <a name="execute-a-sql-query"></a>執行 SQL 查詢

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [pyodbc 範例](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a>連線至 ORM

pyodbc 可搭配其他 ORM，例如 [SQLAlchemy](https://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) 和 [Django](https://github.com/lionheart/django-pyodbc/)。 

## <a name="management-apipythonapioverviewazuresqlmanagement"></a>[管理 API](/python/api/overview/azure/sql/management)

使用管理 API 在訂用帳戶中建立和管理 Azure SQL Database 資源。 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a>範例

建立 SQL Database 資源，並使用防火牆規則限制只能存取某個 IP 位址範圍。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.sql import SqlManagementClient

RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_SERVER = 'yourvirtualsqlserver'
SQL_DB = 'YOUR_SQLDB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_SERVER,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Create a SQL database in the Basic tier
database = sql_client.databases.create_or_update(
    RESOURCE_GROUP,
    SQL_SERVER,
    SQL_DB,
    {
        'location': LOCATION,
        'collation': 'SQL_Latin1_General_CP1_CI_AS',
        'create_mode': 'default',
        'requested_service_objective_name': 'Basic'
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/sql/management)

