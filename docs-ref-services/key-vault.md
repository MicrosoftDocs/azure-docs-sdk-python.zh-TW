---
title: 適用於 Python 的 Azure Key Vault 程式庫
description: 適用於 Azure Key Vault 之 Python 用戶端程式庫的參考文件
author: sptramer
manager: carmonm
ms.author: sttramer
ms.date: 06/10/2019
ms.topic: conceptual
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: f4661ee389c13ce8546e7b5cc8866ab7b216d3b0
ms.sourcegitcommit: 92fa5dbcfd9a20f4a49da5f4bdc03045783d3495
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2019
ms.locfileid: "67149330"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="720c8-103">適用於 Python 的 Azure Key Vault 程式庫</span><span class="sxs-lookup"><span data-stu-id="720c8-103">Azure Key Vault libraries for Python</span></span>

<span data-ttu-id="720c8-104">[Azure Key Vault](/azure/key-vault/) 是 Azure 的密碼金鑰、祕密和憑證管理的存放區和管理系統。</span><span class="sxs-lookup"><span data-stu-id="720c8-104">[Azure Key Vault](/azure/key-vault/) is Azure's storage and management system for cryptographic keys, secrets, and certificate management.</span></span> <span data-ttu-id="720c8-105">適用於 Key Vault 的 Python SDK API 分為用戶端程式庫和管理程式庫。</span><span class="sxs-lookup"><span data-stu-id="720c8-105">The Python SDK API for Key Vault is split between client libraries and management libraries.</span></span>

<span data-ttu-id="720c8-106">使用用戶端程式庫可以：</span><span class="sxs-lookup"><span data-stu-id="720c8-106">Use the client library to:</span></span>
- <span data-ttu-id="720c8-107">存取、更新或刪除儲存在 Azure Key Vault 中的項目</span><span class="sxs-lookup"><span data-stu-id="720c8-107">Access, update, or delete items stored in an Azure Key Vault</span></span>
- <span data-ttu-id="720c8-108">取得儲存憑證的中繼資料</span><span class="sxs-lookup"><span data-stu-id="720c8-108">Get metadata for stored certificates</span></span>
- <span data-ttu-id="720c8-109">驗證簽章和 Key Vault 中的對稱金鑰</span><span class="sxs-lookup"><span data-stu-id="720c8-109">Verify signatures against symmetric keys in Key Vault</span></span>

<span data-ttu-id="720c8-110">使用管理程式庫可以：</span><span class="sxs-lookup"><span data-stu-id="720c8-110">Use the management library to:</span></span>
- <span data-ttu-id="720c8-111">建立、更新或刪除新的 Key Vault 存放區</span><span class="sxs-lookup"><span data-stu-id="720c8-111">Create, update, or delete new Key Vault stores</span></span>
- <span data-ttu-id="720c8-112">控管保存庫存取原則</span><span class="sxs-lookup"><span data-stu-id="720c8-112">Control vault access policies</span></span>
- <span data-ttu-id="720c8-113">依訂用帳戶或資源群組列出保存庫</span><span class="sxs-lookup"><span data-stu-id="720c8-113">List vaults by subscription or resource group</span></span>
- <span data-ttu-id="720c8-114">檢查保存庫名稱可用性</span><span class="sxs-lookup"><span data-stu-id="720c8-114">Check for vault name availability</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="720c8-115">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="720c8-115">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="720c8-116">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="720c8-116">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="720c8-117">範例</span><span class="sxs-lookup"><span data-stu-id="720c8-117">Examples</span></span>

<span data-ttu-id="720c8-118">下列範例使用建議的服務主體驗證作為與 Azure 連線的應用程式登入方法。</span><span class="sxs-lookup"><span data-stu-id="720c8-118">The following examples use service principal authentication, which is the recommended sign in method for applications that connect to Azure.</span></span> <span data-ttu-id="720c8-119">要學習服務主體驗證相關資訊，請參閱[使用適用於 Python 的 Azure SDK 驗證](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span><span class="sxs-lookup"><span data-stu-id="720c8-119">To learn about service principal authentication, see [Authenticate with the Azure SDK for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span></span>

<span data-ttu-id="720c8-120">從保存庫擷取非對稱金鑰的公開部分：</span><span class="sxs-lookup"><span data-stu-id="720c8-120">Retrieve the public portion of an asymmetric key from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# KEY_VERSION is required, and can be obtained with the KeyVaultClient.get_key_versions(self, vault_url, key_name) API
key_bundle = client.get_key(VAULT_URL, KEY_NAME, KEY_VERSION)
key = key_bundle.key
```

<span data-ttu-id="720c8-121">從保存庫擷取祕密：</span><span class="sxs-lookup"><span data-stu-id="720c8-121">Retrieve a secret from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# SECRET_VERSION is required, and can be obtained with the KeyVaultClient.get_secret_versions(self, vault_url, secret_id) API
secret_bundle = client.get_secret(VAULT_URL, SECRET_ID, SECRET_VERSION)
secret = secret_bundle.value
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="720c8-122">探索用戶端 API</span><span class="sxs-lookup"><span data-stu-id="720c8-122">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a><span data-ttu-id="720c8-123">管理程式庫</span><span class="sxs-lookup"><span data-stu-id="720c8-123">Management library</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="720c8-124">範例</span><span class="sxs-lookup"><span data-stu-id="720c8-124">Example</span></span>

<span data-ttu-id="720c8-125">下列範例示範如何建立 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="720c8-125">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient
from azure.common.credentials import ServicePrincipalCredentials


credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

# Even when using service principal credentials, a subscription ID is required. For service principals,
# this should be the subscription used to create the service principal. Storing a token like a valid
# subscription ID in code is not recommended and only shown here for example purposes.
SUBSCRIPTION_ID = '...'
client = KeyVaultManagementClient(credentials, SUBSCRIPTION_ID)

# The object ID and organization ID (tenant) of the user, application, or service principal for access policies.
# These values can be found through the Azure CLI or the Portal.
ALLOW_OBJECT_ID = '...'
ALLOW_TENANT_ID = '...'

RESOURCE_GROUP = '...'
VAULT_NAME = '...'

# Vault properties may also be created by using the azure.mgmt.keyvault.models.VaultCreateOrUpdateParameters
# class, rather than a map. 
operation = client.vaults.create_or_update(
    RESOURCE_GROUP,
    VAULT_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': TENANT_ID,
            'access_policies': [{
                'object_id': OBJECT_ID,
                'tenant_id': ALLOW_TENANT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)

vault = operation.result()
print(f'New vault URI: {vault.properties.vault_uri}')
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="720c8-126">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="720c8-126">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="720c8-127">範例</span><span class="sxs-lookup"><span data-stu-id="720c8-127">Samples</span></span>
* <span data-ttu-id="720c8-128">[管理 Azure Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="720c8-128">[Manage Azure Key Vaults][1]</span></span> 
* <span data-ttu-id="720c8-129">[Azure Key Vault 復原][2]</span><span class="sxs-lookup"><span data-stu-id="720c8-129">[Azure Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="720c8-130">檢視 Azure Key Vault 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。</span><span class="sxs-lookup"><span data-stu-id="720c8-130">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 
