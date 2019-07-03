---
title: 適用於 Python 的 Graph RBAC 程式庫
description: 適用於 Python 的 Graph RBAC 程式庫的參考資料
keywords: Azure, python, SDK, API, Graph RBAC
author: rloutlaw
ms.author: routlaw
manager: jfriend
ms.date: 05/10/2019
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: e9b0aba7998565284ae18e0036da96d033b2b05f
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534270"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a>適用於 Python 的 Azure Active Directory Graph 程式庫

> [!IMPORTANT]
>
> 從 2019 年 2 月起，我們展開了將某些舊版 Azure Active Directory Graph API 汰換為 Microsoft Graph API 的程序。 
>
> 如需詳細資料、更新及時間範圍，請參閱 Office 開發人員中心的 [Microsoft Graph 或 Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph)。
>
> 應用程式於未來皆應該使用 Microsoft Graph API。 

## <a name="overview"></a>概觀 

使用 [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-api) 登入使用者並控制應用程式及 API 的存取。    

## <a name="client-library"></a>用戶端程式庫   

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