---
title: "適用於 Python 的 Azure DNS 程式庫"
description: "適用於 Python 的 Azure DNS 程式庫參考"
keywords: Azure, python, SDK, API, DNS
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 59ae3628b4a810db8fee21aacf46c13054dc8cd3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-dns-libraries-for-python"></a>適用於 Python 的 Azure DNS 程式庫

## <a name="overview"></a>概觀

[Azure DNS](/azure/dns/dns-overview) 是 DNS 網域的主機服務，可透過 Azure 基礎結構提供 DNS 解析。

若要開始使用 Azure DNS，請參閱[利用 Azure 入口網站開始使用 Azure DNS](/azure/dns/dns-getstarted-portal)。

## <a name="management-apis"></a>管理 API

使用管理 API 建立和管理 DNS 區域及記錄。

使用 pip 安裝管理套件。

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a>範例

建立新的 DNS 區域。

```python
from azure.mgmt.dns import DnsManagementClient

dns_client = DnsManagementClient(credentials, 'your-subscription-id')
zone = dns_client.zones.create_or_update('resource-group',
                                         'zone_name_no_dot',
                                         {
                                            "location": "global"
                                         })

```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/dns/managementlibrary)
