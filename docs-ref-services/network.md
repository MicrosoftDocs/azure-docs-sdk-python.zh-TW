---
title: 適用於 Python 的 Azure 網路程式庫
description: 適用於 Python 的 Azure 網路程式庫參考
keywords: Azure, python, SDK, API, 網路
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f468f844becc2f0fe07d892c725c00df4669f4e3
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "52273024"
---
# <a name="azure-network-libraries-for-python"></a><span data-ttu-id="47dca-104">適用於 Python 的 Azure 網路程式庫</span><span class="sxs-lookup"><span data-stu-id="47dca-104">Azure Network libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="47dca-105">概觀</span><span class="sxs-lookup"><span data-stu-id="47dca-105">Overview</span></span>

<span data-ttu-id="47dca-106">[Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)可讓您連線 Azure 資源，還能將它們連線至內部部署網路。</span><span class="sxs-lookup"><span data-stu-id="47dca-106">[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) allows you to connect Azure resources, and also connect them to your on-premises network.</span></span>

<span data-ttu-id="47dca-107">若要開始使用 Azure 虛擬網路，請參閱[建立您的第一個虛擬網路](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。</span><span class="sxs-lookup"><span data-stu-id="47dca-107">To get started with Azure Virtual Network, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-apis"></a><span data-ttu-id="47dca-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="47dca-108">Management APIs</span></span>

<span data-ttu-id="47dca-109">使用管理 API 來檢查、管理及設定 Azure 虛擬網路。</span><span class="sxs-lookup"><span data-stu-id="47dca-109">Inspect, manage, and configure Azure virtual networks with the management APIs.</span></span>

<span data-ttu-id="47dca-110">不同於其他 Azure Python API，網路 API 會明確地建立版本以分成個別套件。</span><span class="sxs-lookup"><span data-stu-id="47dca-110">Unlike other Azure python APIs, the networking APIs are explicitly versioned into separage packages.</span></span> <span data-ttu-id="47dca-111">您不需要將它們個別匯入，因為套件資訊會在用戶端建構函式中加以指定。</span><span class="sxs-lookup"><span data-stu-id="47dca-111">You do not need to import them individually since the package information is specified in the client constructor.</span></span>

<span data-ttu-id="47dca-112">使用 pip 安裝管理套件。</span><span class="sxs-lookup"><span data-stu-id="47dca-112">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-network
```

### <a name="example"></a><span data-ttu-id="47dca-113">範例</span><span class="sxs-lookup"><span data-stu-id="47dca-113">Example</span></span>

<span data-ttu-id="47dca-114">建立虛擬網路和相關聯的子網路。</span><span class="sxs-lookup"><span data-stu-id="47dca-114">Create a virtual network and an associated subnet.</span></span>

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
> [<span data-ttu-id="47dca-115">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="47dca-115">Explore the Management APIs</span></span>](/python/api/overview/azure/network/management)

### <a name="samples"></a><span data-ttu-id="47dca-116">範例</span><span class="sxs-lookup"><span data-stu-id="47dca-116">Samples</span></span>

* [<span data-ttu-id="47dca-117">在 Python 中開始使用適用於負載平衡器的 Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="47dca-117">Getting started with Azure Resource Manager for load balancers in Python</span></span>](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)

<span data-ttu-id="47dca-118">檢視 Azure 虛擬網路範例的[完整清單](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network)。</span><span class="sxs-lookup"><span data-stu-id="47dca-118">View the [complete list](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) of Azure Virtual Network samples.</span></span>
