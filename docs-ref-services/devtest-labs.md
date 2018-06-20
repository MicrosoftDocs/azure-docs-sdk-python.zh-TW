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
ms.openlocfilehash: 5807b85f02c05c2a767a2df2e89d9e98b7e6e05b
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551571"
---
# <a name="azure-devtest-labs-libraries-for-python"></a><span data-ttu-id="2212e-104">適用於 Python 的 Azure DevTest Labs 程式庫</span><span class="sxs-lookup"><span data-stu-id="2212e-104">Azure DevTest Labs libraries for python</span></span>

## <a name="management-apipythonapioverviewazuredevtestlabsmanagement"></a>[<span data-ttu-id="2212e-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="2212e-105">Management API</span></span>](/python/api/overview/azure/devtestlabs/management)

```bash
pip install azure-mgmt-devtestlabs
```

## <a name="create-the-management-client"></a><span data-ttu-id="2212e-106">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="2212e-106">Create the management client</span></span>

<span data-ttu-id="2212e-107">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="2212e-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="2212e-108">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="2212e-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="2212e-109">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2212e-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

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

## <a name="create-lab"></a><span data-ttu-id="2212e-110">建立實驗室</span><span class="sxs-lookup"><span data-stu-id="2212e-110">Create lab</span></span>

```python
async_lab = self.client.lab.create_or_update_resource(
    'MyResourceGroup',
    'MyLab',
    {'location': 'westus'}
)
lab = async_lab.result() # Blocking wait
``` 

> [!div class="nextstepaction"]
> [<span data-ttu-id="2212e-111">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="2212e-111">Explore the Management APIs</span></span>](/python/api/overview/azure/devtestlabs/management)