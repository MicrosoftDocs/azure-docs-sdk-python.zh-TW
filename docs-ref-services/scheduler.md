---
title: 適用於 Python 的 Azure 排程器程式庫
description: 適用於 Python 的 Azure 排程器程式庫參考
keywords: Azure, python, SDK, API, 排程器
author: lisawong19
ms.author: routlaw
manager: mbaldwin
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 73c33ff808212fade192ca7c7c05a8e64d3fafda
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534216"
---
# <a name="azure-scheduler-libraries-for-python"></a><span data-ttu-id="af420-104">適用於 Python 的 Azure 排程器程式庫</span><span class="sxs-lookup"><span data-stu-id="af420-104">Azure Scheduler libraries for python</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="af420-105">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="af420-105">Install the libraries</span></span>

## <a name="management"></a><span data-ttu-id="af420-106">管理性</span><span class="sxs-lookup"><span data-stu-id="af420-106">Management</span></span>

```bash
pip install azure-mgmt-scheduler
```
## <a name="example"></a><span data-ttu-id="af420-107">範例</span><span class="sxs-lookup"><span data-stu-id="af420-107">Example</span></span>

### <a name="create-the-management-client"></a><span data-ttu-id="af420-108">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="af420-108">Create the management client</span></span>

<span data-ttu-id="af420-109">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="af420-109">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="af420-110">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="af420-110">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="af420-111">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="af420-111">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.scheduler import SchedulerManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

scheduler_client = SchedulerManagementClient(
    credentials,
    subscription_id
)
```

### <a name="create-a-job-collection"></a><span data-ttu-id="af420-112">建立作業集合</span><span class="sxs-lookup"><span data-stu-id="af420-112">Create a job collection</span></span>

<span data-ttu-id="af420-113">下列程式碼會在現有資源群組底下建立作業集合。</span><span class="sxs-lookup"><span data-stu-id="af420-113">The following code creates a job collection under an existing resource group.</span></span>
<span data-ttu-id="af420-114">若要建立或管理資源群組，請參閱[資源管理](/python/api/overview/azure/azure.mgmt.resource)。</span><span class="sxs-lookup"><span data-stu-id="af420-114">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.scheduler.models import JobCollectionDefinition, JobCollectionProperties, Sku

group_name = 'myresourcegroup'
job_collection_name = "myjobcollection"
scheduler_client.job_collections.create_or_update(
    group_name,
    job_collection_name,
    JobCollectionDefinition(
        location = "West US",
        properties = JobCollectionProperties(
            sku = Sku(
                name="Free"
            )
        )
    )
)
# scheduler_client is a JobCollectionDefinition instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="af420-115">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="af420-115">Explore the Management APIs</span></span>](/python/api/overview/azure/scheduler/management)