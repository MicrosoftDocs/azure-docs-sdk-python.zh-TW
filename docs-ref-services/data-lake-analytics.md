---
title: 適用於 Python 的 Azure Data Lake Analytics 程式庫
description: 適用於 Python 的 Azure Data Lake Analytics 程式庫參考
keywords: Azure, python, SDK, API, Data Lake Analytics
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 08/04/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: e98b2f314080146429c89061ab5e154526a87a48
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534306"
---
# <a name="azure-data-lake-analytics-libraries-for-python"></a><span data-ttu-id="9e67d-104">適用於 Python 的 Azure Data Lake Analytics 程式庫</span><span class="sxs-lookup"><span data-stu-id="9e67d-104">Azure Data Lake Analytics libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="9e67d-105">概觀</span><span class="sxs-lookup"><span data-stu-id="9e67d-105">Overview</span></span>
<span data-ttu-id="9e67d-106">使用 [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) 來執行擴充至大規模資料集的巨量資料分析作業。</span><span class="sxs-lookup"><span data-stu-id="9e67d-106">Run big data analysis jobs that scale to massive data sets with [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="9e67d-107">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="9e67d-107">Install the libraries</span></span>

## <a name="management-api"></a><span data-ttu-id="9e67d-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="9e67d-108">Management API</span></span>
<span data-ttu-id="9e67d-109">使用管理 API 來管理 Data Lake Analytics 帳戶、作業、原則和目錄。</span><span class="sxs-lookup"><span data-stu-id="9e67d-109">Use the management API to manage Data Lake Analytics accounts, jobs, policies, and catalogs.</span></span>

```bash
pip install azure-mgmt-datalake-analytics
```

### <a name="example"></a><span data-ttu-id="9e67d-110">範例</span><span class="sxs-lookup"><span data-stu-id="9e67d-110">Example</span></span>
<span data-ttu-id="9e67d-111">這個範例說明如何建立 Data Lake Analytics 帳戶並提交作業。</span><span class="sxs-lookup"><span data-stu-id="9e67d-111">This is an example of how to create a Data Lake Analytics account and submit a job.</span></span> 

```python
## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adls = '<Azure Data Lake Analytics Account Name>'

# Create the clients
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')

# Create resource group
armGroupResult = resourceClient.resource_groups.create_or_update(rg, ResourceGroup(location=location))

# Create a store account
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()

# Create an ADLA account
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()

# Submit a job
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9e67d-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="9e67d-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a><span data-ttu-id="9e67d-113">範例</span><span class="sxs-lookup"><span data-stu-id="9e67d-113">Samples</span></span>
[<span data-ttu-id="9e67d-114">管理 Azure Data Lake Anyalytics</span><span class="sxs-lookup"><span data-stu-id="9e67d-114">Manage Azure Data Lake Anyalytics</span></span>](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-manage-use-python-sdk)