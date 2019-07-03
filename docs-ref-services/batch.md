---
title: 適用於 Python 的 Azure Batch 程式庫
description: Python Batch 程式庫的參考文件
keywords: Azure, Python, SDK, API, Batch, 流程, 排程, 長時間執行
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: bbc691a8db6597c77575900b4e2a06f34ebb179c
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534359"
---
# <a name="azure-batch-libraries-for-python"></a><span data-ttu-id="63c54-104">適用於 Python 的 Azure Batch 程式庫</span><span class="sxs-lookup"><span data-stu-id="63c54-104">Azure Batch libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="63c54-105">概觀</span><span class="sxs-lookup"><span data-stu-id="63c54-105">Overview</span></span>

<span data-ttu-id="63c54-106">使用 [Azure Batch](/azure/batch/batch-technical-overview) 在雲端有效地執行大規模的平行和高效能計算應用程式。</span><span class="sxs-lookup"><span data-stu-id="63c54-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>

<span data-ttu-id="63c54-107">若要開始使用 Azure Batch，請參閱[使用 Azure 入口網站建立 Batch 帳戶](/azure/batch/batch-account-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="63c54-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="63c54-108">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="63c54-108">Install the libraries</span></span>

## <a name="client-library"></a><span data-ttu-id="63c54-109">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="63c54-109">Client library</span></span>
<span data-ttu-id="63c54-110">Azure Batch 用戶端程式庫可讓您設定計算節點和集區、定義工作並將其設定為在作業中執行，以及設定作業管理員以控制和監控作業的執行。</span><span class="sxs-lookup"><span data-stu-id="63c54-110">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="63c54-111">[深入了解](/azure/batch/batch-api-basics)如何使用這些物件以執行大規模的平行計算解決方案。</span><span class="sxs-lookup"><span data-stu-id="63c54-111">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

```bash
pip install azure-batch
```
### <a name="example"></a><span data-ttu-id="63c54-112">範例</span><span class="sxs-lookup"><span data-stu-id="63c54-112">Example</span></span>

<span data-ttu-id="63c54-113">在 Batch 帳戶中設定 Linux 計算節點集區：</span><span class="sxs-lookup"><span data-stu-id="63c54-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```python
# create the batch client for an account using its URI and keys
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

## <a name="management-api"></a><span data-ttu-id="63c54-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="63c54-114">Management API</span></span>
<span data-ttu-id="63c54-115">使用 Azure Batch 管理程式庫來建立和刪除 Batch 帳戶、讀取和重新產生 Batch 帳戶金鑰，以及管理 Batch 帳戶的儲存體。</span><span class="sxs-lookup"><span data-stu-id="63c54-115">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="63c54-116">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="63c54-116">Explore the Client APIs</span></span>](/python/api/overview/azure/batch/client)

### <a name="example"></a><span data-ttu-id="63c54-117">範例</span><span class="sxs-lookup"><span data-stu-id="63c54-117">Example</span></span>
<span data-ttu-id="63c54-118">建立 Azure Batch 帳戶，並為其設定新的應用程式和 Azure 儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="63c54-118">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```python
from azure.mgmt.batch import BatchManagementClient
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

LOCATION ='eastus'
GROUP_NAME ='batchresourcegroup'
STORAGE_ACCOUNT_NAME ='batchstorageaccount'

# Create Resource group
print('Create Resource Group')
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})

# Create a storage account
storage_async_operation = storage_client.storage_accounts.create(
    GROUP_NAME,
    STORAGE_ACCOUNT_NAME,
    StorageAccountCreateParameters(
        sku=Sku(SkuName.standard_ragrs),
        kind=Kind.storage,
        location=LOCATION
    )
)
storage_account = storage_async_operation.result()

# Create a Batch Account, specifying the storage account we want to link
storage_resource = storage_account.id
batch_account = azure.mgmt.batch.models.BatchAccountCreateParameters(
    location=LOCATION,
    auto_storage=azure.mgmt.batch.models.AutoStorageBaseProperties(storage_resource)
)
creating = batch_client.account.create('MyBatchAccount', LOCATION, batch_account)
creating.wait()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="63c54-119">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="63c54-119">Explore the Management APIs</span></span>](/python/api/overview/azure/batch/management)
