---
title: 適用於 Python 的 Azure SDK 作業組態
description: 適用於 Python 的 Azure SDK 擲回的 C
keywords: Azure, Python, SDK, API, 例外狀況
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 03/07/2018
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: adeb6aa8a2c363c3119e97503df9536fb0633b4c
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376869"
---
# <a name="operation-config"></a><span data-ttu-id="a5de6-104">作業組態</span><span class="sxs-lookup"><span data-stu-id="a5de6-104">Operation config</span></span> 

<span data-ttu-id="a5de6-105">作業的方法有可在 `kwargs` 中提供的額外參數。</span><span class="sxs-lookup"><span data-stu-id="a5de6-105">Methods on operations have extra parameters which can be provided in the `kwargs`.</span></span> <span data-ttu-id="a5de6-106">這稱為 operation_config。</span><span class="sxs-lookup"><span data-stu-id="a5de6-106">This is called operation_config.</span></span>

<span data-ttu-id="a5de6-107">作業組態的選項如下：</span><span class="sxs-lookup"><span data-stu-id="a5de6-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="a5de6-108">參數名稱</span><span class="sxs-lookup"><span data-stu-id="a5de6-108">Parameter name</span></span>|<span data-ttu-id="a5de6-109">類型</span><span class="sxs-lookup"><span data-stu-id="a5de6-109">Type</span></span>|<span data-ttu-id="a5de6-110">角色</span><span class="sxs-lookup"><span data-stu-id="a5de6-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="a5de6-111">驗證</span><span class="sxs-lookup"><span data-stu-id="a5de6-111">verify</span></span> |`bool`|<span data-ttu-id="a5de6-112">是否要驗證 SSL 憑證。</span><span class="sxs-lookup"><span data-stu-id="a5de6-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="a5de6-113">預設值是 True。</span><span class="sxs-lookup"><span data-stu-id="a5de6-113">Default is True.</span></span>|
|  <span data-ttu-id="a5de6-114">cert</span><span class="sxs-lookup"><span data-stu-id="a5de6-114">cert</span></span> |`str`| <span data-ttu-id="a5de6-115">進行用戶端驗證的本機憑證路徑。</span><span class="sxs-lookup"><span data-stu-id="a5de6-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="a5de6-116">timeout</span><span class="sxs-lookup"><span data-stu-id="a5de6-116">timeout</span></span> |`int`| <span data-ttu-id="a5de6-117">建立伺服器連線逾時 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="a5de6-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="a5de6-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="a5de6-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="a5de6-119">是否允許重新導向。</span><span class="sxs-lookup"><span data-stu-id="a5de6-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="a5de6-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="a5de6-120">max_redirects</span></span>  |`int`| <span data-ttu-id="a5de6-121">允許的重新導向數目上限。</span><span class="sxs-lookup"><span data-stu-id="a5de6-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="a5de6-122">Proxy</span><span class="sxs-lookup"><span data-stu-id="a5de6-122">proxies</span></span>  |`dict` |<span data-ttu-id="a5de6-123">Proxy 伺服器設定。</span><span class="sxs-lookup"><span data-stu-id="a5de6-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="a5de6-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="a5de6-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="a5de6-125">是否從本機環境變數中讀取 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="a5de6-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="a5de6-126">重試</span><span class="sxs-lookup"><span data-stu-id="a5de6-126">retries</span></span>  |`int` | <span data-ttu-id="a5de6-127">嘗試重試的次數。</span><span class="sxs-lookup"><span data-stu-id="a5de6-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="a5de6-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="a5de6-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="a5de6-129">啟用偵錯模式中的的 HTTP 記錄 (預設為 False)。</span><span class="sxs-lookup"><span data-stu-id="a5de6-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
