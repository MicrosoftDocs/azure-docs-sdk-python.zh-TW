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
# <a name="hdinsight-sdk-for-python"></a><span data-ttu-id="bca62-104">適用於 Python 的 HDInsight SDK</span><span class="sxs-lookup"><span data-stu-id="bca62-104">HDInsight SDK for Python</span></span>

## <a name="overview"></a><span data-ttu-id="bca62-105">概觀</span><span class="sxs-lookup"><span data-stu-id="bca62-105">Overview</span></span>

<span data-ttu-id="bca62-106">適用於 Python 的 Azure HDInsight SDK 提供可讓您管理 HDInsight 叢集的類別和方法。</span><span class="sxs-lookup"><span data-stu-id="bca62-106">The HDInsight SDK for Python provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="bca62-107">它包含用來建立、刪除、更新、列出、調整大小、執行指令碼動作、監視、取得 HDInsight 叢集屬性的作業，和其他多種作業。</span><span class="sxs-lookup"><span data-stu-id="bca62-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bca62-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="bca62-108">Prerequisites</span></span>

* <span data-ttu-id="bca62-109">一個 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="bca62-109">An Azure account.</span></span> <span data-ttu-id="bca62-110">如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="bca62-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* [<span data-ttu-id="bca62-111">Python</span><span class="sxs-lookup"><span data-stu-id="bca62-111">Python</span></span>](https://www.python.org/downloads/)
* [<span data-ttu-id="bca62-112">pip</span><span class="sxs-lookup"><span data-stu-id="bca62-112">pip</span></span>](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a><span data-ttu-id="bca62-113">SDK 安裝</span><span class="sxs-lookup"><span data-stu-id="bca62-113">SDK Installation</span></span>

<span data-ttu-id="bca62-114">您可以在 [Python 套件索引](https://pypi.org/project/azure-mgmt-hdinsight/)中找到 適用於 Python 的 HDInsight SDK，然後執行下列命令進行安裝：</span><span class="sxs-lookup"><span data-stu-id="bca62-114">The HDInsight SDK for Python can be found in the [Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) and can be installed by running:</span></span> 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a><span data-ttu-id="bca62-115">Authentication</span><span class="sxs-lookup"><span data-stu-id="bca62-115">Authentication</span></span>

<span data-ttu-id="bca62-116">SDK 必須先使用您的 Azure 訂用帳戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="bca62-116">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="bca62-117">請依照下列範例建立服務主體，並使用它來驗證。</span><span class="sxs-lookup"><span data-stu-id="bca62-117">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="bca62-118">此動作完成後，您會有 `HDInsightManagementClient` 的執行個體，其中包含許多可用來執行管理作業的方法 (分述於下列各節中)。</span><span class="sxs-lookup"><span data-stu-id="bca62-118">After this is done, you will have an instance of an `HDInsightManagementClient`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="bca62-119">除了下列範例以外，還有其他方式可進行驗證，可能更符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="bca62-119">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="bca62-120">此處概述所有方法：[使用適用於 Python 的 Azure 管理程式庫來進行驗證](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)</span><span class="sxs-lookup"><span data-stu-id="bca62-120">All methods are outlined here: [Authenticate with the Azure Management Libraries for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="bca62-121">使用服務主體的驗證範例</span><span class="sxs-lookup"><span data-stu-id="bca62-121">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="bca62-122">首先，請登入 [Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="bca62-122">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="bca62-123">確認您目前使用的訂用帳戶，是您希望建立服務主體的位置。</span><span class="sxs-lookup"><span data-stu-id="bca62-123">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="bca62-124">您的訂用帳戶資訊會以 JSON 的形式顯示。</span><span class="sxs-lookup"><span data-stu-id="bca62-124">Your subscription information is displayed as JSON.</span></span>

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

<span data-ttu-id="bca62-125">如果您未登入正確的訂用帳戶，請執行下列命令以選取正確的訂用帳戶：</span><span class="sxs-lookup"><span data-stu-id="bca62-125">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="bca62-126">如果您尚未以其他方法註冊 HDInsight 資源提供者 (例如，透過 Azure 入口網站建立 HDInsight 叢集)，您必須立即執行此動作，才能進行驗證。</span><span class="sxs-lookup"><span data-stu-id="bca62-126">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="bca62-127">此動作也可以從 [Azure Cloud Shell](https://shell.azure.com/bash) 完成，只要執行下列命令即可：</span><span class="sxs-lookup"><span data-stu-id="bca62-127">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="bca62-128">接下來，請選擇服務主體的名稱，並使用下列命令加以建立：</span><span class="sxs-lookup"><span data-stu-id="bca62-128">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="bca62-129">服務主體資訊會顯示為 JSON。</span><span class="sxs-lookup"><span data-stu-id="bca62-129">The service principal information is displayed as JSON.</span></span>

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
<span data-ttu-id="bca62-130">複製下列 Python 程式碼片段，並且在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填入在執行建立服務主體的命令後傳回的 JSON 中所包含的字串。</span><span class="sxs-lookup"><span data-stu-id="bca62-130">Copy the below Python snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

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


## <a name="cluster-management"></a><span data-ttu-id="bca62-131">叢集管理</span><span class="sxs-lookup"><span data-stu-id="bca62-131">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="bca62-132">本節假設您已通過驗證並建構 `HDInsightManagementClient` 執行個體，並且將其儲存在名為 `client` 的變數中。</span><span class="sxs-lookup"><span data-stu-id="bca62-132">This section assumes you have already authenticated and constructed an `HDInsightManagementClient` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="bca62-133">驗證及取得 `HDInsightManagementClient` 的指示可在先前的「驗證」一節中找到。</span><span class="sxs-lookup"><span data-stu-id="bca62-133">Instructions for authenticating and obtaining an `HDInsightManagementClient` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="bca62-134">建立叢集</span><span class="sxs-lookup"><span data-stu-id="bca62-134">Create a Cluster</span></span>

<span data-ttu-id="bca62-135">新叢集可藉由呼叫 `client.clusters.create()` 來建立。</span><span class="sxs-lookup"><span data-stu-id="bca62-135">A new cluster can be created by calling `client.clusters.create()`.</span></span>

#### <a name="samples"></a><span data-ttu-id="bca62-136">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-136">Samples</span></span>

<span data-ttu-id="bca62-137">這裡提供建立數個常見 HDInsight 叢集類型的程式碼範例：[HDInsight Python 範例](https://github.com/Azure-Samples/hdinsight-python-sdk-samples).</span><span class="sxs-lookup"><span data-stu-id="bca62-137">Code samples for creating several common types of HDInsight clusters are available: [HDInsight Python Samples](https://github.com/Azure-Samples/hdinsight-python-sdk-samples).</span></span>

#### <a name="example"></a><span data-ttu-id="bca62-138">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-138">Example</span></span>

<span data-ttu-id="bca62-139">此範例示範如何使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。</span><span class="sxs-lookup"><span data-stu-id="bca62-139">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="bca62-140">您必須先建立資源群組和儲存體帳戶，說明如下。</span><span class="sxs-lookup"><span data-stu-id="bca62-140">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="bca62-141">如果您已建立這些項目，則可以略過這些步驟。</span><span class="sxs-lookup"><span data-stu-id="bca62-141">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="bca62-142">建立資源群組</span><span class="sxs-lookup"><span data-stu-id="bca62-142">Creating a Resource Group</span></span>

<span data-ttu-id="bca62-143">您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立資源群組，只要執行下列命令即可</span><span class="sxs-lookup"><span data-stu-id="bca62-143">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="bca62-144">建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="bca62-144">Creating a Storage Account</span></span>

<span data-ttu-id="bca62-145">您可以使用 [Azure Cloud Shell](https://shell.azure.com/bash) 建立儲存體帳戶，只要執行下列命令即可：</span><span class="sxs-lookup"><span data-stu-id="bca62-145">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="bca62-146">現在請執行下列命令，以取得儲存體帳戶的金鑰 (您需要此金鑰才能建立叢集)：</span><span class="sxs-lookup"><span data-stu-id="bca62-146">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="bca62-147">下列 Python 程式碼片段會使用 2 個前端節點和 1 個背景工作節點來建立 Spark 叢集。</span><span class="sxs-lookup"><span data-stu-id="bca62-147">The below Python snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="bca62-148">請依照註解中的說明填入空白變數，並依據您的特定需求變更其他參數。</span><span class="sxs-lookup"><span data-stu-id="bca62-148">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

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

### <a name="get-cluster-details"></a><span data-ttu-id="bca62-149">取得叢集詳細資料</span><span class="sxs-lookup"><span data-stu-id="bca62-149">Get Cluster Details</span></span>

<span data-ttu-id="bca62-150">若要取得特定叢集的屬性：</span><span class="sxs-lookup"><span data-stu-id="bca62-150">To get properties for a given cluster:</span></span>

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="bca62-151">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-151">Example</span></span>

<span data-ttu-id="bca62-152">您可以使用 `get` 來確認您已成功建立叢集。</span><span class="sxs-lookup"><span data-stu-id="bca62-152">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

<span data-ttu-id="bca62-153">輸出應會顯示如下：</span><span class="sxs-lookup"><span data-stu-id="bca62-153">The output should look like:</span></span>

```output
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a><span data-ttu-id="bca62-154">列出叢集</span><span class="sxs-lookup"><span data-stu-id="bca62-154">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="bca62-155">列出訂用帳戶下的叢集</span><span class="sxs-lookup"><span data-stu-id="bca62-155">List Clusters Under The Subscription</span></span>

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="bca62-156">依資源群組列出叢集</span><span class="sxs-lookup"><span data-stu-id="bca62-156">List Clusters By Resource Group</span></span>

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> <span data-ttu-id="bca62-157">`list()` 和 `list_by_resource_group()` 都會傳回 `ClusterPaged` 物件。</span><span class="sxs-lookup"><span data-stu-id="bca62-157">Both `list()` and `list_by_resource_group()` return a `ClusterPaged` object.</span></span> <span data-ttu-id="bca62-158">呼叫 `advance_page()` 會傳回該頁面上的叢集清單，並將 `ClusterPaged` 物件往前送到下一個頁面。</span><span class="sxs-lookup"><span data-stu-id="bca62-158">Calling `advance_page()` returns a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="bca62-159">這可以重複到出現 `StopIteration` 例外狀況為止，表示沒有更多的頁面。</span><span class="sxs-lookup"><span data-stu-id="bca62-159">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="bca62-160">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-160">Example</span></span>

<span data-ttu-id="bca62-161">下列範例會列印目前訂用帳戶所有叢集的屬性：</span><span class="sxs-lookup"><span data-stu-id="bca62-161">The following example prints the properties of all clusters for the current subscription:</span></span>

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a><span data-ttu-id="bca62-162">刪除叢集</span><span class="sxs-lookup"><span data-stu-id="bca62-162">Delete a Cluster</span></span>

<span data-ttu-id="bca62-163">若要刪除叢集：</span><span class="sxs-lookup"><span data-stu-id="bca62-163">To delete a cluster:</span></span>

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a><span data-ttu-id="bca62-164">更新叢集標記</span><span class="sxs-lookup"><span data-stu-id="bca62-164">Update Cluster Tags</span></span>

<span data-ttu-id="bca62-165">您可以更新指定叢集的標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bca62-165">You can update the tags of a given cluster like so:</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a><span data-ttu-id="bca62-166">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-166">Example</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="resize-cluster"></a><span data-ttu-id="bca62-167">調整叢集大小</span><span class="sxs-lookup"><span data-stu-id="bca62-167">Resize Cluster</span></span>

<span data-ttu-id="bca62-168">您可以藉由指定新的大小來調整指定叢集的背景工作節點數目，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bca62-168">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="bca62-169">叢集監視</span><span class="sxs-lookup"><span data-stu-id="bca62-169">Cluster Monitoring</span></span>

<span data-ttu-id="bca62-170">HDInsight 管理 SDK 也可用來透過 Operations Management Suite (OMS) 管理您對叢集的監視。</span><span class="sxs-lookup"><span data-stu-id="bca62-170">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="bca62-171">啟用 OMS 監視</span><span class="sxs-lookup"><span data-stu-id="bca62-171">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="bca62-172">若要啟用 OMS 監視，您必須擁有現有的 Log Analytics 工作區。</span><span class="sxs-lookup"><span data-stu-id="bca62-172">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="bca62-173">如果您尚未建立工作區，您可以在此了解如何建立：[在 Azure 入口網站中建立 Log Analytics 工作區](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)。</span><span class="sxs-lookup"><span data-stu-id="bca62-173">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="bca62-174">若要對您的叢集啟用 OMS 監視：</span><span class="sxs-lookup"><span data-stu-id="bca62-174">To enable OMS Monitoring on your cluster:</span></span>

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="bca62-175">檢視 OMS 監視的狀態</span><span class="sxs-lookup"><span data-stu-id="bca62-175">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="bca62-176">若要取得叢集的 OMS 狀態：</span><span class="sxs-lookup"><span data-stu-id="bca62-176">To get the status of OMS on your cluster:</span></span>

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="bca62-177">停用 OMS 監視</span><span class="sxs-lookup"><span data-stu-id="bca62-177">Disable OMS Monitoring</span></span>

<span data-ttu-id="bca62-178">若要對您的叢集停用 OMS：</span><span class="sxs-lookup"><span data-stu-id="bca62-178">To disable OMS on your cluster:</span></span>

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a><span data-ttu-id="bca62-179">指令碼動作</span><span class="sxs-lookup"><span data-stu-id="bca62-179">Script Actions</span></span>

<span data-ttu-id="bca62-180">HDInsight 提供名為指令碼動作的設定方法，此方法會叫用自訂指令碼來自訂叢集。</span><span class="sxs-lookup"><span data-stu-id="bca62-180">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="bca62-181">您可以在此處找到如何使用指令碼動作的詳細資訊：[使用指令碼動作自訂 Linux 型 HDInsight 叢集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="bca62-181">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="bca62-182">執行指令碼動作</span><span class="sxs-lookup"><span data-stu-id="bca62-182">Execute Script Actions</span></span>
<span data-ttu-id="bca62-183">若要對指定的叢集執行指令碼動作：</span><span class="sxs-lookup"><span data-stu-id="bca62-183">To execute script actions on a given cluster:</span></span>

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="bca62-184">刪除指令碼動作</span><span class="sxs-lookup"><span data-stu-id="bca62-184">Delete Script Action</span></span>

<span data-ttu-id="bca62-185">若要刪除對給定叢集指定的持續性指令碼動作：</span><span class="sxs-lookup"><span data-stu-id="bca62-185">To delete a specified persisted script action on a given cluster:</span></span>

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="bca62-186">列出持續性指令碼動作</span><span class="sxs-lookup"><span data-stu-id="bca62-186">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="bca62-187">`list()` 和 `list_persisted_scripts()` 會傳回 `RuntimeScriptActionDetailPaged` 物件。</span><span class="sxs-lookup"><span data-stu-id="bca62-187">`list()` and `list_persisted_scripts()` return a `RuntimeScriptActionDetailPaged` object.</span></span> <span data-ttu-id="bca62-188">呼叫 `advance_page()` 會傳回該頁面上的 `RuntimeScriptActionDetail` 清單，並將 `RuntimeScriptActionDetailPaged` 物件往前送到下一個頁面。</span><span class="sxs-lookup"><span data-stu-id="bca62-188">Calling `advance_page()` returns a list of `RuntimeScriptActionDetail` on that page and advances the `RuntimeScriptActionDetailPaged` object to the next page.</span></span> <span data-ttu-id="bca62-189">這可以重複到出現 `StopIteration` 例外狀況為止，表示沒有更多的頁面。</span><span class="sxs-lookup"><span data-stu-id="bca62-189">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span> <span data-ttu-id="bca62-190">請參閱下方的範例。</span><span class="sxs-lookup"><span data-stu-id="bca62-190">See the example below.</span></span>

<span data-ttu-id="bca62-191">若要列出指定叢集的所有持續性指令碼動作：</span><span class="sxs-lookup"><span data-stu-id="bca62-191">To list all persisted script actions for the specified cluster:</span></span>
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="bca62-192">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-192">Example</span></span>

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="bca62-193">列出所有指令碼的執行歷程記錄</span><span class="sxs-lookup"><span data-stu-id="bca62-193">List All Scripts' Execution History</span></span>

<span data-ttu-id="bca62-194">若要針對指定的叢集列出所有指令碼的執行歷程記錄：</span><span class="sxs-lookup"><span data-stu-id="bca62-194">To list all scripts' execution history for the specified cluster:</span></span>

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="bca62-195">範例</span><span class="sxs-lookup"><span data-stu-id="bca62-195">Example</span></span>

<span data-ttu-id="bca62-196">此範例會列印過去所有指令碼執行的所有詳細資料。</span><span class="sxs-lookup"><span data-stu-id="bca62-196">This example prints all the details for all past script executions.</span></span>

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
