---
title: "適用於 Python 的 Azure 資源庫"
description: "適用於 Python 的 Azure 資源庫參考"
keywords: "Azure, python, SDK, API, 資源"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: d924a8aea18d9b59ec8c78d85dce8fe0ce7c8d6c
ms.sourcegitcommit: d521a7350216461eb2fa68152c4975f55152f831
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2017
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
