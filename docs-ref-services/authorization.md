---
title: 適用於 Python 的 Azure 授權庫
description: 適用於 Python 的 Azure 授權庫參考
keywords: Azure, python, SDK, API, 授權
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: ee562614e9745cdc38ae427728df16c117ff80cf
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376719"
---
# <a name="azure-authorization-libraries-for-python"></a>適用於 Python 的 Azure 授權庫

## <a name="management-apipythonapioverviewazureauthorizationmanagement"></a>[管理 API](/python/api/overview/azure/authorization/management)

```bash
pip install azure-mgmt-authorization
```

## <a name="create-the-management-client"></a>建立管理用戶端

下列程式碼會建立管理用戶端的執行個體。

您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。

請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。

```python
from azure.mgmt.authorization import AuthorizationManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password'       # Your password
)

authorization_client = AuthorizationManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-permissions-for-a-resource-group"></a>檢查資源群組的權限

下列程式碼會檢查指定資源群組中的權限。 若要建立或管理資源群組，請參閱[資源管理](/python/api/overview/azure/azure.mgmt.resource)。

```python
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

group_name = 'myresourcegroup'
permissions = self.authorization_client.permissions.list_for_resource_group(
    group_name
)
# permissions is a iterable of Permissions instances
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/authorization/management)
