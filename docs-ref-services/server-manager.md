---
title: 適用於 Python 的 Azure 伺服器管理員程式庫
description: 適用於 Python 的 Azure 伺服器管理員程式庫參考
keywords: Azure, python, SDK, API, 伺服器管理員
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 480513a76683c97fdac8d2a65b38fde0fffb6dcd
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479231"
---
# <a name="azure-server-manager-libraries-for-python"></a><span data-ttu-id="d89a0-104">適用於 Python 的 Azure 伺服器管理員程式庫</span><span class="sxs-lookup"><span data-stu-id="d89a0-104">Azure Server Manager libraries for python</span></span>

## <a name="management-apipythonapioverviewazureservermanagermanagement"></a>[<span data-ttu-id="d89a0-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="d89a0-105">Management API</span></span>](/python/api/overview/azure/servermanager/management)

```bash
pip install azure-mgmt-servermanager
```

## <a name="create-the-management-client"></a><span data-ttu-id="d89a0-106">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="d89a0-106">Create the management client</span></span>

<span data-ttu-id="d89a0-107">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d89a0-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="d89a0-108">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="d89a0-108">You will need to provide your ``subscription_id`` which can be retrieved from your [subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="d89a0-109">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d89a0-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.servermanager import ServerManagement
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

servermanager_client = ServerManagement(
    credentials,
    subscription_id
)
``` 

## <a name="create-gateway"></a><span data-ttu-id="d89a0-110">建立閘道</span><span class="sxs-lookup"><span data-stu-id="d89a0-110">Create gateway</span></span>
```python
gateway_async = servermanager_client.gateway.create(
    'MyResourceGroup',
    'MyGateway',
    'centralus'
)
gateway = gateway_async.result() # Blocking wait
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d89a0-111">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="d89a0-111">Explore the Management APIs</span></span>](/python/api/overview/azure/servermanager/management)