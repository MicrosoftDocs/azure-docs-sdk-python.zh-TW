---
title: 使用適用於 Python 的 Azure 管理程式庫來進行驗證
description: 使用用來進入適用於 Python 之 Azure 管理程式庫的服務主體來進行驗證
keywords: Azure, Python, SDK, API, 驗證, Active Directory, 服務主體
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/11/2019
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 51f26b120cefffd2d7f4af9c2b6b2cb532bc6006
ms.sourcegitcommit: 375a1f9180eb1323fe2af0a7e28fd4676973c68e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2019
ms.locfileid: "59586806"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="0799b-104">使用適用於 Python 的 Azure 管理程式庫來進行驗證</span><span class="sxs-lookup"><span data-stu-id="0799b-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="0799b-105">在使用 Python 管理程式庫來建立和管理資源時，有多個選項可供您向 Azure 驗證應用程式。</span><span class="sxs-lookup"><span data-stu-id="0799b-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="0799b-106">使用權杖認證進行驗證</span><span class="sxs-lookup"><span data-stu-id="0799b-106">Authenticate with token credentials</span></span>

<span data-ttu-id="0799b-107">在組態檔、登錄或 Azure Key Vault 中安全地儲存認證。</span><span class="sxs-lookup"><span data-stu-id="0799b-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="0799b-108">下列範例會使用[服務主體](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)進行驗證。</span><span class="sxs-lookup"><span data-stu-id="0799b-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="0799b-109">若要使用 Azure CLI 建立服務主體，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="0799b-109">To create a service principal with the Azure CLI, use the following command:</span></span>
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> <span data-ttu-id="0799b-110">若要深入了解使用 CLI 設定服務主體，請參閱[使用 Azure CLI 建立 Azure 服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="0799b-110">To learn more about setting up service princpals with the CLI, see [Create an Azure service principal with Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials

# Tenant ID for your Azure subscription
TENANT_ID = '<Your tenant ID>'

# Your service principal App ID
CLIENT = '<Your service principal ID>'

# Your service principal password
KEY = '<Your service principal password>'

credentials = ServicePrincipalCredentials(
    client_id = CLIENT,
    secret = KEY,
    tenant = TENANT_ID
)
```

> <span data-ttu-id="0799b-111">[注意！] 若要連線到其中一個 Azure Sovereign Cloud，請使用 `cloud_environment` 參數。</span><span class="sxs-lookup"><span data-stu-id="0799b-111">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>
>
> ```python
> from azure.common.credentials import ServicePrincipalCredentials
> from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
> 
> # Tenant ID for your Azure Subscription
> TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
> 
> # Your Service Principal App ID
> CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'
> 
> # Your Service Principal Password
> KEY = 'password'
> 
> credentials = ServicePrincipalCredentials(
>     client_id = CLIENT,
>     secret = KEY,
>     tenant = TENANT_ID,
>     cloud_environment = AZURE_CHINA_CLOUD
> )
> ```

<span data-ttu-id="0799b-112">如果您需要更多的控制，建議使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包裝函式。</span><span class="sxs-lookup"><span data-stu-id="0799b-112">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="0799b-113">請參閱 ADAL 網站取得所有可用的案例清單和範例。</span><span class="sxs-lookup"><span data-stu-id="0799b-113">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="0799b-114">適用於服務主體驗證的範例：</span><span class="sxs-lookup"><span data-stu-id="0799b-114">For instance for service principal authentication:</span></span>

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

<span data-ttu-id="0799b-115">所有 ADAL 的有效呼叫可以搭配使用 `AdalAuthentication` 類別。</span><span class="sxs-lookup"><span data-stu-id="0799b-115">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="0799b-116">然後，建立用戶端物件開始使用 API：</span><span class="sxs-lookup"><span data-stu-id="0799b-116">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="0799b-117">[注意！] 使用 Azure Sovereign Cloud 時，也必須在建立管理用戶端時指定適當的基底 URL (透過 `msrestazure.azure_cloud` 中的常數)。</span><span class="sxs-lookup"><span data-stu-id="0799b-117">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="0799b-118">對於 Azure China Cloud 的範例：</span><span class="sxs-lookup"><span data-stu-id="0799b-118">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="0799b-119">檔案型驗證</span><span class="sxs-lookup"><span data-stu-id="0799b-119">File based authentication</span></span>

<span data-ttu-id="0799b-120">最簡單的驗證方式就是建立 JSON 檔，其中包含適用於 Azure 服務主體的認證。</span><span class="sxs-lookup"><span data-stu-id="0799b-120">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="0799b-121">您可以使用下列 CLI 命令，同時建立新的服務主體和這個檔案：</span><span class="sxs-lookup"><span data-stu-id="0799b-121">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="0799b-122">將此檔案儲存在系統上可供程式碼讀取且安全的位置。</span><span class="sxs-lookup"><span data-stu-id="0799b-122">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="0799b-123">在殼層中使用該檔案的完整路徑來設定環境變數：</span><span class="sxs-lookup"><span data-stu-id="0799b-123">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="0799b-124">如果您需要自行建立檔案，請遵循以下格式：</span><span class="sxs-lookup"><span data-stu-id="0799b-124">If you want to create the file yourself, please follow this format:</span></span>

```json
{
    "clientId": "<Service principal ID>",
    "clientSecret": "<Service principal secret/password>",
    "subscriptionId": "<Subscription associated with the service principal>",
    "tenantId": "<The service principal's tenant>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

<span data-ttu-id="0799b-125">然後，您可以建立使用用戶端處理站的任何用戶端：</span><span class="sxs-lookup"><span data-stu-id="0799b-125">You can then create any client using the client factory:</span></span>

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="0799b-126">使用 Azure 受控識別進行驗證</span><span class="sxs-lookup"><span data-stu-id="0799b-126">Authenticate with Azure Managed Identities</span></span>
<span data-ttu-id="0799b-127">Azure 受控識別是一種簡單的方法，讓 Azure 中的資源可以在無需建立特定認證的情況下，使用 SDK/CLI。</span><span class="sxs-lookup"><span data-stu-id="0799b-127">Azure Managed Identity is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="0799b-128">若要使用受控識別，您必須從 Azure Functions 或在 Azure 中執行的虛擬機器 Azure 資源連線到 Azure。</span><span class="sxs-lookup"><span data-stu-id="0799b-128">To use managed identities, you must be connecting to Azure from an Azure resource, such as an Azure Function or a VM running in Azure.</span></span> <span data-ttu-id="0799b-129">若要了解如何設定資源的受控識別，請參閱[設定 Azure 資源的受控識別](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)和[如何使用適用於 Azure 資源的受控識別](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)。</span><span class="sxs-lookup"><span data-stu-id="0799b-129">To learn how to configure a managed identity for a resource, see [Configure managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) and [How to use managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).</span></span>

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

## <a name="mgmt-auth-cli"></a><span data-ttu-id="0799b-130">CLI 型驗證</span><span class="sxs-lookup"><span data-stu-id="0799b-130">CLI-based authentication</span></span>

<span data-ttu-id="0799b-131">SDK 能夠使用 Azure CLI 的使用中訂用帳戶來建立用戶端。</span><span class="sxs-lookup"><span data-stu-id="0799b-131">The SDK is able to create a client using the Azure CLI's active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0799b-132">這應該用來作為快速入門開發人員的體驗。</span><span class="sxs-lookup"><span data-stu-id="0799b-132">This should be used as quick start developer experience.</span></span> <span data-ttu-id="0799b-133">若要用於生產環境，請使用 [ADAL](#mgmt-auth-legacy) 或您自己的認證系統。</span><span class="sxs-lookup"><span data-stu-id="0799b-133">For production purposes, use [ADAL](#mgmt-auth-legacy) or your own credentials system.</span></span>
> <span data-ttu-id="0799b-134">對 CLI 設定的任何變更都會影響 SDK 執行。</span><span class="sxs-lookup"><span data-stu-id="0799b-134">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="0799b-135">若要定義使用中的認證，請使用 [az 登入](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="0799b-135">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="0799b-136">預設訂用帳戶識別碼是您所擁有的唯一一個，或是可以使用 [az 帳戶](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)加以定義</span><span class="sxs-lookup"><span data-stu-id="0799b-136">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="0799b-137">使用權杖認證進行驗證 (舊版)</span><span class="sxs-lookup"><span data-stu-id="0799b-137">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="0799b-138">在舊版的 SDK 中，ADAL 尚無法提供使用，因此我們提供了 `UserPassCredentials` 類別。</span><span class="sxs-lookup"><span data-stu-id="0799b-138">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="0799b-139">這會視為已被取代，不應該再使用。</span><span class="sxs-lookup"><span data-stu-id="0799b-139">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="0799b-140">這個範例示範使用者/密碼情節。</span><span class="sxs-lookup"><span data-stu-id="0799b-140">This sample shows user/password scenario.</span></span> <span data-ttu-id="0799b-141">不支援 2FA。</span><span class="sxs-lookup"><span data-stu-id="0799b-141">This does not support 2FA.</span></span>

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```
