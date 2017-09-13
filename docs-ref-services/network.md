---
title: "適用於 Python 的 Azure 網路程式庫"
description: "適用於 Python 的 Azure 網路程式庫參考"
keywords: "Azure, python, SDK, API, 網路"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: ed6ddfe4d1f4d860952369a75e10166a1f22c483
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-network-libraries-for-python"></a>適用於 Python 的 Azure 網路程式庫

## <a name="overview"></a>概觀

[Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)可讓您連線 Azure 資源，還能將它們連線至內部部署網路。

若要開始使用 Azure 虛擬網路，請參閱[建立您的第一個虛擬網路](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。

## <a name="management-apis"></a>管理 API

使用管理 API 來檢查、管理及設定 Azure 虛擬網路。

不同於其他 Azure Python API，網路 API 會明確地建立版本以分成個別套件。 您不需要將它們個別匯入，因為套件資訊會在用戶端建構函式中加以指定。

使用 pip 安裝管理套件。

```bash
pip install azure-mgmt-network
```

### <a name="example"></a>範例

建立虛擬網路和相關聯的子網路。

```python
from azure.mgmt.network import NetworkManagementClient

GROUP_NAME = 'resource-group'
VNET_NAME = 'your-vnet-identifier'
LOCATION = 'region'
SUBNET_NAME = 'your-subnet-identifier'

network_client = NetworkManagementClient(credentials, 'your-subscription-id')

async_vnet_creation = network_client.virtual_networks.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    {
        'location': LOCATION,
        'address_space': {
            'address_prefixes': ['10.0.0.0/16']
        }
    }
)
async_vnet_creation.wait()

# Create Subnet
async_subnet_creation = network_client.subnets.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    SUBNET_NAME,
    {'address_prefix': '10.0.0.0/24'}
)
subnet_info = async_subnet_creation.result()
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/network/managementlibrary)

### <a name="samples"></a>範例

* [在 Python 中開始使用適用於負載平衡器的 Azure Resource Manager][1]

檢視 Azure 虛擬網路範例的[完整清單](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network)。

[1]: [https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/]
