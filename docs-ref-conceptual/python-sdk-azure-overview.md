---
title: 適用於 Python 的 Azure 程式庫
description: 適用於 Python 的 Azure 管理和服務程式庫概觀
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: ''
ms.openlocfilehash: 2b3e6d31edd7b946664853b3478e22205ab8c92e
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478801"
---
# <a name="azure-libraries-for-python"></a><span data-ttu-id="53e53-104">適用於 Python 的 Azure 程式庫</span><span class="sxs-lookup"><span data-stu-id="53e53-104">Azure libraries for Python</span></span>

<span data-ttu-id="53e53-105">適用於 Python 的 Azure 程式庫可讓您使用 Azure 服務，並從應用程式程式碼管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="53e53-105">The Azure libraries for Python let you use Azure services and manage Azure resources from your application code.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="53e53-106">管理 Azure 資源</span><span class="sxs-lookup"><span data-stu-id="53e53-106">Manage Azure resources</span></span>

<span data-ttu-id="53e53-107">使用適用於 Python 的 Azure 程式庫可從 Python 應用程式建立和管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="53e53-107">Create and manage Azure resources from Python applications using the Azure libraries for Python.</span></span>

<span data-ttu-id="53e53-108">例如，若要建立 SQL Server 執行個體，您可以使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="53e53-108">For example, to create a SQL Server instance, you can use the following code:</span></span>

```python
sql_client = SqlManagementClient(
    credentials,
    subscription_id
)

server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

<span data-ttu-id="53e53-109">請檢閱[安裝指示](python-sdk-azure-install.md)以取得完整的程式庫清單，以及了解如何將程式庫匯入到專案中，然後閱讀[開始使用文章](python-sdk-azure-get-started.yml)，以針對您自己的 Azure 訂用帳戶設定驗證和執行程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="53e53-109">Review the [install instructions](python-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](python-sdk-azure-get-started.yml) to set up your authentication and run sample code against your own Azure subscription.</span></span>

## <a name="connect-to-azure-services"></a><span data-ttu-id="53e53-110">連線到 Azure 服務</span><span class="sxs-lookup"><span data-stu-id="53e53-110">Connect to Azure services</span></span>

<span data-ttu-id="53e53-111">除了使用 Python 程式庫在 Azure 中建立和管理資源外，您也可以使用 Python 程式庫連線到應用程式中的資源並加以使用。</span><span class="sxs-lookup"><span data-stu-id="53e53-111">In addition to using Python libraries to create and manage resources within Azure, you can also use Python libraries to connect and use those resources in your apps.</span></span> <span data-ttu-id="53e53-112">例如，您可以更新資料表 SQL Database，或在 Azure 儲存體中儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="53e53-112">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="53e53-113">從完整程式庫清單選取特定服務所需的程式庫，然後瀏覽 Python 開發人員中心，以取得可協助您在應用程式中使用程式庫的教學課程和程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="53e53-113">Select the library you need for a particular service from the complete list of libraries and visit the Python developer center for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="53e53-114">例如，若要在 blob 上將簡單的 HTML 頁面上傳並取得 URL：</span><span class="sxs-lookup"><span data-stu-id="53e53-114">For example, to upload a simple HTML page on a blob and get the Url:</span></span>

```python
storage_client = CloudStorageAccount(storage_account_name, storage_key)
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(
    'mycontainername',
    public_access=PublicAccess.Blob
)

blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url('mycontainername', 'myblobname'))
```

## <a name="sample-code-and-reference"></a><span data-ttu-id="53e53-115">程式碼範例和參考</span><span class="sxs-lookup"><span data-stu-id="53e53-115">Sample code and reference</span></span>
<span data-ttu-id="53e53-116">下列範例涵蓋的常見自動化工作，可使用適用於 Python 的 Azure 管理程式庫來執行，而且這些範例中已備有程式碼，可供您自己的應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="53e53-116">The following samples cover common automation tasks with the Azure management libraries for Python and have code ready to use in your own apps:</span></span>
- [<span data-ttu-id="53e53-117">虛擬機器</span><span class="sxs-lookup"><span data-stu-id="53e53-117">Virtual Machines</span></span>](python-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="53e53-118">Web Apps</span><span class="sxs-lookup"><span data-stu-id="53e53-118">Web apps</span></span>](python-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="53e53-119">SQL Database</span><span class="sxs-lookup"><span data-stu-id="53e53-119">SQL Database</span></span>](python-sdk-azure-sql-database-samples.md)

<span data-ttu-id="53e53-120">我們提供了一個適用於服務和管理程式庫中所有套件的[參考](/python/api/overview/azure)。</span><span class="sxs-lookup"><span data-stu-id="53e53-120">A [reference](/python/api/overview/azure) is available for all packages in both the service an management libraries.</span></span> <span data-ttu-id="53e53-121">新功能、重大變更以及從先前版本移轉的指示則會在[版本資訊](python-sdk-azure-release-notes.md)中提供。</span><span class="sxs-lookup"><span data-stu-id="53e53-121">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](python-sdk-azure-release-notes.md).</span></span> 

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="53e53-122">獲得協助及提供意見</span><span class="sxs-lookup"><span data-stu-id="53e53-122">Get help and give feedback</span></span>

<span data-ttu-id="53e53-123">將對於社群的問題張貼於 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python)，並將對於 SDK 的開放式問題張貼於[專案 GitHub](https://github.com/Azure/azure-sdk-for-python)。</span><span class="sxs-lookup"><span data-stu-id="53e53-123">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) and open issues against the SDK on the [project GitHub](https://github.com/Azure/azure-sdk-for-python).</span></span>
