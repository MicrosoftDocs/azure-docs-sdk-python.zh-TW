---
title: "適用於 Python 的 Azure 虛擬機器程式庫"
description: 
keywords: "Azure, Python, SDK, API, 計算, 虛擬機器"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: c4128dae1c1fd47d2ac34b178b7e1031aa14c948
ms.sourcegitcommit: 1229121faaae8536a7d8cc89cddd24abf1e30cb8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="56d5b-103">Azure 虛擬機器程式庫</span><span class="sxs-lookup"><span data-stu-id="56d5b-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="56d5b-104">概觀</span><span class="sxs-lookup"><span data-stu-id="56d5b-104">Overview</span></span>

<span data-ttu-id="56d5b-105">執行 Linux 或 Windows 並可視需要擴充的計算資源。</span><span class="sxs-lookup"><span data-stu-id="56d5b-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="56d5b-106">若要開始使用 Azure 虛擬機器，請參閱[使用 Azure 入口網站建立 Linux 虛擬機器](/azure/virtual-machines/linux/quick-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="56d5b-106">To get started with Azure Virtual Machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="56d5b-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="56d5b-107">Management API</span></span>

<span data-ttu-id="56d5b-108">從程式碼使用管理 API 在 Azure 中建立、設定、管理及調整 Windows 和 Linux 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="56d5b-108">Create, configure, manage and scale Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="56d5b-109">透過 pip 安裝程式庫。</span><span class="sxs-lookup"><span data-stu-id="56d5b-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a><span data-ttu-id="56d5b-110">範例</span><span class="sxs-lookup"><span data-stu-id="56d5b-110">Example</span></span>

<span data-ttu-id="56d5b-111">在現有的 Azure 資源群組中，使用受管理服務身分識別 (MSI) 驗證建立新的 Linux 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="56d5b-111">Create a new Linux virtual machine in an existing Azure resource group with Managed Service Identity(MSI) authentication.</span></span>

```python
VM_PARAMETERS={
        'location': 'LOCATION',
        'os_profile': {
            'computer_name': 'VM_NAME',
            'admin_username': 'USERNAME',
            'admin_password': 'PASSWORD'
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': 'Canonical',
                'offer': 'UbuntuServer',
                'sku': '16.04.0-LTS',
                'version': 'latest'
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': 'NIC_ID',
            }]
        },
    }

def create_vm()
    compute_client.virtual_machines.create_or_update(
        'RESOURCE_GROUP_NAME', 'VM_NAME', VM_PARAMETERS)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="56d5b-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="56d5b-112">Explore the Management APIs</span></span>](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a><span data-ttu-id="56d5b-113">範例</span><span class="sxs-lookup"><span data-stu-id="56d5b-113">Samples</span></span>

* <span data-ttu-id="56d5b-114">[管理虛擬機器][1]</span><span class="sxs-lookup"><span data-stu-id="56d5b-114">[Manage virtual machines][1]</span></span>
* <span data-ttu-id="56d5b-115">[使用受管理服務身分識別進行驗證][2]</span><span class="sxs-lookup"><span data-stu-id="56d5b-115">[Authenticate with Managed Service Identity][2]</span></span>
* <span data-ttu-id="56d5b-116">[使用受管理的服務識別延伸模組來建立虛擬機器][3]</span><span class="sxs-lookup"><span data-stu-id="56d5b-116">[Create a virtual machine with Managed Service Identity Extension][3]</span></span>
* <span data-ttu-id="56d5b-117">[管理負載平衡器][4]</span><span class="sxs-lookup"><span data-stu-id="56d5b-117">[Manage a load balancer][4]</span></span>
* <span data-ttu-id="56d5b-118">[建立及設定受控磁碟][5]</span><span class="sxs-lookup"><span data-stu-id="56d5b-118">[Create and configure managed disks][5]</span></span>
* <span data-ttu-id="56d5b-119">[列出映像][6]</span><span class="sxs-lookup"><span data-stu-id="56d5b-119">[List images][6]</span></span> 
* <span data-ttu-id="56d5b-120">[監視虛擬機器][7]</span><span class="sxs-lookup"><span data-stu-id="56d5b-120">[Monitor virtual machines][7]</span></span>

<span data-ttu-id="56d5b-121">檢視虛擬機器範本的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)。</span><span class="sxs-lookup"><span data-stu-id="56d5b-121">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) of virtual machine samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://github.com/Azure-Samples/resource-manager-python-manage-resources-with-msi
[3]: https://github.com/Azure-Samples/compute-python-msi-vm
[4]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[6]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[7]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md