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
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>使用適用於 Python 的 Azure 管理程式庫來進行驗證

在使用 Python 管理程式庫來建立和管理資源時，有多個選項可供您向 Azure 驗證應用程式。

## <a name="mgmt-auth-token"></a>使用權杖認證進行驗證

在組態檔、登錄或 Azure Key Vault 中安全地儲存認證。

下列範例會使用[服務主體](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)進行驗證。

> [!NOTE]
> 若要使用 Azure CLI 建立服務主體，請使用下列命令：
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> 若要深入了解使用 CLI 設定服務主體，請參閱[使用 Azure CLI 建立 Azure 服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)

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

> [注意！] 若要連線到其中一個 Azure Sovereign Cloud，請使用 `cloud_environment` 參數。
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

如果您需要更多的控制，建議使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包裝函式。 請參閱 ADAL 網站取得所有可用的案例清單和範例。 適用於服務主體驗證的範例：

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

所有 ADAL 的有效呼叫可以搭配使用 `AdalAuthentication` 類別。

然後，建立用戶端物件開始使用 API：

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> [注意！] 使用 Azure Sovereign Cloud 時，也必須在建立管理用戶端時指定適當的基底 URL (透過 `msrestazure.azure_cloud` 中的常數)。 對於 Azure China Cloud 的範例：
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a>檔案型驗證

最簡單的驗證方式就是建立 JSON 檔，其中包含適用於 Azure 服務主體的認證。 您可以使用下列 CLI 命令，同時建立新的服務主體和這個檔案：

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

將此檔案儲存在系統上可供程式碼讀取且安全的位置。 在殼層中使用該檔案的完整路徑來設定環境變數：

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

如果您需要自行建立檔案，請遵循以下格式：

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

然後，您可以建立使用用戶端處理站的任何用戶端：

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a>使用 Azure 受控識別進行驗證
Azure 受控識別是一種簡單的方法，讓 Azure 中的資源可以在無需建立特定認證的情況下，使用 SDK/CLI。

> [!IMPORTANT]
>
> 若要使用受控識別，您必須從 Azure Functions 或在 Azure 中執行的虛擬機器 Azure 資源連線到 Azure。 若要了解如何設定資源的受控識別，請參閱[設定 Azure 資源的受控識別](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)和[如何使用適用於 Azure 資源的受控識別](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)。

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

## <a name="mgmt-auth-cli"></a>CLI 型驗證

SDK 能夠使用 Azure CLI 的使用中訂用帳戶來建立用戶端。

> [!IMPORTANT]
> 這應該用來作為快速入門開發人員的體驗。 若要用於生產環境，請使用 [ADAL](#mgmt-auth-legacy) 或您自己的認證系統。
> 對 CLI 設定的任何變更都會影響 SDK 執行。

若要定義使用中的認證，請使用 [az 登入](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。
預設訂用帳戶識別碼是您所擁有的唯一一個，或是可以使用 [az 帳戶](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)加以定義

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a>使用權杖認證進行驗證 (舊版)

在舊版的 SDK 中，ADAL 尚無法提供使用，因此我們提供了 `UserPassCredentials` 類別。 這會視為已被取代，不應該再使用。

這個範例示範使用者/密碼情節。 不支援 2FA。

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```
