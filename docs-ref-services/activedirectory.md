---
title: "適用於 Python 的 Azure Active Directory 程式庫"
description: "適用於 Azure Active Directory 之 Python 用戶端程式庫的參考文件"
keywords: "Azure, Python, SDK, API, SQL, 驗證, AAD, Active Directory , Graph, OAuth 2.0"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/07/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: active-directory
ms.openlocfilehash: 78df70001dd0d55ac2c9c9da04fac6a51c5919e6
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="azure-active-directory-libraries-for-python"></a>適用於 Python 的 Azure Active Directory 程式庫

## <a name="overview"></a>概觀

使用 [Azure Active Directory](/azure/active-directory/active-directory-whatis) 登入使用者並控制應用程式及 API 的存取。

## <a name="client-library"></a>用戶端程式庫

使用[適用於 Python 的 Azure Active Directory 驗證程式庫 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 來設定 OAuth2、OpenID Connect 或 Active Directory Graph 驗證和 [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) 單一登入。

```bash
pip install azure-graphrbac
```

### <a name="example"></a>範例
> [!NOTE]
> 您在建立認證執行個體時，必須將資源參數變更為 https://graph.windows.net

```python
from azure.graphrbac import GraphRbacManagementClient
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
            'user@domain.com',      # Your user
            'my_password',          # Your password
            resource="https://graph.windows.net"
    )

tenant_id = "myad.onmicrosoft.com"

graphrbac_client = GraphRbacManagementClient(
    credentials,
    tenant_id
)
```
下列程式碼會建立使用者、直接取得並依清單篩選，然後再將它刪除。
```python
from azure.graphrbac.models import UserCreateParameters, PasswordProfile

user = graphrbac_client.users.create(
    UserCreateParameters(
        user_principal_name="testbuddy@{}".format(MY_AD_DOMAIN),
        account_enabled=False,
        display_name='Test Buddy',
        mail_nickname='testbuddy',
        password_profile=PasswordProfile(
            password='MyStr0ngP4ssword',
            force_change_password_next_login=True
        )
    )
)
# user is a User instance
self.assertEqual(user.display_name, 'Test Buddy')

user = graphrbac_client.users.get(user.object_id)
self.assertEqual(user.display_name, 'Test Buddy')

for user in graphrbac_client.users.list(filter="displayName eq 'Test Buddy'"):
    self.assertEqual(user.display_name, 'Test Buddy')

graphrbac_client.users.delete(user.object_id)
```

> [!div class="nextstepaction"]
> [探索用戶端 API](/python/api/overview/azure/activedirectory/client)

深入探索可在應用程式中使用的 [Azure AD Python 程式碼範例](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python)。