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
ms.author: routlaw
ms.openlocfilehash: 8618b42a545e2e36ccca8944ef1dc6cf49966b00
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534411"
---
# <a name="managed-disks"></a><span data-ttu-id="ae699-103">受控磁碟</span><span class="sxs-lookup"><span data-stu-id="ae699-103">Managed Disks</span></span>

<span data-ttu-id="ae699-104">Azure 受控磁碟提供簡化的磁碟管理、增強的延展性、更佳的安全性和調整。</span><span class="sxs-lookup"><span data-stu-id="ae699-104">Azure Managed Disks provide a simplified disk Management, enhanced Scalability, better Security and Scale.</span></span> <span data-ttu-id="ae699-105">它不會採用磁碟的儲存體帳戶概念，讓使用者能夠進行調整，而無需擔心與儲存體帳戶相關聯的限制。</span><span class="sxs-lookup"><span data-stu-id="ae699-105">It takes away the notion of storage account for disks, enabling customers to scale without worrying about the limitations associated with storage accounts.</span></span> <span data-ttu-id="ae699-106">這篇文章會提供關於使用來自 Python 之服務的快速簡介和參考。</span><span class="sxs-lookup"><span data-stu-id="ae699-106">This post provides a quick introduction and reference on consuming the service from Python.</span></span>

