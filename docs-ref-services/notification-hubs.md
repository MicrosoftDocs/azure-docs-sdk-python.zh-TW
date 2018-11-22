---
title: 適用於 Python 的 Azure 通知中樞程式庫
description: 適用於 Python 的 Azure 通知中樞程式庫參考
keywords: Azure, python, SDK, API, 通知中樞
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 3a9cc087d315ee2a274d3ef00623b304280017e5
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277240"
---
# <a name="azure-notification-hubs-libraries-for-python"></a><span data-ttu-id="5395f-104">適用於 Python 的 Azure 通知中樞程式庫</span><span class="sxs-lookup"><span data-stu-id="5395f-104">Azure Notification Hubs libraries for python</span></span>

## <a name="management-apipythonapioverviewazurenotificationhubsmanagement"></a>[<span data-ttu-id="5395f-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="5395f-105">Management API</span></span>](/python/api/overview/azure/notificationhubs/management)

```bash
pip install azure-mgmt-notificationhubs
```

## <a name="create-the-management-client"></a><span data-ttu-id="5395f-106">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="5395f-106">Create the management client</span></span>

<span data-ttu-id="5395f-107">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="5395f-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="5395f-108">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="5395f-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="5395f-109">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5395f-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.notificationhubs import NotificationHubsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

redis_client = NotificationHubsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-namespace-availability"></a><span data-ttu-id="5395f-110">檢查命名空間可用性</span><span class="sxs-lookup"><span data-stu-id="5395f-110">Check namespace availability</span></span>

<span data-ttu-id="5395f-111">下列程式碼會檢查通知中樞的命名空間可用性。</span><span class="sxs-lookup"><span data-stu-id="5395f-111">The following code check namespace availability of a notification hub.</span></span>
```python
from azure.mgmt.notificationhubs.models import CheckAvailabilityParameters

account_name = 'mynotificationhub'
output = notificationhubs_client.namespaces.check_availability(
    azure.mgmt.notificationhubs.models.CheckAvailabilityParameters(
        name = account_name
    )
)
# output is a CheckAvailibilityResource instance
print(output.is_availiable) # Yes, it's 'availiable', it's a typo in the REST API
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5395f-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="5395f-112">Explore the Management APIs</span></span>](/python/api/overview/azure/notificationhubs/management)
