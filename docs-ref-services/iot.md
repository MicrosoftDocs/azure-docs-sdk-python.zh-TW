---
title: "適用於 Python 的 Azure IoT 中樞程式庫"
description: "適用於 Python 的 Azure IoT 中樞程式庫參考"
keywords: "Azure, python, SDK, API, IoT 中樞"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 0a1a5efa299f66ff8c31e8224e29dd7bcdc41783
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="azure-iot-hub-libraries-for-python"></a><span data-ttu-id="32d9a-104">適用於 Python 的 Azure IoT 中樞程式庫</span><span class="sxs-lookup"><span data-stu-id="32d9a-104">Azure IoT Hub libraries for python</span></span>

## <a name="management-apipythonapioverviewazureiotmanagement"></a>[<span data-ttu-id="32d9a-105">管理 API</span><span class="sxs-lookup"><span data-stu-id="32d9a-105">Management API</span></span>](/python/api/overview/azure/iot/management)

```bash
pip install azure-mgmt-iothub
```

## <a name="create-the-management-client"></a><span data-ttu-id="32d9a-106">建立管理用戶端</span><span class="sxs-lookup"><span data-stu-id="32d9a-106">Create the management client</span></span>

<span data-ttu-id="32d9a-107">下列程式碼會建立管理用戶端的執行個體。</span><span class="sxs-lookup"><span data-stu-id="32d9a-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="32d9a-108">您必須提供您的 ``subscription_id`` (可從[訂用帳戶清單](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)來擷取)。</span><span class="sxs-lookup"><span data-stu-id="32d9a-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="32d9a-109">請參閱[資源管理驗證](/python/azure/python-sdk-azure-authenticate)，以深入了解如何使用 Python SDK 來處理 Azure Active Directory 驗證，以及如何建立 ``Credentials`` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="32d9a-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.iothub import IotHubClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

iothub_client = IotHubClient(
    credentials,
    subscription_id
)
```

## <a name="create-an-iothub"></a><span data-ttu-id="32d9a-110">建立 IoTHub</span><span class="sxs-lookup"><span data-stu-id="32d9a-110">Create an IoTHub</span></span>
```python
async_iot_hub = iothub_client.iot_hub_resource.create_or_update(
    'MyResourceGroup',
    'MyIoTHubAccount',
    {
        'location': 'westus',
        'subscriptionid': subscription_id,
        'resourcegroup': 'MyResourceGroup',
        'sku': {
            'name': 'S1',
            'capacity': 2
        },
        'properties': {
            'enable_file_upload_notifications': False,
            'operations_monitoring_properties': {
            'events': {
                "C2DCommands": "Error",
                "DeviceTelemetry": "Error",
                "DeviceIdentityOperations": "Error",
                "Connections": "Information"
            }
            },
            "features": "None",
        }
    }
)
iothub = async_iot_hub.result() # Blocking wait for creation
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="32d9a-111">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="32d9a-111">Explore the Management APIs</span></span>](/python/api/overview/azure/iot/management)