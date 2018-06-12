---
title: 適用於 Python 的 Azure 容器執行個體程式庫
description: 適用於 Python 的 Azure 容器執行個體程式庫參考
keywords: Azure、python、SDK、API、ACI、容器、執行個體
author: mmacy
manager: jeconnoc
ms.date: 06/04/2018
ms.author: marsma
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: container-instances
ms.openlocfilehash: 09f39375e0e92b6d09a965c3972d772a1437d0d4
ms.sourcegitcommit: 8c70bfd95309c3a77a4c0f73373c1785d59cdd10
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2018
ms.locfileid: "34761320"
---
# <a name="azure-container-instances-libraries-for-python"></a>適用於 Python 的 Azure 容器執行個體程式庫

使用適用於 Python 的 Microsoft Azure 容器執行個體程式庫，來建立和管理 Azure 容器執行個體。 如需詳細資訊，請參閱 [Azure 容器執行個體概觀](/azure/container-instances/container-instances-overview)。

## <a name="management-apis"></a>管理 API

使用管理程式庫來建立和管理 Azure 中的 Azure 容器執行個體。

透過 pip 安裝管理套件：

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a>範例來源

如果您想要查看下列程式碼範例在相關情境中的應用，您可以在下列 GitHub 存放庫中找到這些範例：

[Azure-Samples/aci-docs-sample-python](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a>驗證

驗證 SDK 用戶端 (如下列範例中的 Azure 容器執行個體與資源管理員用戶端) 最簡單的方式之一是使用[檔案式驗證](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file)。 檔案式驗證會查詢 `AZURE_AUTH_LOCATION` 環境變數，以取得認證檔案的路徑。 若要使用檔案式驗證：

1. 使用 [Azure CLI](/cli/azure) 或 [Cloud Shell](https://shell.azure.com/) 建立認證檔案：

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   如果您使用 [Cloud Shell](https://shell.azure.com/) 產生認證檔案，請將其內容複製到 Python 應用程式可以存取的本機檔案中。

2. 將 `AZURE_AUTH_LOCATION` 環境變數設定為所產生認證檔案的完整路徑。 例如 (在 Bash 中)：

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

一旦您建立了認證檔案，並填入 `AZURE_AUTH_LOCATION` 環境變數後，請使用 [client_factory][client_factory] 模組的 `get_client_from_auth_file` 方法來初始化 [ResourceManagementClient][ResourceManagementClient] 和 [ContainerInstanceManagementClient][ContainerInstanceManagementClient] 物件。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]

若要深入了解 Azure 的 Python 管理程式庫中有哪些可用驗證方法，請參閱[使用適用於 Python 的 Azure 管理程式庫來進行驗證](/python/azure/python-sdk-azure-authenticate)。

## <a name="create-container-group---single-container"></a>建立容器群組 - 單一容器

此範例會建立一個容器群組搭配單一容器

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L140 "Create single-container group")]

## <a name="create-container-group---multiple-containers"></a>建立容器群組 - 多個容器

此範例會建立一個容器群組搭配兩個容器：分別是應用程式容器和側車容器。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L143-L196 "Create multi-container group")]

## <a name="create-task-based-container-group"></a>立工作型容器群組

此範例會建立一個容器群組搭配單一工作型容器。 此範例會示範多個功能：

