---
title: 適用於 Python 的 Azure MySQL/PostgreSQL 程式庫
description: ''
keywords: Azure, Python, SDK, API, SQL, 資料庫, MySQL, PostgreSQL
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.openlocfilehash: 81a29ea16dc9857257859181f0c2e5be8b4b7901
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534230"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a>適用於 Python 的 Azure MySQL/PostgreSQL 程式庫

## <a name="mysql"></a>MySQL

從 Python 透過 MySQL 管理員與 pyodbc 使用儲存在 [Azure MySQL Database](/azure/mysql/overview) 的資源和資料。

### <a name="client-odbc-driver-and-pyodbc"></a>Client ODBC 驅動程式和 pyodbc

用於存取 Azure Database for MySQL 的建議用戶端程式庫是 Microsoft [ODBC 驅動程式](/azure/sql-database/sql-database-connect-query-python#prerequisites)。 使用 ODBC 驅動程式來連線到資料庫並直接執行 SQL 陳述式。

#### <a name="example"></a>範例

連線到 Azure Database for MySQL 並選取銷售資料表中的所有記錄。 您可以從 Azure 入口網站取得資料庫的 ODBC 連接字串。

```python
SERVER = 'YOUR_SEVER_NAME' + '.mysql.database.azure.com'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_MYSQL_USERNAME'
PASSWORD = 'YOUR_MYSQL_PASSWORD'

DRIVER = '{MySQL ODBC 5.3 UNICODE Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=3306;SERVER=' + SERVER + ';PORT=3306;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

### <a name="management-api"></a>管理 API

使用管理 API 在訂用帳戶中建立和管理 MySQL Database 資源。

#### <a name="requirements"></a>需求
您必須安裝適用於 Python 的 MySQL 管理程式庫。
```bash
pip install azure-mgmt-rdbms
```

如需取得認證以驗證管理用戶端的詳細資料，請參閱 [Python SDK 驗證](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)頁面。

#### <a name="example"></a>範例

建立 MySQL 5.7 Database 資源，並使用防火牆規則限制只能存取某個 IP 位址範圍。

```python

from azure.mgmt.rdbms.mysql import MySQLManagementClient
from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE-GROUP_WITH_POSTGRES"
MYSQL_SERVER = "YOUR_DESIRED_MYSQL_SERVER_NAME"
MYSQL_ADMIN_USER = "YOUR_MYSQL_ADMIN_USERNAME"
MYSQL_ADMIN_PASSWORD = "YOUR_MYSQL_ADMIN_PASSOWRD"
LOCATION = "westus"  # example Azure availability zone, should match resource group


client = MySQLManagementClient(credentials=creds,
    subscription_id=SUBSCRIPTION_ID)

server_creation_poller = client.servers.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=MYSQL_SERVER,
    ServerForCreate(
        ServerPropertiesForDefaultCreate(
            administrator_login=MYSQL_ADMIN_USER,
            administrator_login_password=MYSQL_ADMIN_PASSWORD,
            version=ServerVersion.five_full_stop_seven
        ),
        location=LOCATION
    )
)

server = server_creation_poller.result()

# Open access to this server for IPs
rule_creation_poller = client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    MYSQL_SERVER,
    "some_custom_ip_range_whitelist_rule_name",  # Custom firewall rule name
    "123.123.123.123",  # Start ip range
    "167.220.0.235"  # End ip range
)

firewall_rule = rule_creation_poller.result()
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a>PostgreSQL
使用 ODBC 驅動程式和 pyodbc 來連線到資料庫，並直接執行 SQL 陳述式。

深入了解[適用於 PostgreSQL 的 Azure 資料庫](https://docs.microsoft.com/azure/postgresql/)。

### <a name="client-odbc-driver-and-pyodbc"></a>Client ODBC 驅動程式和 pyodbc
建議用於存取適用於 PostgreSQL 的 Azure 資料庫用戶端程式庫是 Microsoft [ODBC 驅動程式和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#prerequisites)。

#### <a name="example"></a>範例 

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

### <a name="management-api"></a>管理 API
#### <a name="requirements"></a>需求
您必須安裝適用於 Python 的 PostgreSQL 管理程式庫。
```bash
pip install azure-mgmt-rdbms
```

如需取得認證以驗證管理用戶端的詳細資料，請參閱 [Python SDK 驗證](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)頁面。

#### <a name="example"></a>範例
在此範例中，我們將在現有的 Postgres 伺服器上建立新的 Postgres 資料庫。
```python
from azure.mgmt.rdbms.postgresql import PostgreSQLManagementClient

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
> [探索管理 API](/python/api/overview/azure/postgresql/mysql/management)
