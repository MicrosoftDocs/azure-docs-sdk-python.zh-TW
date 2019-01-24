---
title: 適用於 Python 的 Azure MySQL/PostgreSQL 程式庫
description: ''
keywords: Azure, Python, SDK, API, SQL, 資料庫, MySQL, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.openlocfilehash: 402e87ae81e6df64b040293992244902313e5b1b
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747718"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a><span data-ttu-id="51f02-103">適用於 Python 的 Azure MySQL/PostgreSQL 程式庫</span><span class="sxs-lookup"><span data-stu-id="51f02-103">Azure MySQL/PostgreSQL libraries for Python</span></span>

## <a name="mysql"></a><span data-ttu-id="51f02-104">MySQL</span><span class="sxs-lookup"><span data-stu-id="51f02-104">MySQL</span></span>

<span data-ttu-id="51f02-105">從 Python 透過 MySQL 管理員與 pyodbc 使用儲存在 [Azure MySQL Database](/azure/mysql/overview) 的資源和資料。</span><span class="sxs-lookup"><span data-stu-id="51f02-105">Work with resources and data stored in [Azure MySQL Database](/azure/mysql/overview) from python with the MySQL manager and pyodbc.</span></span>

### <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="51f02-106">Client ODBC 驅動程式和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="51f02-106">Client ODBC driver and pyodbc</span></span>

<span data-ttu-id="51f02-107">用於存取 Azure Database for MySQL 的建議用戶端程式庫是 Microsoft [ODBC 驅動程式](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)。</span><span class="sxs-lookup"><span data-stu-id="51f02-107">The recommended client library for accessing Azure Database for MySQL is the Microsoft [ODBC driver](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span> <span data-ttu-id="51f02-108">使用 ODBC 驅動程式來連線到資料庫並直接執行 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="51f02-108">Use the ODBC driver to connect to the database and execute SQL statements directly.</span></span>

#### <a name="example"></a><span data-ttu-id="51f02-109">範例</span><span class="sxs-lookup"><span data-stu-id="51f02-109">Example</span></span>

<span data-ttu-id="51f02-110">連線到 Azure Database for MySQL 並選取銷售資料表中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="51f02-110">Connect to a Azure Database for MySQL and select all records in the sales table.</span></span> <span data-ttu-id="51f02-111">您可以從 Azure 入口網站取得資料庫的 ODBC 連接字串。</span><span class="sxs-lookup"><span data-stu-id="51f02-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

### <a name="management-api"></a><span data-ttu-id="51f02-112">管理 API</span><span class="sxs-lookup"><span data-stu-id="51f02-112">Management API</span></span>

<span data-ttu-id="51f02-113">使用管理 API 在訂用帳戶中建立和管理 MySQL Database 資源。</span><span class="sxs-lookup"><span data-stu-id="51f02-113">Create and manage MySQL resources in your subscription with the management API.</span></span>

#### <a name="requirements"></a><span data-ttu-id="51f02-114">需求</span><span class="sxs-lookup"><span data-stu-id="51f02-114">Requirements</span></span>
<span data-ttu-id="51f02-115">您必須安裝適用於 Python 的 MySQL 管理程式庫。</span><span class="sxs-lookup"><span data-stu-id="51f02-115">You must install the MySQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="51f02-116">如需取得認證以驗證管理用戶端的詳細資料，請參閱 [Python SDK 驗證](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)頁面。</span><span class="sxs-lookup"><span data-stu-id="51f02-116">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

#### <a name="example"></a><span data-ttu-id="51f02-117">範例</span><span class="sxs-lookup"><span data-stu-id="51f02-117">Example</span></span>

<span data-ttu-id="51f02-118">建立 MySQL 5.7 Database 資源，並使用防火牆規則限制只能存取某個 IP 位址範圍。</span><span class="sxs-lookup"><span data-stu-id="51f02-118">Create a MySQL 5.7 Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

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
> [<span data-ttu-id="51f02-119">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="51f02-119">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a><span data-ttu-id="51f02-120">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="51f02-120">PostgreSQL</span></span>
<span data-ttu-id="51f02-121">使用 ODBC 驅動程式和 pyodbc 來連線到資料庫，並直接執行 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="51f02-121">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="51f02-122">深入了解[適用於 PostgreSQL 的 Azure 資料庫](https://docs.microsoft.com/azure/postgresql/)。</span><span class="sxs-lookup"><span data-stu-id="51f02-122">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

### <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="51f02-123">Client ODBC 驅動程式和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="51f02-123">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="51f02-124">用於存取 Azure Database for PostgreSQL 的建議用戶端程式庫是 Microsoft [ODBC 驅動程式和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span><span class="sxs-lookup"><span data-stu-id="51f02-124">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

#### <a name="example"></a><span data-ttu-id="51f02-125">範例</span><span class="sxs-lookup"><span data-stu-id="51f02-125">Example</span></span> 

<span data-ttu-id="51f02-126">連線到 Azure Database for PostgreSQL 並選取 `SALES` 資料表中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="51f02-126">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="51f02-127">您可以從 Azure 入口網站取得資料庫的 ODBC 連接字串。</span><span class="sxs-lookup"><span data-stu-id="51f02-127">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

### <a name="management-api"></a><span data-ttu-id="51f02-128">管理 API</span><span class="sxs-lookup"><span data-stu-id="51f02-128">Management API</span></span>
#### <a name="requirements"></a><span data-ttu-id="51f02-129">需求</span><span class="sxs-lookup"><span data-stu-id="51f02-129">Requirements</span></span>
<span data-ttu-id="51f02-130">您必須安裝適用於 Python 的 PostgreSQL 管理程式庫。</span><span class="sxs-lookup"><span data-stu-id="51f02-130">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="51f02-131">如需取得認證以驗證管理用戶端的詳細資料，請參閱 [Python SDK 驗證](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)頁面。</span><span class="sxs-lookup"><span data-stu-id="51f02-131">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

#### <a name="example"></a><span data-ttu-id="51f02-132">範例</span><span class="sxs-lookup"><span data-stu-id="51f02-132">Example</span></span>
<span data-ttu-id="51f02-133">在此範例中，我們將在現有的 Postgres 伺服器上建立新的 Postgres 資料庫。</span><span class="sxs-lookup"><span data-stu-id="51f02-133">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
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
> [<span data-ttu-id="51f02-134">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="51f02-134">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/mysql/management)
