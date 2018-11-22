---
title: 適用於 Python 的 Azure 資源庫
description: 適用於 Python 的 Azure 資源庫參考
keywords: Azure, python, SDK, API, 資源
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f331a88ceac73449c9c1aff7bccbbaff4c4ba7be
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277375"
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="f12bc-104">適用於 Python 的 Azure 資源庫</span><span class="sxs-lookup"><span data-stu-id="f12bc-104">Azure Resources libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="f12bc-105">概觀</span><span class="sxs-lookup"><span data-stu-id="f12bc-105">Overview</span></span> 
<span data-ttu-id="f12bc-106">使用 [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) 部署、監視和管理群組中的資源。</span><span class="sxs-lookup"><span data-stu-id="f12bc-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="f12bc-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="f12bc-107">Management API</span></span>
<span data-ttu-id="f12bc-108">使用管理 API 來透過範本建立資源群組及部署資源。</span><span class="sxs-lookup"><span data-stu-id="f12bc-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a><span data-ttu-id="f12bc-109">範例</span><span class="sxs-lookup"><span data-stu-id="f12bc-109">Example</span></span> 
<span data-ttu-id="f12bc-110">在 Azure 的美國東部區域建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="f12bc-110">Create a new resource group in the Azure Eastern US region.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="f12bc-111">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="f12bc-111">Explore the Management APIs</span></span>](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a><span data-ttu-id="f12bc-112">範例</span><span class="sxs-lookup"><span data-stu-id="f12bc-112">Samples</span></span>
[<span data-ttu-id="f12bc-113">管理 Azure 資源和資源群組</span><span class="sxs-lookup"><span data-stu-id="f12bc-113">Manage Azure resources and resource groups</span></span>](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

<span data-ttu-id="f12bc-114">檢視 Azure Resource Manager 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)。</span><span class="sxs-lookup"><span data-stu-id="f12bc-114">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) of Azure Resource Manager samples.</span></span>