<span data-ttu-id="ae699-107">從開發人員的觀點而言，Azure CLI 中的受控磁碟體驗是其他跨平台工具的 CLI 體驗之慣用語。</span><span class="sxs-lookup"><span data-stu-id="ae699-107">From a developer perspective, the Managed Disks experience in Azure CLI is idomatic to the CLI experience in other cross-platform tools.</span></span> <span data-ttu-id="ae699-108">您可以使用 [Azure Python](https://azure.microsoft.com/develop/python/) SDK 和 [azure-mgmt-compute 套件 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) 來管理受控磁碟。</span><span class="sxs-lookup"><span data-stu-id="ae699-108">You can use the [Azure Python](https://azure.microsoft.com/develop/python/) SDK and the [azure-mgmt-compute package 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) to administer Managed Disks.</span></span> <span data-ttu-id="ae699-109">您可以使用此[教學課程](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)來建立計算用戶端。</span><span class="sxs-lookup"><span data-stu-id="ae699-109">You can create a compute client using this [tutorial](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python).</span></span>

## <a name="standalone-managed-disks"></a><span data-ttu-id="ae699-110">獨立的受控磁碟</span><span class="sxs-lookup"><span data-stu-id="ae699-110">Standalone Managed Disks</span></span>

<span data-ttu-id="ae699-111">您可以各種不同的方式輕鬆建立獨立的受控磁碟。</span><span class="sxs-lookup"><span data-stu-id="ae699-111">You can easily create standalone Managed Disks in a variety of ways.</span></span>

### <a name="create-an-empty-managed-disk"></a><span data-ttu-id="ae699-112">建立空的受控磁碟</span><span class="sxs-lookup"><span data-stu-id="ae699-112">Create an empty Managed Disk</span></span>

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

### <a name="create-a-managed-disk-from-blob-storage"></a><span data-ttu-id="ae699-113">從 Blob 儲存體建立受控磁碟</span><span class="sxs-lookup"><span data-stu-id="ae699-113">Create a Managed Disk from blob storage</span></span>

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

### <a name="create-an-image-from-blob-storage"></a><span data-ttu-id="ae699-114">從 Blob 儲存體建立映像</span><span class="sxs-lookup"><span data-stu-id="ae699-114">Create an image from blob storage</span></span>

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.images.create_or_update(
    'my_resource_group',
    'my_image_name',
    {
        'location': 'eastus',
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
image_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a><span data-ttu-id="ae699-115">從您自己的映像建立受控磁碟</span><span class="sxs-lookup"><span data-stu-id="ae699-115">Create a Managed Disk from your own image</span></span>

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

## <a name="virtual-machine-with-managed-disks"></a><span data-ttu-id="ae699-116">使用受控磁碟的虛擬機器</span><span class="sxs-lookup"><span data-stu-id="ae699-116">Virtual machine with Managed Disks</span></span>

<span data-ttu-id="ae699-117">您可以使用特定磁碟映像的隱含受控磁碟來建立虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="ae699-117">You can create a Virtual Machine with an implicit Managed Disk for a specific disk image.</span></span> <span data-ttu-id="ae699-118">使用受控磁碟的隱含建立可簡化建立，而無須指定所有的磁碟詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ae699-118">Creation is simplified with implicit creation of managed disks without specifying all the disk details.</span></span> <span data-ttu-id="ae699-119">您不必擔心建立及管理儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="ae699-119">You do not have to worry about creating and managing Storage Accounts.</span></span>

<span data-ttu-id="ae699-120">從 Azure 中的作業系統映像建立 VM 時，會隱含地建立受控磁碟。</span><span class="sxs-lookup"><span data-stu-id="ae699-120">A Managed Disk is created implicitly when creating VM from an OS image in Azure.</span></span> <span data-ttu-id="ae699-121">在 ``storage_profile`` 參數中，``os_disk`` 現在是選擇性的，而且不需要建立儲存體帳戶作為必要的先決條件就可以建立虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="ae699-121">In the ``storage_profile`` parameter, ``os_disk`` is now optional and you don't have to create a storage account as required precondition to create a Virtual Machine.</span></span>

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

<span data-ttu-id="ae699-122">這個 ``storage_profile`` 參數現在是有效的。</span><span class="sxs-lookup"><span data-stu-id="ae699-122">This ``storage_profile`` parameter is now valid.</span></span> <span data-ttu-id="ae699-123">若要取得有關如何在 Python (包括網路等等) 中建立 VM 的完整範例，請查看完整的 [Python 中 VM 教學課程](https://github.com/Azure-Samples/virtual-machines-python-manage)。</span><span class="sxs-lookup"><span data-stu-id="ae699-123">To get a complete example on how to create a VM in Python (including network, etc), check the full [VM tutorial in Python](https://github.com/Azure-Samples/virtual-machines-python-manage).</span></span>

<span data-ttu-id="ae699-124">您也可以使用自己的映像來建立 ``storage_profile``：</span><span class="sxs-lookup"><span data-stu-id="ae699-124">You can also create a ``storage_profile`` from your own image:</span></span>

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

<span data-ttu-id="ae699-125">您可以輕鬆地連結先前已佈建的受控磁碟。</span><span class="sxs-lookup"><span data-stu-id="ae699-125">You can easily attach a previously provisioned Managed Disk.</span></span>

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

## <a name="virtual-machine-scale-sets-with-managed-disks"></a><span data-ttu-id="ae699-126">使用受控磁碟的虛擬機器擴展集</span><span class="sxs-lookup"><span data-stu-id="ae699-126">Virtual machine Scale Sets with Managed Disks</span></span>

<span data-ttu-id="ae699-127">在受控磁碟之前，您必須針對擴展集內需要的所有 VM，以手動方式建立儲存體帳戶，然後使用 list 參數 ``vhd_containers`` 向擴展集 RestAPI 提供所有儲存體帳戶名稱。</span><span class="sxs-lookup"><span data-stu-id="ae699-127">Before Managed Disks, you needed to create a storage account manually for all the VMs you wanted inside your Scale Set, and then use the list parameter ``vhd_containers`` to provide all the storage account name to the Scale Set RestAPI.</span></span> <span data-ttu-id="ae699-128">可在 `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>` 文章中取得官方轉換指南。</span><span class="sxs-lookup"><span data-stu-id="ae699-128">The official transition guide is available in this article `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`.</span></span>

<span data-ttu-id="ae699-129">現在透過受控磁碟，您就完全不需要管理任何儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="ae699-129">Now with Managed Disk, you don't have to manage any storage account at all.</span></span> <span data-ttu-id="ae699-130">如果您習慣使用 VMSS Python SDK，``storage_profile`` 現在可以與建立 VM 時所用的完全相同：</span><span class="sxs-lookup"><span data-stu-id="ae699-130">If you're are used to the VMSS Python SDK, your ``storage_profile`` can now be exactly the same as the one used in VM creation:</span></span>

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

<span data-ttu-id="ae699-131">完整範例為：</span><span class="sxs-lookup"><span data-stu-id="ae699-131">The full sample being:</span></span>

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

## <a name="other-operations-with-managed-disks"></a><span data-ttu-id="ae699-132">使用受控磁碟的其他作業</span><span class="sxs-lookup"><span data-stu-id="ae699-132">Other operations with Managed Disks</span></span>

### <a name="resizing-a-managed-disk"></a><span data-ttu-id="ae699-133">調整受控磁碟大小</span><span class="sxs-lookup"><span data-stu-id="ae699-133">Resizing a Managed Disk</span></span>

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

### <a name="update-the-storage-account-type-of-the-managed-disks"></a><span data-ttu-id="ae699-134">更新受控磁碟的儲存體帳戶類型</span><span class="sxs-lookup"><span data-stu-id="ae699-134">Update the storage account type of the Managed Disks</span></span>

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

### <a name="create-an-image-from-nlob-storage"></a><span data-ttu-id="ae699-135">從 Blob 儲存體建立映像</span><span class="sxs-lookup"><span data-stu-id="ae699-135">Create an image from nlob storage</span></span>

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

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a><span data-ttu-id="ae699-136">建立目前連結至虛擬機器的受控磁碟快照集</span><span class="sxs-lookup"><span data-stu-id="ae699-136">Create a snapshot of a Managed Disk that is currently attached to a virtual machine</span></span>

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
