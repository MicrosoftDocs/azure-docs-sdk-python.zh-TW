---
title: 適用於 Python 的 Azure Web Apps 程式庫
description: ''
keywords: Azure, Python, SDK, API, Web Apps, App Service
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 4870394c6ee39cde546d090fa1a0d136609851b3
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376697"
---
# <a name="azure-web-apps-libraries-for-python"></a>適用於 Python 的 Azure Web Apps 程式庫

## <a name="overview"></a>概觀

使用 [Azure App Service](/azure/app-service) 來部署及調整網站、Web 應用程式、服務和 REST API。

若要開始使用 Azure App Service，請參閱[在 Azure 中建立 Python Web 應用程式](/azure/app-service-web/app-service-web-get-started-python)。

## <a name="management-api"></a>管理 API

使用管理 API 來部署、管理及調整裝載在 Azure App Service 中的元素。

透過 pip 安裝程式庫。

```bash
pip install azure-mgmt-web
```

### <a name="example"></a>範例

從 GitHub 存放庫將 webapp 部署到 Azure Web 應用程式。

```python
siteConfiguration = SiteConfig(
    python_version='3.4'
)

# create a web app
web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=SERVICE_PLAN_ID,
        site_config=siteConfiguration
    )
)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url='https://github.com/lisawong19/python-docs-hello-world',
        branch='master'
    )
)
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/webapps/management)

## <a name="samples"></a>範例

* [使用 Python 管理 Azure 網站][1]
* [建立邏輯應用程式工作流程][2]

檢視 Web 應用程式範例的[完整清單](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app)。

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md
