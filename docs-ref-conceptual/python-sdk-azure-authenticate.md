---
title: "使用適用於 Python 的 Azure 管理程式庫來進行驗證"
description: "使用用來進入適用於 Python 之 Azure 管理程式庫的服務主體來進行驗證"
keywords: "Azure, Python, SDK, API, 驗證, Active Directory, 服務主體"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 271722eee1ef982d1f091b3d3af29069917f3e17
ms.sourcegitcommit: 97e5d660eb4a006f969c3010087e1386cc6eb482
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2018
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="446f6-104">使用適用於 Python 的 Azure 管理程式庫來進行驗證</span><span class="sxs-lookup"><span data-stu-id="446f6-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="446f6-105">在使用 Python 管理程式庫來建立和管理資源時，有多個選項可供您向 Azure 驗證應用程式。</span><span class="sxs-lookup"><span data-stu-id="446f6-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="446f6-106">使用權杖認證進行驗證</span><span class="sxs-lookup"><span data-stu-id="446f6-106">Authenticate with token credentials</span></span>

<span data-ttu-id="446f6-107">在組態檔、登錄或 Azure Key Vault 中安全地儲存認證。</span><span class="sxs-lookup"><span data-stu-id="446f6-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="446f6-108">下列範例會使用[服務主體](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)進行驗證。</span><span class="sxs-lookup"><span data-stu-id="446f6-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="446f6-109">您可以透過 Azure CLI 2.0 來建立服務主體</span><span class="sxs-lookup"><span data-stu-id="446f6-109">You can create a Service Principal via the Azure CLI 2.0</span></span>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```

```python
    from azure.common.credentials import ServicePrincipalCredentials

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID
    )
```

> <span data-ttu-id="446f6-110">[注意！] 若要連線到其中一個 Azure Sovereign Cloud，請使用 `cloud_environment` 參數。</span><span class="sxs-lookup"><span data-stu-id="446f6-110">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>

```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID,
        cloud_environment = AZURE_CHINA_CLOUD
    )
```

<span data-ttu-id="446f6-111">如果您需要更多的控制，建議使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包裝函式。</span><span class="sxs-lookup"><span data-stu-id="446f6-111">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="446f6-112">請參閱 ADAL 網站取得所有可用的案例清單和範例。</span><span class="sxs-lookup"><span data-stu-id="446f6-112">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="446f6-113">適用於服務主體驗證的範例：</span><span class="sxs-lookup"><span data-stu-id="446f6-113">For instance for service principal authentication:</span></span>

```python
    import adal
    from msrestazure.azure_active_directory import AdalAuthentication
    from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
    RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

    context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
    credentials = AdalAuthentication(
        context.acquire_token_with_client_credentials,
        RESOURCE,
        CLIENT,
        KEY
    )
