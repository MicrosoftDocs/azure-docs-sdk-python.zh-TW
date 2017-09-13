---
title: "開始使用適用於 Python 的 Azure 程式庫"
description: "開始使用適用於 Python 的 Azure 程式庫"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: get-started
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 848ca9dc40000e68e5e3cea3af8b8a0d22747881
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-azure-libraries-for-python"></a>開始使用適用於 Python 的 Azure 程式庫

本指南會示範適用於 Python 的幾個 Azure 程式庫使用方式。  您將設定驗證、建立和使用 Azure 儲存體帳戶、建立和使用 Azure SQL Database、部署部分的虛擬機器，以及從 GitHub 部署 Azure App Service Web 應用程式。

## <a name="prerequisites"></a>必要條件

- 一個 Azure 帳戶。 如果您沒有帳戶，請[取得免費試用帳戶](https://azure.microsoft.com/free/)。
- [Python](https://www.python.org/downloads/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。

[!INCLUDE [azure-cloud-shell](../docs-ref-conceptual/includes/cloud-shell-try-it.md)]

## <a name="set-up-authentication"></a>設定驗證
> [!IMPORTANT]
> 這應該用來作為快速入門開發人員的體驗。 若要用於生產環境，請使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 或您自己的認證系統。
> 對 CLI 設定的任何變更都會影響 SDK 執行。

SDK 能夠使用 CLI 使用中訂用帳戶來建立用戶端。

若要定義使用中的認證，請使用 [az 登入](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。
預設訂用帳戶識別碼是您所擁有的唯一一個，或是可以使用 [az 帳戶](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)加以定義。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
## <a name="create-a-virtual-environment"></a>建立虛擬環境

> [!IMPORTANT]
> 建立虛擬環境是選擇性的，但強烈建議您這麼做。

在 Bash 中建立虛擬環境 (Linux 或[適用於 Windows 的 Bash](https://msdn.microsoft.com/commandline/wsl/about))：
```bash
pip install virtualenv
virtualenv mytestenv
cd mytestenv
source ./bin/activate
```

在 Powershell 中建立虛擬環境：
```powershell
pip install virtualenv
virtualenv mytestenv
cd mytestenv
.\Scripts\activate
```

> [!IMPORTANT]
> 請注意，如果您是使用 [VSCode](https://code.visualstudio.com/) 和 [Python 擴充功能](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python)，可以使用 `code .` 開始進行，並取得設定您的環境。

## <a name="create-a-linux-virtual-machine"></a>建立 Linux 虛擬機器
此程式碼會在美國東部 Azure 區域執行的 `sampleVmResourceGroup` 資源群組中，建立名為 `testLinuxVM` 的新 Linux VM。

驗證：
```azcli
az login
```
建立資源群組：
```azurecli-interactive
az group create -l eastus --n sampleVmResourceGroup
```

建立虛擬網路和子網路：
```azurecli-interactive
az network vnet create -g sampleVmResourceGroup -n azure-sample-vnet --address-prefix 10.0.0.0/16 --subnet-name azure-sample-subnet --subnet-prefix 10.0.0.0/24
```

建立公用 IP 位址：
```azurecli-interactive
az network public-ip create -g sampleVmResourceGroup -n azure-sample-ip --allocation-method Dynamic --version IPv6
```
建立網路介面用戶端：
```azurecli-interactive
az network nic create -g sampleVmResourceGroup --vnet-name azure-sample-vnet --subnet azure-sample-subnet -n azure-sample-nic --public-ip-address azure-sample-ip
```

```python
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
from azure.common.client_factory import get_client_from_cli_profile

# Azure Datacenter
LOCATION = 'eastus'

# Resource Group
GROUP_NAME = 'sampleVmResourceGroup'

# Network
VNET_NAME = 'azure-sample-vnet'
SUBNET_NAME = 'azure-sample-subnet'

# VM
NIC_NAME = 'azure-sample-nic'
VM_NAME = 'testLinuxVM'

USERNAME = 'userlogin'
PASSWORD = 'Pa$$w0rd91'

IP_ADDRESS_NAME='azure-sample-ip'

VM_REFERENCE = {
    'linux': {
        'publisher': 'Canonical',
        'offer': 'UbuntuServer',
        'sku': '16.04.0-LTS',
        'version': 'latest'
    },
    'windows': {
        'publisher': 'MicrosoftWindowsServerEssentials',
        'offer': 'WindowsServerEssentials',
        'sku': 'WindowsServerEssentials',
        'version': 'latest'
    }
}


def run_example():

    compute_client = get_client_from_cli_profile(ComputeManagementClient)
    network_client = get_client_from_cli_profile(NetworkManagementClient)

    # get nic id
    nic = network_client.network_interfaces.get(GROUP_NAME, NIC_NAME)

    # Create Linux VM
    print('\nCreating Linux Virtual Machine')
    vm_parameters = create_vm_parameters(nic.id, VM_REFERENCE['linux'])
    print(vm_parameters)
    async_vm_creation = compute_client.virtual_machines.create_or_update(
        GROUP_NAME, VM_NAME, vm_parameters)
    async_vm_creation.wait()


def create_vm_parameters(nic_id, vm_reference):
    """Create the VM parameters structure.
    """
    return {
        'location': LOCATION,
        'os_profile': {
            'computer_name': VM_NAME,
            'admin_username': USERNAME,
            'admin_password': PASSWORD
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': vm_reference['publisher'],
                'offer': vm_reference['offer'],
                'sku': vm_reference['sku'],
                'version': vm_reference['version']
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': nic_id,
            }]
        },
    }


if __name__ == "__main__":
    run_example()
```

當程式完成時，請使用 Azure CLI 2.0 在訂用帳戶中確認虛擬機器：

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

在確認程式碼有效後，請使用 CLI 來刪除 VM 和其資源。

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>從 GitHub 存放庫部署 Web 應用程式
此程式碼會將 GitHub 存放庫中 `master` 分支內的 Flask Web 應用程式，部署至執行於免費層的新 [Azure App Service Web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。 

開始之前：Fork https://github.com/Azure-Samples/python-docs-hello-world

驗證：
```azcli
az login
```
建立資源群組：
```azurecli-interactive
az group create -l eastus -n sampleWebResourceGroup
```

建立免費 App Service 方案：
```azurecli-interactive
az appservice plan create -g sampleWebResourceGroup -n sampleFreePlan  --sku Free
```

```python
from azure.mgmt.web import WebSiteManagementClient
from azure.mgmt.web.models import Site, SiteSourceControl, SiteConfig
from azure.common.client_factory import get_client_from_cli_profile

RESOURCE_GROUP_NAME = 'sampleWebResourceGroup'
SERVICE_PLAN_NAME = 'sampleFreePlanName'
WEB_APP_NAME = 'sampleflaskapp123'
REPO_URL = 'GitHub_Repository_URL'

#log in
web_client = get_client_from_cli_profile(WebSiteManagementClient)

# get service plan id
service_plan = web_client.app_service_plans.get(RESOURCE_GROUP_NAME, SERVICE_PLAN_NAME)

# create a web app
siteConfiguration = SiteConfig(
    python_version='3.4',
)
site_async_operation = web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=service_plan.id,
        site_config=siteConfiguration
    ),
)

site = site_async_operation.result()
print('created webapp: ' + site.default_host_name)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url= REPO_URL,
        branch='master'
    )
)

source_control = source_control_async_operation.result()
print("added source control to: " + source_control.name + "azurewebsites.net")
```

使用 CLI 開啟瀏覽器並讓其指向該應用程式：
```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

確認過部署後，請從訂用帳戶中移除 Web 應用程式和方案。 
```azurecli-interactive
az group delete --name sampleWebResourceGroup
```


## <a name="connect-to-a-sql-database"></a>連線到 SQL Database
此程式碼會使用允許遠端存取的防火牆規則來建立新的 SQL Database，然後使用 Microsoft ODBC 驅動程式來與該資料庫連線。 Pyodbc 會使用 Linux 上的 Microsoft ODBC Driver 連線至 SQL Database。 

驗證：
```azcli
az login
```
建立資源群組：
```azurecli-interactive
az group create -l eastus -n azure-sample-group
```

建立 SQL Server：
```azurecli-interactive
az sql server create --admin-password HusH_Sec4et --admin-user mysecretname -l eastus -n samplesqlserver123 -g azure-sample-group
```

新增防火牆規則：
```azurecli-interactive
az sql server firewall-rule create --end-ip-address 167.220.0.235 --name firewall_rule_name_123.123.123.123 --resource-group azure-sample-group --server samplesqlserver123 --start-ip-address 123.123.123.123
```

建立 SQL Database：
```azurecli-interactive
az sql db create --name sample-db --resource-group azure-sample-group --server samplesqlserver123
```


```python
from azure.mgmt.sql import SqlManagementClient
from azure.common.client_factory import get_client_from_cli_profile
import pyodbc

LOCATION = 'eastus'
GROUP_NAME = 'azure-sample-group'
SERVER_NAME = 'samplesqlserver123'
DATABASE_NAME = 'sample-db'
USER_NAME ='mysecretname'
PASSWORD='HusH_Sec4et'

# authenticate
sql_client = get_client_from_cli_profile(SqlManagementClient)


def run_example():
    # Get SQL database
    print('Get SQL database')
    database = sql_client.databases.get(
        GROUP_NAME,
        SERVER_NAME,
        DATABASE_NAME
    )
    print(database)
    print("\n\n")


def create_and_insert():
    server = SERVER_NAME+'.database.windows.net'
    database = DATABASE_NAME
    username = USER_NAME
    password = PASSWORD
    driver = '{ODBC Driver 13 for SQL Server}'
    cnxn = pyodbc.connect(
        'DRIVER=' + driver + ';PORT=1433;SERVER=' + server + ';PORT=1443;DATABASE=' + database + ';UID=' + username + ';PWD=' + password)
    cursor = cnxn.cursor()
    tsql = "CREATE TABLE CLOUD (name varchar(255), code int);"
    tsqlInsert = "INSERT INTO CLOUD (name, code) VALUES ('Azure', 1); "
    selectValues = "SELECT * FROM CLOUD"
    with cursor.execute(tsql):
        print('Successfully created table!')
    with cursor.execute(tsqlInsert):
        print('Successfully inserted data into table')
    cursor.execute(selectValues)
    row = cursor.fetchone()
    while row:
        print(str(row[0]) + " " + str(row[1]))
        row = cursor.fetchone()
    cnxn.commit()

if __name__ == "__main__":
    run_example()
    create_and_insert()
```

在確認程式碼有效後，請使用 CLI 來刪除 SQL Database 和其資源。

```azurecli-interactive
az group delete --name azure-sample-group
```

## <a name="write-a-blob-into-a-new-storage-account"></a>將 blob 寫入新的儲存體帳戶中

驗證：
```azcli
az login
```
建立資源群組：
```azurecli-interactive
az group create -l eastus -n sampleStorageResourceGroup
```

建立儲存體帳戶：
```azurecli-interactive
az storage account create -n samplestorageaccountname -g sampleStorageResourceGroup -l eastus --sku Standard_RAGRS
```

此程式碼會建立 [Azure 儲存體帳戶](https://docs.microsoft.com/azure/storage/storage-introduction)，然後使用適用於 Python 的 Azure 儲存體程式庫在雲端建立新的 html 檔案。 
```python
from azure.storage import CloudStorageAccount
from azure.storage.blob import PublicAccess
from azure.storage.blob.models import ContentSettings
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.storage import StorageManagementClient

RESOURCE_GROUP = 'sampleStorageResourceGroup'
STORAGE_ACCOUNT_NAME = 'samplestorageaccountname'
CONTAINER_NAME = 'samplecontainername'

# log in
storage_client = get_client_from_cli_profile(StorageManagementClient)

# create a public storage container to hold the file
storage_keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP, STORAGE_ACCOUNT_NAME)
storage_keys = {v.key_name: v.value for v in storage_keys.keys}

storage_client = CloudStorageAccount(STORAGE_ACCOUNT_NAME, storage_keys['key1'])
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(CONTAINER_NAME, public_access=PublicAccess.Container)

blob_service.create_blob_from_bytes(
    CONTAINER_NAME,
    'helloworld.html',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url(CONTAINER_NAME, 'helloworld.html'))
```
若要確認內容已成功上傳，請瀏覽至列印的 URL。 

使用 CLI 清除儲存體帳戶：
```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>探索更多範例

若要深入了解如何使用適用於 Python 的 Azure 管理程式庫來管理資源和自動執行工作，請參閱我們針對[虛擬機器](python-sdk-azure-web-apps-samples.md)、[Web 應用程式](python-sdk-azure-web-apps-samples.md)和 [SQL Database](python-sdk-azure-sql-database-samples.md) 所提供的程式碼範例。


## <a name="reference"></a>參考

我們針對所有套件提供了[參考資料](/python/api/overview/azure/?view=azure-python)。