* [命令列覆寫](/azure/container-instances/container-instances-restart-policy#command-line-override) - 指定自訂命令列，但這不同於在容器 Dockerfile `CMD` 列中指定的內容。 命令列覆寫可讓您指定容器啟動時要執行的自訂命令列，並覆寫容器中內建的預設命令列。 關於要在容器啟動時執行多個命令，適用下列方式：

   如果您想要使用數個命令列引數執行**單一命令**，例如 `echo FOO BAR`，則必須將其以字串清單方式提供給[容器][Container]的 `command` 屬性。 例如︰

   `command = ['echo', 'FOO', 'BAR']`

   然而，如果您想要(或可能會) 使用多個引數來執行**多個命令**，則必須執行殼層，並將鏈結的命令做為引數傳遞。 例如，這樣會執行 `echo` 和 `tail` 命令：

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* [環境變數](/azure/container-instances/container-instances-environment-variables) - 針對容器群組中的容器指定兩個環境變數。 使用環境變數來修改執行階段的指令碼或應用程式行為，或是將動態資訊傳遞至容器中執行的應用程式。
* [重新啟動原則](/azure/container-instances/container-instances-restart-policy) - 使用 "Never" 的重新啟動原則設定容器，適用於作為批次工作一部份執行的工作型容器。
* 使用 [AzureOperationPoller][AzureOperationPoller] 進行作業輪詢 - 叫用建立方法之後會對作業進行輪詢，以判斷作業何時完成，並可取得容器群組的記錄。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L199-L275 "Run a task-based container")]

## <a name="list-container-groups"></a>列出容器群組

此範例會列出資源群組中的容器群組，然後列印其中的部分屬性。

當您列出容器群組時，每個所傳回群組的 [instance_view][instance_view] 都是 `None`。 若要取得容器群組內的容器詳細資料，您必須再 [get (取得)][containergroupoperations_get] 容器群組，這會傳回其中已填入 `instance_view` 屬性的群組。 請參閱下一節＜[取得現有的容器群組](#get-an-existing-container-group)＞，透過範例了解如何在容器群組的 `instance_view` 中逐一查看其中容器。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L278-L292 "List container groups")]

## <a name="get-an-existing-container-group"></a>取得現有的容器群組

此範例會從資源群組中取得特定容器群組，且會列印出這些容器群組的屬性 (包括其容器) 和值。

[取得作業][containergroupoperations_get]會傳回已填入 [instance_view][instance_view] 的容器群組，這可讓您逐一查看群組中的每個容器。 只有 `get` 作業會填入容器群組的 `instance_vew` 屬性 - 列出訂用帳戶或資源群組中的容器群組並不會填入執行個體檢視，因為此作業的本質可能相當耗費資源 (例如列出數百個容器群組時，每個容器群組可能包含多個容器)。 如同之前在＜[列出容器群組](#list-container-groups)＞一節中所述，執行 `list` 之後，您必須接著 `get` 特定容器群組，才能取得其中容器執行個體的詳細資料。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L295-L324 "Get container group")]

## <a name="delete-a-container-group"></a>刪除容器群組

此範例會從資源群組中刪除數個容器群組和資源群組本身。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]

## <a name="next-steps"></a>後續步驟

* 您可以在 GitHub 中找到先前範例的原始程式碼：

  [Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]

* 更多 Azure 容器執行個體程式碼範例：

  [Azure 程式碼範例][samples-aci]

* 深入探索可在應用程式中使用的 [Python 程式碼範例][samples-python]。

> [!div class="nextstepaction"]
> [探索管理 API](/python/api/overview/azure/containerinstance/management)

<!-- LINKS - External -->
[aci-docs-sample-python]: https://github.com/Azure-Samples/aci-docs-sample-python
[samples-aci]: https://azure.microsoft.com/resources/samples/?sort=0&term=ACI
[samples-python]: https://azure.microsoft.com/resources/samples/?platform=python

<!-- TYPES -->
[AzureOperationPoller]: /python/api/msrestazure.azure_operation.AzureOperationPoller
[client_factory]: /python/api/azure.common.client_factory
[Container]: /python/api/azure.mgmt.containerinstance.models.container
[ContainerGroupInstanceView]: /python/api/azure.mgmt.containerinstance.models.containergrouppropertiesinstanceview
[containergroupoperations_get]: /python/api/azure.mgmt.containerinstance.operations.containergroupsoperations#get
[ContainerInstanceManagementClient]: /python/api/azure.mgmt.containerinstance.containerinstancemanagementclient
[instance_view]: /python/api/azure.mgmt.containerinstance.models.containergroup#variables
[ResourceManagementClient]: /python/api/azure.mgmt.resource.resources.resourcemanagementclient