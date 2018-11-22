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
ms.openlocfilehash: 95571e0da6ef82ef045d8c9ba0a5beb0abe9b63a
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "52273014"
---
# <a name="azure-container-instances-libraries-for-python"></a><span data-ttu-id="5d9b5-104">適用於 Python 的 Azure 容器執行個體程式庫</span><span class="sxs-lookup"><span data-stu-id="5d9b5-104">Azure Container Instances libraries for Python</span></span>

<span data-ttu-id="5d9b5-105">使用適用於 Python 的 Microsoft Azure 容器執行個體程式庫，來建立和管理 Azure 容器執行個體。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-105">Use the Microsoft Azure Container Instances libraries for Python to create and manage Azure container instances.</span></span> <span data-ttu-id="5d9b5-106">如需詳細資訊，請參閱 [Azure 容器執行個體概觀](/azure/container-instances/container-instances-overview)。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-106">Learn more by reading the [Azure Container Instances overview](/azure/container-instances/container-instances-overview).</span></span>

## <a name="management-apis"></a><span data-ttu-id="5d9b5-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="5d9b5-107">Management APIs</span></span>

<span data-ttu-id="5d9b5-108">使用管理程式庫來建立和管理 Azure 中的 Azure 容器執行個體。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-108">Use the management library to create and manage Azure container instances in Azure.</span></span>

<span data-ttu-id="5d9b5-109">透過 pip 安裝管理套件：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-109">Install the management package via pip:</span></span>

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a><span data-ttu-id="5d9b5-110">範例來源</span><span class="sxs-lookup"><span data-stu-id="5d9b5-110">Example source</span></span>

<span data-ttu-id="5d9b5-111">如果您想要查看下列程式碼範例在相關情境中的應用，您可以在下列 GitHub 存放庫中找到這些範例：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-111">If you'd like to see the following code examples in context, you can find them in the following GitHub repository:</span></span>

