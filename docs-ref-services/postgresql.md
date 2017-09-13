---
title: "適用於 Python 的 Azure PostgreSQL 程式庫"
description: 
keywords: "Azure, Python, SDK, API, SQL, 資料庫, Postgres, PostgreSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: e184efc276fb4e6d86504ab44e47340ce72e7006
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
#<a name="azure-postgresql-libraries-for-python"></a><span data-ttu-id="63f9a-103">適用於 Python 的 Azure PostgreSQL 程式庫</span><span class="sxs-lookup"><span data-stu-id="63f9a-103">Azure PostgreSQL libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="63f9a-104">概觀</span><span class="sxs-lookup"><span data-stu-id="63f9a-104">Overview</span></span>
<span data-ttu-id="63f9a-105">使用 ODBC 驅動程式和 pyodbc 來連線到資料庫，並直接執行 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="63f9a-105">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="63f9a-106">深入了解[適用於 PostgreSQL 的 Azure 資料庫](https://docs.microsoft.com/azure/postgresql/)。</span><span class="sxs-lookup"><span data-stu-id="63f9a-106">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="63f9a-107">Client ODBC 驅動程式和 pyodbc</span><span class="sxs-lookup"><span data-stu-id="63f9a-107">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="63f9a-108">用於存取 Azure Database for PostgreSQL 的建議用戶端程式庫是 Microsoft [ODBC 驅動程式和 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span><span class="sxs-lookup"><span data-stu-id="63f9a-108">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

### <a name="example"></a><span data-ttu-id="63f9a-109">範例</span><span class="sxs-lookup"><span data-stu-id="63f9a-109">Example</span></span> 

<span data-ttu-id="63f9a-110">連線到 Azure Database for PostgreSQL 並選取 `SALES` 資料表中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="63f9a-110">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="63f9a-111">您可以從 Azure 入口網站取得資料庫的 ODBC 連接字串。</span><span class="sxs-lookup"><span data-stu-id="63f9a-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="63f9a-112">管理 API</span><span class="sxs-lookup"><span data-stu-id="63f9a-112">Management API</span></span>
### <a name="requirements"></a><span data-ttu-id="63f9a-113">需求</span><span class="sxs-lookup"><span data-stu-id="63f9a-113">Requirements</span></span>
<span data-ttu-id="63f9a-114">您必須安裝適用於 Python 的 PostgreSQL 管理程式庫。</span><span class="sxs-lookup"><span data-stu-id="63f9a-114">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="63f9a-115">如需取得認證以驗證管理用戶端的詳細資料，請參閱 [Python SDK 驗證](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)頁面。</span><span class="sxs-lookup"><span data-stu-id="63f9a-115">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="63f9a-116">範例</span><span class="sxs-lookup"><span data-stu-id="63f9a-116">Example</span></span>
<span data-ttu-id="63f9a-117">在此範例中，我們將在現有的 Postgres 伺服器上建立新的 Postgres 資料庫。</span><span class="sxs-lookup"><span data-stu-id="63f9a-117">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
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
> [<span data-ttu-id="63f9a-118">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="63f9a-118">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/managementlibrary)

