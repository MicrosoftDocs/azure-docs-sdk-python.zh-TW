---
title: 適用於 Python 的 Azure Data Factory 程式庫
description: 適用於 Python 的 Azure Data Factory 程式庫參考
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 05/10/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 05d93f7d1838c110c3ba77f7abd3967f7870774b
ms.sourcegitcommit: d65549030a0edb50d75482f79aac0962d1dacb57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2018
ms.locfileid: "34759043"
---
# <a name="azure-data-factory-libraries-for-python"></a>適用於 Python 的 Azure Data Factory 程式庫

使用 [Azure Data Factory](/azure/data-factory/) 將資料儲存、移動和處理服務撰寫到自動化的資料管線中

[深入了解](/azure/data-factory/introduction) Data Factory，以及透過[使用 Python 建立資料處理站和管線的快速入門](/azure/data-factory/quickstart-create-data-factory-python)開始第一步。 

## <a name="management-module"></a>管理模組

使用管理模組在訂用帳戶中建立和管理 Data Factory 執行個體。

### <a name="installation"></a>安裝

使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 安裝套件：

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a>範例 

在美國東部地區的訂用帳戶中建立 Data Factory。

```python
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.datafactory import DataFactoryManagementClient
from azure.mgmt.datafactory.models import *
import time

#Create a data factory
subscription_id = '<Specify your Azure Subscription ID>'
credentials = ServicePrincipalCredentials(client_id='<Active Directory application/client ID>', secret='<client secret>', tenant='<Active Directory tenant ID>')
adf_client = DataFactoryManagementClient(credentials, subscription_id)

rg_params = {'location':'eastus'}
df_params = {'location':'eastus'}  

df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
```

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/datafactory/management)