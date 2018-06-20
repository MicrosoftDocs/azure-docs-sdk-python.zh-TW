---
title: 適用於 Python 的 Azure Batch 程式庫
description: Python Batch 程式庫的參考文件
keywords: Azure, Python, SDK, API, Batch, 流程, 排程, 長時間執行
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: de5f3a98b1712ff9bdcc417daf10719178819364
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478981"
---
# <a name="azure-batch-libraries-for-python"></a>適用於 Python 的 Azure Batch 程式庫

## <a name="overview"></a>概觀

使用 [Azure Batch](/azure/batch/batch-technical-overview) 在雲端有效地執行大規模的平行和高效能計算應用程式。   

若要開始使用 Azure Batch，請參閱[使用 Azure 入口網站建立 Batch 帳戶](/azure/batch/batch-account-create-portal)。

## <a name="install-the-libraries"></a>安裝程式庫

## <a name="client-library"></a>用戶端程式庫
Azure Batch 用戶端程式庫可讓您設定計算節點和集區、定義工作並將其設定為在作業中執行，以及設定作業管理員以控制和監控作業的執行。 [深入了解](/azure/batch/batch-api-basics)如何使用這些物件以執行大規模的平行計算解決方案。

```bash
pip install azure-batch
```
### <a name="example"></a>範例

在 Batch 帳戶中設定 Linux 計算節點集區：

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

## <a name="management-api"></a>管理 API
使用 Azure Batch 管理程式庫來建立和刪除 Batch 帳戶、讀取和重新產生 Batch 帳戶金鑰，以及管理 Batch 帳戶的儲存體。

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [探索用戶端 API](/python/api/overview/azure/batch/client)

### <a name="example"></a>範例
建立 Azure Batch 帳戶，並為其設定新的應用程式和 Azure 儲存體帳戶。

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
> [探索管理 API](/python/api/overview/azure/batch/management)