---
title: 適用於 Python 的 Azure DevTest Labs 程式庫
description: 適用於 Python 的 Azure DevTest Labs 程式庫參考
keywords: Azure, python, SDK, API, DevTest Labs
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 3da9210dd14c19d591539656fb7229f4d3346ea9
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376884"
---
# <a name="azure-devtest-labs-libraries-for-python"></a><span data-ttu-id="09c2b-104">適用於 Python 的 Azure DevTest Labs 程式庫</span><span class="sxs-lookup"><span data-stu-id="09c2b-104">Azure DevTest Labs libraries for python</span></span>

## <a name="management-apipythonapioverviewazuredevtestlabsmanagement"></a>[<span data-ttu-id="09c2b-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="09c2b-105">Management API</span></span>](/python/api/overview/azure/devtestlabs/management)

```bash
pip install azure-mgmt-devtestlabs
```

## <a name="create-the-management-client"></a><span data-ttu-id="09c2b-106">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="09c2b-106">Create the management client</span></span>

<span data-ttu-id="09c2b-107">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="09c2b-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="09c2b-108">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="09c2b-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="09c2b-109">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="09c2b-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.devtestlabs import DevTestLabsClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

devtestlabs_client = DevTestLabsClient(
    credentials,
    subscription_id
)
```

## <a name="create-lab"></a><span data-ttu-id="09c2b-110">建立實驗室</span><span class="sxs-lookup"><span data-stu-id="09c2b-110">Create lab</span></span>

```python
async_lab = self.client.lab.create_or_update_resource(
    'MyResourceGroup',
    'MyLab',
    {'location': 'westus'}
)
lab = async_lab.result() # Blocking wait
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="09c2b-111">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="09c2b-111">Explore the Management APIs</span></span>](/python/api/overview/azure/devtestlabs/management)