[<span data-ttu-id="5d9b5-112">Azure-Samples/aci-docs-sample-python</span><span class="sxs-lookup"><span data-stu-id="5d9b5-112">Azure-Samples/aci-docs-sample-python</span></span>](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a><span data-ttu-id="5d9b5-113">驗證</span><span class="sxs-lookup"><span data-stu-id="5d9b5-113">Authentication</span></span>

<span data-ttu-id="5d9b5-114">驗證 SDK 用戶端 (如下列範例中的 Azure 容器執行個體與資源管理員用戶端) 最簡單的方式之一是使用[檔案式驗證](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file)。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-114">One of the easiest ways to authenticate SDK clients (like the Azure Container Instances and Resource Manager clients in the following example) is with [file-based authentication](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file).</span></span> <span data-ttu-id="5d9b5-115">檔案式驗證會查詢 `AZURE_AUTH_LOCATION` 環境變數，以取得認證檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-115">File-based authentication queries the `AZURE_AUTH_LOCATION` environment variable for the path to a credentials file.</span></span> <span data-ttu-id="5d9b5-116">若要使用檔案式驗證：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-116">To use file-based authentication:</span></span>

1. <span data-ttu-id="5d9b5-117">使用 [Azure CLI](/cli/azure) 或 [Cloud Shell](https://shell.azure.com/) 建立認證檔案：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-117">Create a credentials file with the [Azure CLI](/cli/azure) or [Cloud Shell](https://shell.azure.com/):</span></span>

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   <span data-ttu-id="5d9b5-118">如果您使用 [Cloud Shell](https://shell.azure.com/) 產生認證檔案，請將其內容複製到 Python 應用程式可以存取的本機檔案中。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-118">If you use the [Cloud Shell](https://shell.azure.com/) to generate the credentials file, copy its contents into a local file that your Python application can access.</span></span>

2. <span data-ttu-id="5d9b5-119">將 `AZURE_AUTH_LOCATION` 環境變數設定為所產生認證檔案的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-119">Set the `AZURE_AUTH_LOCATION` environment variable to the full path of the generated credentials file.</span></span> <span data-ttu-id="5d9b5-120">例如 (在 Bash 中)：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-120">For example (in Bash):</span></span>

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

<span data-ttu-id="5d9b5-121">一旦您建立了認證檔案，並填入 `AZURE_AUTH_LOCATION` 環境變數後，請使用 [client_factory][client_factory] 模組的 `get_client_from_auth_file` 方法來初始化 [ResourceManagementClient][ResourceManagementClient] 和 [ContainerInstanceManagementClient][ContainerInstanceManagementClient] 物件。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-121">Once you've created the credentials file and populated the `AZURE_AUTH_LOCATION` environment variable, use the `get_client_from_auth_file` method of the [client_factory][client_factory] module to initialize the [ResourceManagementClient][ResourceManagementClient] and [ContainerInstanceManagementClient][ContainerInstanceManagementClient] objects.</span></span>

<span data-ttu-id="5d9b5-122"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-122"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]</span></span>

<span data-ttu-id="5d9b5-123">若要深入了解 Azure 的 Python 管理程式庫中有哪些可用驗證方法，請參閱[使用適用於 Python 的 Azure 管理程式庫來進行驗證](/python/azure/python-sdk-azure-authenticate)。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-123">For more details about the available authentication methods in the Python management libraries for Azure, see [Authenticate with the Azure Management Libraries for Python](/python/azure/python-sdk-azure-authenticate).</span></span>

## <a name="create-container-group---single-container"></a><span data-ttu-id="5d9b5-124">建立容器群組 - 單一容器</span><span class="sxs-lookup"><span data-stu-id="5d9b5-124">Create container group - single container</span></span>

<span data-ttu-id="5d9b5-125">此範例會建立一個容器群組搭配單一容器</span><span class="sxs-lookup"><span data-stu-id="5d9b5-125">This example creates a container group with a single container</span></span>

<span data-ttu-id="5d9b5-126"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L140 "Create single-container group")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-126"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L140 "Create single-container group")]</span></span>

## <a name="create-container-group---multiple-containers"></a><span data-ttu-id="5d9b5-127">建立容器群組 - 多個容器</span><span class="sxs-lookup"><span data-stu-id="5d9b5-127">Create container group - multiple containers</span></span>

<span data-ttu-id="5d9b5-128">此範例會建立一個容器群組搭配兩個容器：分別是應用程式容器和側車容器。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-128">This example creates a container group with two containers: an application container and a sidecar container.</span></span>

<span data-ttu-id="5d9b5-129"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L143-L196 "Create multi-container group")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-129"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L143-L196 "Create multi-container group")]</span></span>

## <a name="create-task-based-container-group"></a><span data-ttu-id="5d9b5-130">立工作型容器群組</span><span class="sxs-lookup"><span data-stu-id="5d9b5-130">Create task-based container group</span></span>

<span data-ttu-id="5d9b5-131">此範例會建立一個容器群組搭配單一工作型容器。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-131">This example creates a container group with a single task-based container.</span></span> <span data-ttu-id="5d9b5-132">此範例會示範多個功能：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-132">This example demonstrates several features:</span></span>

* <span data-ttu-id="5d9b5-133">[命令列覆寫](/azure/container-instances/container-instances-restart-policy#command-line-override) - 指定自訂命令列，但這不同於在容器 Dockerfile `CMD` 列中指定的內容。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-133">[Command line override](/azure/container-instances/container-instances-restart-policy#command-line-override) - A custom command line, different from that which is specified in the container's Dockerfile `CMD` line, is specified.</span></span> <span data-ttu-id="5d9b5-134">命令列覆寫可讓您指定容器啟動時要執行的自訂命令列，並覆寫容器中內建的預設命令列。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-134">Command line override allows you to specify a custom command line to execute at container startup, overriding the default command line baked-in to the container.</span></span> <span data-ttu-id="5d9b5-135">關於要在容器啟動時執行多個命令，適用下列方式：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-135">Regarding executing multiple commands at container startup, the following applies:</span></span>

   <span data-ttu-id="5d9b5-136">如果您想要使用數個命令列引數執行**單一命令**，例如 `echo FOO BAR`，則必須將其以字串清單方式提供給[容器][Container]的 `command` 屬性。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-136">If you want to run a **single command** with several command-line arguments, for example `echo FOO BAR`, you must supply them as a string list to the `command` property of the [Container][Container].</span></span> <span data-ttu-id="5d9b5-137">例如︰</span><span class="sxs-lookup"><span data-stu-id="5d9b5-137">For example:</span></span>

   `command = ['echo', 'FOO', 'BAR']`

   <span data-ttu-id="5d9b5-138">然而，如果您想要(或可能會) 使用多個引數來執行**多個命令**，則必須執行殼層，並將鏈結的命令做為引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-138">If, however, you want to run **multiple commands** with (potentially) multiple arguments, you must execute a shell and pass the chained commands as an argument.</span></span> <span data-ttu-id="5d9b5-139">例如，這樣會執行 `echo` 和 `tail` 命令：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-139">For example, this executes both an `echo` and a `tail` command:</span></span>

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* <span data-ttu-id="5d9b5-140">[環境變數](/azure/container-instances/container-instances-environment-variables) - 針對容器群組中的容器指定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-140">[Environment variables](/azure/container-instances/container-instances-environment-variables) - Two environment variables are specified for the container in the container group.</span></span> <span data-ttu-id="5d9b5-141">使用環境變數來修改執行階段的指令碼或應用程式行為，或是將動態資訊傳遞至容器中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-141">Use environment variables to modify script or application behavior at runtime, or otherwise pass dynamic information to an application running in the container.</span></span>
* <span data-ttu-id="5d9b5-142">[重新啟動原則](/azure/container-instances/container-instances-restart-policy) - 使用 "Never" 的重新啟動原則設定容器，適用於作為批次工作一部份執行的工作型容器。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-142">[Restart policy](/azure/container-instances/container-instances-restart-policy) - The container is configured with a restart policy of "Never," useful for task-based containers that are executed as part of a batch job.</span></span>
* <span data-ttu-id="5d9b5-143">使用 [AzureOperationPoller][AzureOperationPoller] 進行作業輪詢 - 叫用建立方法之後會對作業進行輪詢，以判斷作業何時完成，並可取得容器群組的記錄。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-143">Operation polling with [AzureOperationPoller][AzureOperationPoller] - After the create method is invoked, the operation is polled to determine when it has completed and the container group's logs can be obtained.</span></span>

<span data-ttu-id="5d9b5-144"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L199-L275 "Run a task-based container")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-144"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L199-L275 "Run a task-based container")]</span></span>

## <a name="list-container-groups"></a><span data-ttu-id="5d9b5-145">列出容器群組</span><span class="sxs-lookup"><span data-stu-id="5d9b5-145">List container groups</span></span>

<span data-ttu-id="5d9b5-146">此範例會列出資源群組中的容器群組，然後列印其中的部分屬性。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-146">This example lists the container groups in a resource group and then prints a few of their properties.</span></span>

<span data-ttu-id="5d9b5-147">當您列出容器群組時，每個所傳回群組的 [instance_view][instance_view] 都是 `None`。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-147">When you list container groups,the [instance_view][instance_view] of each returned group is `None`.</span></span> <span data-ttu-id="5d9b5-148">若要取得容器群組內的容器詳細資料，您必須再 [get (取得)][containergroupoperations_get] 容器群組，這會傳回其中已填入 `instance_view` 屬性的群組。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-148">To get the details of the containers within a container group, you must then [get][containergroupoperations_get] the container group, which returns the group with its `instance_view` property populated.</span></span> <span data-ttu-id="5d9b5-149">請參閱下一節＜[取得現有的容器群組](#get-an-existing-container-group)＞，透過範例了解如何在容器群組的 `instance_view` 中逐一查看其中容器。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-149">See the next section, [Get an existing container group](#get-an-existing-container-group), for an example of iterating over a container group's containers in its `instance_view`.</span></span>

<span data-ttu-id="5d9b5-150"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L278-L292 "List container groups")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-150"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L278-L292 "List container groups")]</span></span>

## <a name="get-an-existing-container-group"></a><span data-ttu-id="5d9b5-151">取得現有的容器群組</span><span class="sxs-lookup"><span data-stu-id="5d9b5-151">Get an existing container group</span></span>

<span data-ttu-id="5d9b5-152">此範例會從資源群組中取得特定容器群組，且會列印出這些容器群組的屬性 (包括其容器) 和值。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-152">This example gets a specific container group from a resource group, and then prints a few of its properties (including its containers) and their values.</span></span>

<span data-ttu-id="5d9b5-153">[取得作業][containergroupoperations_get]會傳回已填入 [instance_view][instance_view] 的容器群組，這可讓您逐一查看群組中的每個容器。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-153">The [get operation][containergroupoperations_get] returns a container group with its [instance_view][instance_view] populated, which allows you to iterate over each container in the group.</span></span> <span data-ttu-id="5d9b5-154">只有 `get` 作業會填入容器群組的 `instance_vew` 屬性 - 列出訂用帳戶或資源群組中的容器群組並不會填入執行個體檢視，因為此作業的本質可能相當耗費資源 (例如列出數百個容器群組時，每個容器群組可能包含多個容器)。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-154">Only the `get` operation populates the `instance_vew` property of the container group--listing the container groups in a subscription or resource group doesn't populate the instance view due to the potentially expensive nature of the operation (for example, when listing hundreds of container groups, each potentially containing multiple containers).</span></span> <span data-ttu-id="5d9b5-155">如同之前在＜[列出容器群組](#list-container-groups)＞一節中所述，執行 `list` 之後，您必須接著 `get` 特定容器群組，才能取得其中容器執行個體的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-155">As mentioned previously in the [List container groups](#list-container-groups) section, after a `list`, you must subsequently `get` a specific container group to obtain its container instance details.</span></span>

<span data-ttu-id="5d9b5-156"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L295-L324 "Get container group")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-156"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L295-L324 "Get container group")]</span></span>

