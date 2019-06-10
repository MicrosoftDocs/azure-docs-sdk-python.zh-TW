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
ms.openlocfilehash: 27238e00463ae30ec0e47e8c18497ffb9edac62c
ms.sourcegitcommit: 253c8d4b3dbc2bb76d1a273a757ab96ba37617a1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2019
ms.locfileid: "65731534"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a><span data-ttu-id="b911d-104">適用於 Python 的 Azure Active Directory Graph 程式庫</span><span class="sxs-lookup"><span data-stu-id="b911d-104">Azure Active Directory Graph libraries for Python</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="b911d-105">從 2019 年 2 月起，我們展開了將某些舊版 Azure Active Directory Graph API 汰換為 Microsoft Graph API 的程序。</span><span class="sxs-lookup"><span data-stu-id="b911d-105">As of February 2019, we started the process to deprecate some earlier versions of Azure Active Directory Graph API in favor of the Microsoft Graph API.</span></span> 
>
> <span data-ttu-id="b911d-106">如需詳細資料、更新及時間範圍，請參閱 Office 開發人員中心的 [Microsoft Graph 或 Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph)。</span><span class="sxs-lookup"><span data-stu-id="b911d-106">For details, updates, and time frames, see [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in the Office Dev Center.</span></span>
>
> <span data-ttu-id="b911d-107">應用程式於未來皆應該使用 Microsoft Graph API。</span><span class="sxs-lookup"><span data-stu-id="b911d-107">Moving forward, applications should use the Microsoft Graph API.</span></span> 

## <a name="overview"></a><span data-ttu-id="b911d-108">概觀</span><span class="sxs-lookup"><span data-stu-id="b911d-108">Overview</span></span> 

<span data-ttu-id="b911d-109">使用 [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-apis) 登入使用者並控制應用程式及 API 的存取。</span><span class="sxs-lookup"><span data-stu-id="b911d-109">Sign-on users and control access to applications and APIs with [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-apis).</span></span>   

## <a name="client-library"></a><span data-ttu-id="b911d-110">用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="b911d-110">Client library</span></span>   

 ```bash    
pip install azure-graphrbac 
``` 

### <a name="example"></a><span data-ttu-id="b911d-111">範例</span><span class="sxs-lookup"><span data-stu-id="b911d-111">Example</span></span> 
> [!NOTE]   
> <span data-ttu-id="b911d-112">您在建立認證執行個體時，必須將資源參數變更為 https://graph.windows.net</span><span class="sxs-lookup"><span data-stu-id="b911d-112">You need to change the resource parameter to https://graph.windows.net while creating the credentials instance</span></span>    
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
<span data-ttu-id="b911d-113">下列程式碼會建立使用者、直接取得並依清單篩選，然後再將它刪除。</span><span class="sxs-lookup"><span data-stu-id="b911d-113">The following code creates a user, get it directly and by list filtering, and then delete it.</span></span>   
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