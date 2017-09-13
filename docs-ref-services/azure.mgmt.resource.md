---
title: "適用於 Python 的 Azure 資源庫"
description: 
keywords: "Azure, Python, SDK, API, 資源"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: 32e13bee27db091f0bca12c7d9ae4fc62165f4c0
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-resources-libraries-for-python"></a>適用於 Python 的 Azure 資源庫 

## <a name="overview"></a>概觀 
在資源群組中管理 Azure 資源。

| Package  |  說明 |
|---|---|
|[azure.mgmt.resource.features][1]|Azure 功能控制項 (AFEC) 提供一種機制，讓資源提供者可控制要向使用者曝光的功能。|
|[azure.mgmt.resource.links][2]|可將 Azure 資源連結在一起來構成邏輯關聯性。 您可以在屬於不同資源群組的資源之間建立連結。|
|[azure.mgmt.resource.locks][3]|可以鎖定 Azure 資源來防止貴組織中的其他使用者刪除或修改資源。|
|[azure.mgmt.resource.managedapplications][4]|Azure 受管理應用程式 (設備)。|
|[azure.mgmt.resource.policy][5]|若要管理和控制資源的存取權，您可以定義自訂的原則，並在範圍內將它們加以指派。|
|[azure.mgmt.resource.resources][6]| 提供使用資源與資源群組的作業。|
|[azure.mgmt.resource.subscriptions][7]|所有資源群組和資源都在訂用帳戶內。 這些作業可讓您取得訂用帳戶和租用戶的相關資訊。|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a>安裝程式庫 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a>範例
下列範例說明如何建立資源群組。 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

深入探索可在應用程式中使用的 [Python 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=python)。 