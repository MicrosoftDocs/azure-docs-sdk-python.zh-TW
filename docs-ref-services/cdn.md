---
title: 適用於 Python 的 Azure CDN 程式庫
description: 適用於 Python 的 Azure CDN 程式庫參考
keywords: Azure, python, SDK, API, CDN
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 2dd7703e94a814d85716a7b96994666e32f95565
ms.sourcegitcommit: 3db75daa592da90ea9aa8fd17fb99627a30eb4fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2019
ms.locfileid: "66179876"
---
# <a name="azure-cdn-libraries-for-python"></a><span data-ttu-id="91b63-104">適用於 Python 的 Azure CDN 程式庫</span><span class="sxs-lookup"><span data-stu-id="91b63-104">Azure CDN libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="91b63-105">概觀</span><span class="sxs-lookup"><span data-stu-id="91b63-105">Overview</span></span>

<span data-ttu-id="91b63-106">[Azure 內容傳遞網路 (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) 可讓您提供 web 內容快取，以確保全球各地的高頻寬可用性。</span><span class="sxs-lookup"><span data-stu-id="91b63-106">[Azure Content Delivery Network (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) allows you to provide web content caches to ensure high-bandwidth availability across the globe.</span></span>

<span data-ttu-id="91b63-107">若要開始使用 Azure CDN，請參閱[開始使用 Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint)。</span><span class="sxs-lookup"><span data-stu-id="91b63-107">To get started with Azure CDN, see [Getting started with Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-apis"></a><span data-ttu-id="91b63-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="91b63-108">Management APIs</span></span>

<span data-ttu-id="91b63-109">使用管理 API 建立、查詢及管理 Azure CDN。</span><span class="sxs-lookup"><span data-stu-id="91b63-109">Create, query, and manage Azure CDNs with the management API.</span></span>

<span data-ttu-id="91b63-110">透過 pip 安裝管理套件。</span><span class="sxs-lookup"><span data-stu-id="91b63-110">Install the management package via pip.</span></span>

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a><span data-ttu-id="91b63-111">範例</span><span class="sxs-lookup"><span data-stu-id="91b63-111">Example</span></span>

<span data-ttu-id="91b63-112">使用單一定義端點建立 CDN 設定檔：</span><span class="sxs-lookup"><span data-stu-id="91b63-112">Creating a CDN profile with a single defined endpoint:</span></span>

```python
from azure.mgmt.cdn import CdnManagementClient

cdn_client = CdnManagementClient(credentials, 'your-subscription-id')
profile_poller = cdn_client.profiles.create('my-resource-group',
                                            'cdn-name',
                                            {
                                                "location": "some_region", 
                                                "sku": {
                                                    "name": "sku_tier"
                                                } 
                                            })
profile = profile_poller.result()

endpoint_poller = cdn_client.endpoints.create('my-resource-group',
                                          'cdn-name',
                                          'unique-endpoint-name', 
                                          { 
                                              "location": "any_region", 
                                              "origins": [
                                                  {
                                                      "name": "origin_name", 
                                                      "host_name": "url"
                                                  }]
                                          })
endpoint = endpoint_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="91b63-113">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="91b63-113">Explore the Management APIs</span></span>](/python/api/overview/azure/cdn/management)
