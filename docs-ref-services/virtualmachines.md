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
ms.openlocfilehash: c25665e19adb44c7112bf1533097ce1e6c739cb8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-virtual-machine-libraries"></a>Azure 虛擬機器程式庫

## <a name="overview"></a>概觀

執行 Linux 或 Windows 並可視需要擴充的計算資源。

若要開始使用 Azure 虛擬機器，請參閱[使用 Azure 入口網站建立 Linux 虛擬機器](/azure/virtual-machines/linux/quick-create-portal)。

## <a name="management-api"></a>管理 API

從程式碼使用管理 API 在 Azure 中建立、設定、管理及調整 Windows 和 Linux 虛擬機器。

透過 pip 安裝程式庫。

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a>範例

在現有的 Azure 資源群組中建立新的 Linux 虛擬機器。

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
> [探索管理 API](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a>範例

* [管理虛擬機器][1]
* [管理負載平衡器][2]
* [建立及設定受控磁碟][3]
* [列出映像][4] 
* [監視虛擬機器][5]

檢視虛擬機器範本的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)。

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[3]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[4]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md