## <a name="delete-a-container-group"></a><span data-ttu-id="5d9b5-157">刪除容器群組</span><span class="sxs-lookup"><span data-stu-id="5d9b5-157">Delete a container group</span></span>

<span data-ttu-id="5d9b5-158">此範例會從資源群組中刪除數個容器群組和資源群組本身。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-158">This example deletes several container groups from a resource group, as well as the resource group.</span></span>

<span data-ttu-id="5d9b5-159"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-159"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d9b5-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5d9b5-160">Next steps</span></span>

* <span data-ttu-id="5d9b5-161">您可以在 GitHub 中找到先前範例的原始程式碼：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-161">The source code for the preceding examples can be found on GitHub:</span></span>

  <span data-ttu-id="5d9b5-162">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-162">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span></span>

* <span data-ttu-id="5d9b5-163">更多 Azure 容器執行個體程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="5d9b5-163">More Azure Container Instances code samples:</span></span>

  <span data-ttu-id="5d9b5-164">[Azure 程式碼範例][samples-aci]</span><span class="sxs-lookup"><span data-stu-id="5d9b5-164">[Azure Code Samples][samples-aci]</span></span>

* <span data-ttu-id="5d9b5-165">深入探索可在應用程式中使用的 [Python 程式碼範例][samples-python]。</span><span class="sxs-lookup"><span data-stu-id="5d9b5-165">Explore more [sample Python code][samples-python] you can use in your apps.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d9b5-166">探索管理 API</span><span class="sxs-lookup"><span data-stu-id="5d9b5-166">Explore the management APIs</span></span>](/python/api/overview/azure/containerinstance/management)

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