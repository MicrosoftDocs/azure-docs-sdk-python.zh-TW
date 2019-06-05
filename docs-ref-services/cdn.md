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
# <a name="azure-cdn-libraries-for-python"></a>適用於 Python 的 Azure CDN 程式庫

## <a name="overview"></a>概觀

[Azure 內容傳遞網路 (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) 可讓您提供 web 內容快取，以確保全球各地的高頻寬可用性。

若要開始使用 Azure CDN，請參閱[開始使用 Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint)。

## <a name="management-apis"></a>管理 API

使用管理 API 建立、查詢及管理 Azure CDN。

透過 pip 安裝管理套件。

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a>範例

使用單一定義端點建立 CDN 設定檔：

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
> [探索管理 API](/python/api/overview/azure/cdn/management)
