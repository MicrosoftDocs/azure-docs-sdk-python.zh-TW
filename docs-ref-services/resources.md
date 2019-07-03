---
title: 適用於 Python 的 Azure 資源庫
description: 適用於 Python 的 Azure 資源庫參考
keywords: Azure, python, SDK, API, 資源
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: cef1f2bad7dcb3ff73aeae9c56000fb949541df9
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534220"
---
# <a name="azure-resources-libraries-for-python"></a>適用於 Python 的 Azure 資源庫

## <a name="overview"></a>概觀 
使用 [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) 部署、監視和管理群組中的資源。

## <a name="management-api"></a>管理 API
使用管理 API 來透過範本建立資源群組及部署資源。

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a>範例 
在 Azure 的美國東部區域建立新的資源群組。

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a>範例
[管理 Azure 資源和資源群組](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

檢視 Azure Resource Manager 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)。
