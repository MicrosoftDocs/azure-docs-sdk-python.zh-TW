---
title: 適用於 Python 的 Azure HDInsight SDK
description: 適用於 Python 的 Azure HDInsight SDK 的參考。 適用於 Python 的 Azure HDInsight SDK 提供可讓您管理 HDInsight 叢集的類別和方法。
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 04/10/2019
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 3b0799dd77f7ff447ef997b2d142a6744c4a6858
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534261"
---
# <a name="hdinsight-sdk-for-python"></a>適用於 Python 的 HDInsight SDK

## <a name="overview"></a>概觀

適用於 Python 的 Azure HDInsight SDK 提供可讓您管理 HDInsight 叢集的類別和方法。 它包含用來建立、刪除、更新、列出、調整大小、執行指令碼動作、監視、取得 HDInsight 叢集屬性的作業，和其他多種作業。

## <a name="prerequisites"></a>必要條件

* 一個 Azure 帳戶。 如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)。
* [Python](https://www.python.org/downloads/)
* [pip](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a>SDK 安裝

您可以在 [Python 套件索引](https://pypi.org/project/azure-mgmt-hdinsight/)中找到 適用於 Python 的 HDInsight SDK，然後執行下列命令進行安裝： 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a>Authentication

SDK 必須先使用您的 Azure 訂用帳戶進行驗證。  請依照下列範例建立服務主體，並使用它來驗證。 此動作完成後，您會有 `HDInsightManagementClient` 的執行個體，其中包含許多可用來執行管理作業的方法 (分述於下列各節中)。

> [!NOTE]
> 除了下列範例以外，還有其他方式可進行驗證，可能更符合您的需求。 此處概述所有方法：[使用適用於 Python 的 Azure 管理程式庫來進行驗證](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)

### <a name="authentication-example-using-a-service-principal"></a>使用服務主體的驗證範例

首先，請登入 [Azure Cloud Shell](https://shell.azure.com/bash)。 確認您目前使用的訂用帳戶，是您希望建立服務主體的位置。 

```azurecli-interactive
az account show
```

您的訂用帳戶資訊會以 JSON 的形式顯示。

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

如果您未登入正確的訂用帳戶，請執行下列命令以選取正確的訂用帳戶： 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> 如果您尚未以其他方法註冊 HDInsight 資源提供者 (例如，透過 Azure 入口網站建立 HDInsight 叢集)，您必須立即執行此動作，才能進行驗證。 此動作也可以從 [Azure Cloud Shell](https://shell.azure.com/bash) 完成，只要執行下列命令即可：
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

接下來，請選擇服務主體的名稱，並使用下列命令加以建立：

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

服務主體資訊會顯示為 JSON。

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
複製下列 Python 程式碼片段，並且在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填入在執行建立服務主體的命令後傳回的 JSON 中所包含的字串。

```python
from azure.mgmt.hdinsight import HDInsightManagementClient
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.hdinsight.models import *

# Tenant ID for your Azure Subscription
TENANT_ID = ''
# Your Service Principal App Client ID
CLIENT_ID = ''
# Your Service Principal Client Secret
CLIENT_SECRET = ''
# Your Azure Subscription ID
SUBSCRIPTION_ID = ''

credentials = ServicePrincipalCredentials(
    client_id = CLIENT_ID,
    secret = CLIENT_SECRET,
    tenant = TENANT_ID
)

client = HDInsightManagementClient(credentials, SUBSCRIPTION_ID)
```


## <a name="cluster-management"></a>叢集管理

> [!NOTE]
> 本節假設您已通過驗證並建構 `HDInsightManagementClient` 執行個體，並且將其儲存在名為 `client` 的變數中。 驗證及取得 `HDInsightManagementClient` 的指示可在先前的「驗證」一節中找到。

### <a name="create-a-cluster"></a>建立叢集

新叢集可藉由呼叫 `client.clusters.create()` 來建立。

#### <a name="samples"></a>範例

這裡提供建立數個常見 HDInsight 叢集類型的程式碼範例：[HDInsight Python 範例](https://github.com/Azure-Samples/hdinsight-python-sdk-samples).

#### <a name="example"></a>範例

此範例示範如何使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。

> [!NOTE]
> 您必須先建立資源群組和儲存體帳戶，說明如下。 如果您已建立這些項目，則可以略過這些步驟。

##### <a name="creating-a-resource-group"></a>建立資源群組

您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立資源群組，只要執行下列命令即可
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>建立儲存體帳戶

您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立儲存體帳戶，只要執行下列命令即可：
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
現在請執行下列命令，以取得儲存體帳戶的金鑰 (您需要此金鑰才能建立叢集)：
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
下列 Python 程式碼片段會使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。 請依照註解中的說明填入空白變數，並依據您的特定需求變更其他參數。

```python
# The name for the cluster you are creating
cluster_name = ""
# The name of your existing Resource Group
resource_group_name = ""
# Choose a username
username = ""
# Choose a password
password = ""
# Replace <> with the name of your storage account
storage_account = "<>.blob.core.windows.net"
# Storage account key you obtained above
storage_account_key = ""
# Choose a region
location = ""
container = "default"

params = ClusterCreateProperties(
    cluster_version="3.6",
    os_type=OSType.linux,
    tier=Tier.standard,
    cluster_definition=ClusterDefinition(
        kind="spark",
        configurations={
            "gateway": {
                "restAuthCredential.enabled_credential": "True",
                "restAuthCredential.username": username,
                "restAuthCredential.password": password
            }
        }
    ),
    compute_profile=ComputeProfile(
        roles=[
            Role(
                name="headnode",
                target_instance_count=2,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            ),
            Role(
                name="workernode",
                target_instance_count=1,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            )
        ]
    ),
    storage_profile=StorageProfile(
        storageaccounts=[StorageAccount(
            name=storage_account,
            key=storage_account_key,
            container=container,
            is_default=True
        )]
    )
)

client.clusters.create(
    cluster_name=cluster_name,
    resource_group_name=resource_group_name,
    parameters=ClusterCreateParametersExtended(
        location=location,
        tags={},
        properties=params
    ))
```

### <a name="get-cluster-details"></a>取得叢集詳細資料

若要取得特定叢集的屬性：

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>範例

您可以使用 `get` 來確認您已成功建立叢集。

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

輸出應會顯示如下：

```output
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a>列出叢集

#### <a name="list-clusters-under-the-subscription"></a>列出訂用帳戶下的叢集

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a>依資源群組列出叢集

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> `list()` 和 `list_by_resource_group()` 都會傳回 `ClusterPaged` 物件。 呼叫 `advance_page()` 會傳回該頁面上的叢集清單，並將 `ClusterPaged` 物件往前送到下一個頁面。 這可以重複到出現 `StopIteration` 例外狀況為止，表示沒有更多的頁面。

#### <a name="example"></a>範例

下列範例會列印目前訂用帳戶所有叢集的屬性：

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a>刪除叢集

若要刪除叢集：

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>更新叢集標記

您可以更新指定叢集的標記，如下所示：

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a>範例

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="resize-cluster"></a>調整叢集大小

您可以藉由指定新的大小來調整指定叢集的背景工作節點數目，如下所示：

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a>叢集監視

HDInsight 管理 SDK 也可用來透過 Operations Management Suite (OMS) 管理您對叢集的監視。

### <a name="enable-oms-monitoring"></a>啟用 OMS 監視

> [!NOTE]
> 若要啟用 OMS 監視，您必須擁有現有的 Log Analytics 工作區。 如果您尚未建立工作區，您可以在此了解如何建立：[在 Azure 入口網站中建立 Log Analytics 工作區](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)。

若要對您的叢集啟用 OMS 監視：

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a>檢視 OMS 監視的狀態

若要取得叢集的 OMS 狀態：

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>停用 OMS 監視

若要對您的叢集停用 OMS：

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a>指令碼動作

HDInsight 提供名為指令碼動作的設定方法，此方法會叫用自訂指令碼來自訂叢集。
> [!NOTE]
> 您可以在此處找到如何使用指令碼動作的詳細資訊：[使用指令碼動作自訂 Linux 型 HDInsight 叢集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>執行指令碼動作
若要對指定的叢集執行指令碼動作：

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a>刪除指令碼動作

若要刪除對給定叢集指定的持續性指令碼動作：

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>列出持續性指令碼動作

> [!NOTE]
> `list()` 和 `list_persisted_scripts()` 會傳回 `RuntimeScriptActionDetailPaged` 物件。 呼叫 `advance_page()` 會傳回該頁面上的 `RuntimeScriptActionDetail` 清單，並將 `RuntimeScriptActionDetailPaged` 物件往前送到下一個頁面。 這可以重複到出現 `StopIteration` 例外狀況為止，表示沒有更多的頁面。 請參閱下方的範例。

若要列出指定叢集的所有持續性指令碼動作：
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>範例

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a>列出所有指令碼的執行歷程記錄

若要針對指定的叢集列出所有指令碼的執行歷程記錄：

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>範例

此範例會列印過去所有指令碼執行的所有詳細資料。

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
