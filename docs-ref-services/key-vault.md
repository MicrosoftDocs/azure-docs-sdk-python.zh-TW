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
# <a name="azure-key-vault-libraries-for-python"></a>適用於 Python 的 Azure Key Vault 程式庫

[Azure Key Vault](/azure/key-vault/) 是 Azure 的密碼金鑰、祕密和憑證管理的存放區和管理系統。 適用於 Key Vault 的 Python SDK API 分為用戶端程式庫和管理程式庫。

使用用戶端程式庫可以：
- 存取、更新或刪除儲存在 Azure Key Vault 中的項目
- 取得儲存憑證的中繼資料
- 驗證簽章和 Key Vault 中的對稱金鑰

使用管理程式庫可以：
- 建立、更新或刪除新的 Key Vault 存放區
- 控管保存庫存取原則
- 依訂用帳戶或資源群組列出保存庫
- 檢查保存庫名稱可用性

## <a name="install-the-libraries"></a>安裝程式庫

### <a name="client-library"></a>用戶端程式庫

```bash
pip install azure-keyvault
```

## <a name="examples"></a>範例

下列範例使用建議的服務主體驗證作為與 Azure 連線的應用程式登入方法。 要學習服務主體驗證相關資訊，請參閱[使用適用於 Python 的 Azure SDK 驗證](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)

從保存庫擷取非對稱金鑰的公開部分：

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

從保存庫擷取祕密：

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
> [探索用戶端 API](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a>管理程式庫

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>範例

下列範例示範如何建立 Azure Key Vault。 

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
> [探索管理 API](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>範例
* [管理 Azure Key Vault][1] 
* [Azure Key Vault 復原][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

檢視 Azure Key Vault 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。 
