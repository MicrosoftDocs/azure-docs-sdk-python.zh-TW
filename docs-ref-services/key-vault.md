---
title: "適用於 Python 的 Azure Key Vault 程式庫"
description: "適用於 Azure Key Vault 之 Python 用戶端程式庫的參考文件"
author: lisawong19
keywords: "Azure, Python, SDK, API, Keys, Key Vault, 驗證, 祕密, 金鑰, 安全性"
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 6f0f1012839dad21fb8140dbbdf0f883d2877317
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="azure-key-vault-libraries-for-python"></a>適用於 Python 的 Azure Key Vault 程式庫

## <a name="overview"></a>概觀

使用用戶端程式庫建立、更新和刪除 Azure Key Vault 中的金鑰和祕密。

使用 Azure Key Vault 管理程式庫來建立金鑰保存庫、授權應用程式並管理權限。 

深入了解 [Azure Key Vault](/azure/key-vault/key-vault-whatis)。

## <a name="install-the-libraries"></a>安裝程式庫

### <a name="client-library"></a>用戶端程式庫
```bash
pip install azure-keyvault
```

## <a name="example"></a>範例
從 Key Vault 擷取[JSON Web 金鑰](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callack(server, resource, scope):
    credentials = credentials or ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = resource
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callack))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```
[!div class="nextstepaction"]
[探索用戶端 API](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a>管理 API
```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>範例
下列範例示範如何建立 Azure Key Vault。 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [探索管理 API](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>範例
* [管理 Key Vault][1] 
* [Key Vault 復原][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

檢視 Azure Key Vault 範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)。 