```

<span data-ttu-id="446f6-114">所有 ADAL 的有效呼叫可以搭配使用 `AdalAuthentication` 類別。</span><span class="sxs-lookup"><span data-stu-id="446f6-114">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="446f6-115">然後，建立用戶端物件開始使用 API：</span><span class="sxs-lookup"><span data-stu-id="446f6-115">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="446f6-116">[注意！] 使用 Azure Sovereign Cloud 時，也必須在建立管理用戶端時指定適當的基底 URL (透過 `msrestazure.azure_cloud` 中的常數)。</span><span class="sxs-lookup"><span data-stu-id="446f6-116">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="446f6-117">對於 Azure China Cloud 的範例：</span><span class="sxs-lookup"><span data-stu-id="446f6-117">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="446f6-118">檔案型驗證</span><span class="sxs-lookup"><span data-stu-id="446f6-118">File based authentication</span></span>

<span data-ttu-id="446f6-119">最簡單的驗證方式就是建立 JSON 檔，其中包含適用於 Azure 服務主體的認證。</span><span class="sxs-lookup"><span data-stu-id="446f6-119">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="446f6-120">您可以使用下列 CLI 命令，同時建立新的服務主體和這個檔案：</span><span class="sxs-lookup"><span data-stu-id="446f6-120">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="446f6-121">將此檔案儲存在系統上可供程式碼讀取且安全的位置。</span><span class="sxs-lookup"><span data-stu-id="446f6-121">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="446f6-122">在殼層中使用該檔案的完整路徑來設定環境變數：</span><span class="sxs-lookup"><span data-stu-id="446f6-122">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="446f6-123">如果您需要自行建立檔案，請遵循以下格式：</span><span class="sxs-lookup"><span data-stu-id="446f6-123">If you want to create the file yourself, please follow this format:</span></span>

```json
{
    "clientId": "ad735158-65ca-11e7-ba4d-ecb1d756380e",
    "clientSecret": "b70bb224-65ca-11e7-810c-ecb1d756380e",
    "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
    "tenantId": "c81da1d8-65ca-11e7-b1d1-ecb1d756380e",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

<span data-ttu-id="446f6-124">然後，您可以建立使用用戶端處理站的任何用戶端：</span><span class="sxs-lookup"><span data-stu-id="446f6-124">You can then create any client using the client factory:</span></span>
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="446f6-125">使用受管理服務身分識別 (MSI)</span><span class="sxs-lookup"><span data-stu-id="446f6-125">Authenticate with Managed Service Identity(MSI)</span></span> 
<span data-ttu-id="446f6-126">MSI 是一種簡單的方法，讓 Azure 中的資源可以在無需建立特定認證的情況下，使用 SDK/CLI。</span><span class="sxs-lookup"><span data-stu-id="446f6-126">MSI is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

    # Create MSI Authentication
    credentials = MSIAuthentication()


    # Create a Subscription Client
    subscription_client = SubscriptionClient(credentials)
    subscription = next(subscription_client.subscriptions.list())
    subscription_id = subscription.subscription_id

    # Create a Resource Management client
    resource_client = ResourceManagementClient(credentials, subscription_id)

    
    # List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
    for resource_group in resource_client.resource_groups.list():
        print(resource_group.name)

```

## <a name="mgmt-auth-cli"></a><span data-ttu-id="446f6-127">CLI 型驗證</span><span class="sxs-lookup"><span data-stu-id="446f6-127">CLI-based authentication</span></span>

<span data-ttu-id="446f6-128">SDK 能夠使用 CLI 使用中訂用帳戶來建立用戶端。</span><span class="sxs-lookup"><span data-stu-id="446f6-128">The SDK is able to create a client using your CLI active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="446f6-129">這應該用來作為快速入門開發人員的體驗。</span><span class="sxs-lookup"><span data-stu-id="446f6-129">This should be used as quick start developer experience.</span></span> <span data-ttu-id="446f6-130">若要用於生產環境，請使用 [ADAL](#authenticate-with-token-credentials) 或您自己的認證系統。</span><span class="sxs-lookup"><span data-stu-id="446f6-130">For production purposes, use [ADAL](#authenticate-with-token-credentials) or your own credentials system.</span></span>
> <span data-ttu-id="446f6-131">對 CLI 設定的任何變更都會影響 SDK 執行。</span><span class="sxs-lookup"><span data-stu-id="446f6-131">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="446f6-132">若要定義使用中的認證，請使用 [az 登入](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="446f6-132">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="446f6-133">預設訂用帳戶識別碼是您所擁有的唯一一個，或是可以使用 [az 帳戶](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)加以定義</span><span class="sxs-lookup"><span data-stu-id="446f6-133">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="446f6-134">使用權杖認證進行驗證 (舊版)</span><span class="sxs-lookup"><span data-stu-id="446f6-134">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="446f6-135">在舊版的 SDK 中，ADAL 尚無法提供使用，因此我們提供了 `UserPassCredentials` 類別。</span><span class="sxs-lookup"><span data-stu-id="446f6-135">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="446f6-136">這會視為已被取代，不應該再使用。</span><span class="sxs-lookup"><span data-stu-id="446f6-136">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="446f6-137">這個範例示範使用者/密碼情節。</span><span class="sxs-lookup"><span data-stu-id="446f6-137">This sample shows user/password scenario.</span></span> <span data-ttu-id="446f6-138">不支援 2FA。</span><span class="sxs-lookup"><span data-stu-id="446f6-138">This does not support 2FA.</span></span>

```python
    from azure.common.credentials import UserPassCredentials

    credentials = UserPassCredentials(
        'user@domain.com',
        'my_smart_password',
    )
```
