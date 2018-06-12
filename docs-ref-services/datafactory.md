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
# <a name="azure-data-factory-libraries-for-python"></a><span data-ttu-id="733c1-103">適用於 Python 的 Azure Data Factory 程式庫</span><span class="sxs-lookup"><span data-stu-id="733c1-103">Azure Data Factory libraries for Python</span></span>

<span data-ttu-id="733c1-104">使用 [Azure Data Factory](/azure/data-factory/) 將資料儲存、移動和處理服務撰寫到自動化的資料管線中</span><span class="sxs-lookup"><span data-stu-id="733c1-104">Compose data storage, movement, and processing services into automated data pipelines with [Azure Data Factory](/azure/data-factory/)</span></span>

<span data-ttu-id="733c1-105">[深入了解](/azure/data-factory/introduction) Data Factory，以及透過[使用 Python 建立資料處理站和管線的快速入門](/azure/data-factory/quickstart-create-data-factory-python)開始第一步。</span><span class="sxs-lookup"><span data-stu-id="733c1-105">[Learn more](/azure/data-factory/introduction) about Data Factory and get started with the [Create a data factory and pipeline using Python quickstart](/azure/data-factory/quickstart-create-data-factory-python).</span></span> 

## <a name="management-module"></a><span data-ttu-id="733c1-106">管理模組</span><span class="sxs-lookup"><span data-stu-id="733c1-106">Management module</span></span>

<span data-ttu-id="733c1-107">使用管理模組在訂用帳戶中建立和管理 Data Factory 執行個體。</span><span class="sxs-lookup"><span data-stu-id="733c1-107">Create and manage Data Factory instances in your subscription with the management module.</span></span>

### <a name="installation"></a><span data-ttu-id="733c1-108">安裝</span><span class="sxs-lookup"><span data-stu-id="733c1-108">Installation</span></span>

<span data-ttu-id="733c1-109">使用 [pip](https://pip.pypa.io/en/stable/quickstart/) 安裝套件：</span><span class="sxs-lookup"><span data-stu-id="733c1-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a><span data-ttu-id="733c1-110">範例</span><span class="sxs-lookup"><span data-stu-id="733c1-110">Example</span></span> 

<span data-ttu-id="733c1-111">在美國東部地區的訂用帳戶中建立 Data Factory。</span><span class="sxs-lookup"><span data-stu-id="733c1-111">Create a Data Factory in your subscription on the East US region.</span></span>

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
> [<span data-ttu-id="733c1-112">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="733c1-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datafactory/management)