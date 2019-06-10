---
title: 受控磁碟
description: 建立、調整大小並更新受控磁碟。
author: lisawong19
manager: douge
ms.assetid: ''
ms.devlang: python
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 6/15/2017
ms.author: liwong
ms.openlocfilehash: bee17efdb90d6365acb2adbf9c01d1f7e843da42
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376852"
---
# <a name="managed-disks"></a>受控磁碟

Azure 受控磁碟提供簡化的磁碟管理、增強的延展性、更佳的安全性和調整。 它不會採用磁碟的儲存體帳戶概念，讓使用者能夠進行調整，而無需擔心與儲存體帳戶相關聯的限制。 這篇文章會提供關於使用來自 Python 之服務的快速簡介和參考。

從開發人員的觀點而言，Azure CLI 中的受控磁碟體驗是其他跨平台工具的 CLI 體驗之慣用語。 您可以使用 [Azure Python](https://azure.microsoft.com/develop/python/) SDK 和 [azure-mgmt-compute 套件 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) 來管理受控磁碟。 您可以使用此[教學課程](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)來建立計算用戶端。

## <a name="standalone-managed-disks"></a>獨立的受控磁碟

您可以各種不同的方式輕鬆建立獨立的受控磁碟。

### <a name="create-an-empty-managed-disk"></a>建立空的受控磁碟

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>從 Blob 儲存體建立受控磁碟

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>從您自己的映像建立受控磁碟

```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')
async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = async_creation.result()
```

## <a name="virtual-machine-with-managed-disks"></a>使用受控磁碟的虛擬機器

您可以使用特定磁碟映像的隱含受控磁碟來建立虛擬機器。 使用受控磁碟的隱含建立可簡化建立，而無須指定所有的磁碟詳細資料。 您不必擔心建立及管理儲存體帳戶。

從 Azure 中的作業系統映像建立 VM 時，會隱含地建立受控磁碟。 在 ``storage_profile`` 參數中，``os_disk`` 現在是選擇性的，而且不需要建立儲存體帳戶作為必要的先決條件就可以建立虛擬機器。

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
```

這個 ``storage_profile`` 參數現在是有效的。 若要取得有關如何在 Python (包括網路等等) 中建立 VM 的完整範例，請查看完整的 [Python 中 VM 教學課程](https://github.com/Azure-Samples/virtual-machines-python-manage)。

您可以輕鬆地連結先前已佈建的受控磁碟。

```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})
async_update = compute_client.virtual_machines.create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>使用受控磁碟的虛擬機器擴展集

在受控磁碟之前，您必須針對擴展集內需要的所有 VM，以手動方式建立儲存體帳戶，然後使用 list 參數 ``vhd_containers`` 向擴展集 RestAPI 提供所有儲存體帳戶名稱。 可在 `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>` 文章中取得官方轉換指南。

現在透過受控磁碟，您就完全不需要管理任何儲存體帳戶。 如果您習慣使用 VMSS Python SDK，``storage_profile`` 現在可以與建立 VM 時所用的完全相同：

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

完整範例為：

```python
naming_infix = "PyTestInfix"
vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    } 
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
```

## <a name="other-operations-with-managed-disks"></a>使用受控磁碟的其他作業

### <a name="resizing-a-managed-disk"></a>調整受控磁碟大小

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>更新受控磁碟的儲存體帳戶類型

```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-nlob-storage"></a>從 Blob 儲存體建立映像

```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>建立目前連結至虛擬機器的受控磁碟快照集

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
async_snapshot_creation = self.compute_client.snapshots.create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```
