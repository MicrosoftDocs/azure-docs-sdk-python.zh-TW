---
title: 適用於 Python 的 Azure 授權庫
description: 適用於 Python 的 Azure 授權庫參考
keywords: Azure, python, SDK, API, 授權
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: c7c4af34e82f2baad39635ecd20f1b9c48900b41
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534362"
---
# <a name="azure-authorization-libraries-for-python"></a><span data-ttu-id="abdf7-104">適用於 Python 的 Azure 授權庫</span><span class="sxs-lookup"><span data-stu-id="abdf7-104">Azure Authorization libraries for python</span></span>

## <a name="management-apipythonapioverviewazureauthorizationmanagement"></a>[<span data-ttu-id="abdf7-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="abdf7-105">Management API</span></span>](/python/api/overview/azure/authorization/management)

```bash
pip install azure-mgmt-authorization
```

## <a name="create-the-management-client"></a><span data-ttu-id="abdf7-106">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="abdf7-106">Create the management client</span></span>

<span data-ttu-id="abdf7-107">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abdf7-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="abdf7-108">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="abdf7-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="abdf7-109">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="abdf7-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

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

## <a name="check-permissions-for-a-resource-group"></a><span data-ttu-id="abdf7-110">檢查資源群組的權限</span><span class="sxs-lookup"><span data-stu-id="abdf7-110">Check permissions for a resource group</span></span>

<span data-ttu-id="abdf7-111">下列程式碼會檢查指定資源群組中的權限。</span><span class="sxs-lookup"><span data-stu-id="abdf7-111">The following code checks permissions in a given resource group.</span></span> <span data-ttu-id="abdf7-112">若要建立或管理資源群組，請參閱[資源管理](/python/api/overview/azure/azure.mgmt.resource)。</span><span class="sxs-lookup"><span data-stu-id="abdf7-112">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

group_name = 'myresourcegroup'
permissions = self.authorization_client.permissions.list_for_resource_group(
    group_name
)
# permissions is a iterable of Permissions instances
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="abdf7-113">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="abdf7-113">Explore the Management APIs</span></span>](/python/api/overview/azure/authorization/management)
