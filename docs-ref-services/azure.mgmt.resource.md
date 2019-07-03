---
title: 適用於 Python 的 Azure 資源庫
description: ''
keywords: Azure, Python, SDK, API, 資源
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: d708a5e7296b166b6e55b9b7b0d995e72e264267
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534373"
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="fbcae-103">適用於 Python 的 Azure 資源庫</span><span class="sxs-lookup"><span data-stu-id="fbcae-103">Azure Resources libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="fbcae-104">概觀</span><span class="sxs-lookup"><span data-stu-id="fbcae-104">Overview</span></span> 
<span data-ttu-id="fbcae-105">在資源群組中管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="fbcae-105">Manage Azure resources in resource groups.</span></span>

| <span data-ttu-id="fbcae-106">Package</span><span class="sxs-lookup"><span data-stu-id="fbcae-106">Package</span></span>  |  <span data-ttu-id="fbcae-107">說明</span><span class="sxs-lookup"><span data-stu-id="fbcae-107">Description</span></span> |
|---|---|
|<span data-ttu-id="fbcae-108">[azure.mgmt.resource.features][1]</span><span class="sxs-lookup"><span data-stu-id="fbcae-108">[azure.mgmt.resource.features][1]</span></span>|<span data-ttu-id="fbcae-109">Azure 功能控制項 (AFEC) 提供一種機制，讓資源提供者可控制要向使用者曝光的功能。</span><span class="sxs-lookup"><span data-stu-id="fbcae-109">Azure Feature Exposure Control (AFEC) provides a mechanism for the resource providers to control feature exposure to users.</span></span>|
|<span data-ttu-id="fbcae-110">[azure.mgmt.resource.links][2]</span><span class="sxs-lookup"><span data-stu-id="fbcae-110">[azure.mgmt.resource.links][2]</span></span>|<span data-ttu-id="fbcae-111">可將 Azure 資源連結在一起來構成邏輯關聯性。</span><span class="sxs-lookup"><span data-stu-id="fbcae-111">Azure resources can be linked together to form logical relationships.</span></span> <span data-ttu-id="fbcae-112">您可以在屬於不同資源群組的資源之間建立連結。</span><span class="sxs-lookup"><span data-stu-id="fbcae-112">You can establish links between resources belonging to different resource groups.</span></span>|
|<span data-ttu-id="fbcae-113">[azure.mgmt.resource.locks][3]</span><span class="sxs-lookup"><span data-stu-id="fbcae-113">[azure.mgmt.resource.locks][3]</span></span>|<span data-ttu-id="fbcae-114">可以鎖定 Azure 資源來防止貴組織中的其他使用者刪除或修改資源。</span><span class="sxs-lookup"><span data-stu-id="fbcae-114">Azure resources can be locked to prevent other users in your organization from deleting or modifying resources.</span></span>|
|<span data-ttu-id="fbcae-115">[azure.mgmt.resource.managedapplications][4]</span><span class="sxs-lookup"><span data-stu-id="fbcae-115">[azure.mgmt.resource.managedapplications][4]</span></span>|<span data-ttu-id="fbcae-116">Azure 受控應用程式 (設備)。</span><span class="sxs-lookup"><span data-stu-id="fbcae-116">ARM managed applications (appliances).</span></span>|
|<span data-ttu-id="fbcae-117">[azure.mgmt.resource.policy][5]</span><span class="sxs-lookup"><span data-stu-id="fbcae-117">[azure.mgmt.resource.policy][5]</span></span>|<span data-ttu-id="fbcae-118">若要管理和控制資源的存取權，您可以定義自訂的原則，並在範圍內將它們加以指派。</span><span class="sxs-lookup"><span data-stu-id="fbcae-118">To manage and control access to your resources, you can define customized policies and assign them at a scope.</span></span>|
|<span data-ttu-id="fbcae-119">[azure.mgmt.resource.resources][6]</span><span class="sxs-lookup"><span data-stu-id="fbcae-119">[azure.mgmt.resource.resources][6]</span></span>| <span data-ttu-id="fbcae-120">提供使用資源與資源群組的作業。</span><span class="sxs-lookup"><span data-stu-id="fbcae-120">Provides operations for working with resources and resource groups.</span></span>|
|<span data-ttu-id="fbcae-121">[azure.mgmt.resource.subscriptions][7]</span><span class="sxs-lookup"><span data-stu-id="fbcae-121">[azure.mgmt.resource.subscriptions][7]</span></span>|<span data-ttu-id="fbcae-122">所有資源群組和資源都在訂用帳戶內。</span><span class="sxs-lookup"><span data-stu-id="fbcae-122">All resource groups and resources exist within subscriptions.</span></span> <span data-ttu-id="fbcae-123">這些作業可讓您取得訂用帳戶和租用戶的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="fbcae-123">These operation enable you get information about your subscriptions and tenants.</span></span>|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a><span data-ttu-id="fbcae-124">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="fbcae-124">Install the libraries</span></span> 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="fbcae-125">範例</span><span class="sxs-lookup"><span data-stu-id="fbcae-125">Example</span></span>
<span data-ttu-id="fbcae-126">下列範例說明如何建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="fbcae-126">The following is an example of how to create a resource group.</span></span> 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

<span data-ttu-id="fbcae-127">深入探索可在應用程式中使用的 [Python 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=python)。</span><span class="sxs-lookup"><span data-stu-id="fbcae-127">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span> 