---
title: 適用於 Python 的 Azure 儲存體程式庫
description: ''
keywords: Azure, Python, SDK, API, 儲存體
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: e45b12af9e026e0f6390556813385d86784feaa4
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2018
ms.locfileid: "35720059"
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="8327b-103">適用於 Python 的 Azure 儲存體程式庫</span><span class="sxs-lookup"><span data-stu-id="8327b-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="8327b-104">概觀</span><span class="sxs-lookup"><span data-stu-id="8327b-104">Overview</span></span>
- <span data-ttu-id="8327b-105">從 [Azure Blob 儲存體](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)讀取和寫入物件及檔案</span><span class="sxs-lookup"><span data-stu-id="8327b-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="8327b-106">使用 [Azure 佇列儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)在雲端連線的應用程式之間傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="8327b-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="8327b-107">使用 [Azure 資料表儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)讀取及寫入大型的結構化資料</span><span class="sxs-lookup"><span data-stu-id="8327b-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="8327b-108">使用 [Azure 檔案儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)共用應用程式之間的儲存體</span><span class="sxs-lookup"><span data-stu-id="8327b-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="8327b-109">使用管理程式庫來建立、更新和管理 Azure 儲存體帳戶，並從您的 Python 程式碼查詢及重新產生存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="8327b-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="8327b-110">安裝程式庫</span><span class="sxs-lookup"><span data-stu-id="8327b-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="8327b-111">用戶端</span><span class="sxs-lookup"><span data-stu-id="8327b-111">Client</span></span>

<span data-ttu-id="8327b-112">Azure 儲存體用戶端程式庫由 4 個套件組成，分別是：Blob、檔案、訂列和資料表。</span><span class="sxs-lookup"><span data-stu-id="8327b-112">Azure Storage Client Libraries consist of 4 packages: Blob, File, Queue and Table.</span></span> <span data-ttu-id="8327b-113">若要安裝 Blob 套件，請執行：</span><span class="sxs-lookup"><span data-stu-id="8327b-113">To install the blob package, run:</span></span>

```bash
pip install azure-storage-blob
```

### <a name="management"></a><span data-ttu-id="8327b-114">管理性</span><span class="sxs-lookup"><span data-stu-id="8327b-114">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="8327b-115">範例</span><span class="sxs-lookup"><span data-stu-id="8327b-115">Example</span></span>
```python
from azure.storage.blob import BlockBlobService

blob_service = BlockBlobService(account_name, account_key)

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

## <a name="samples"></a><span data-ttu-id="8327b-116">範例</span><span class="sxs-lookup"><span data-stu-id="8327b-116">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="8327b-117">在 Python 中開始使用 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="8327b-117">Get started with Azure Blob Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | <span data-ttu-id="8327b-118">在 Azure 儲存體中建立、讀取、更新、限制存取，並刪除檔案和物件。</span><span class="sxs-lookup"><span data-stu-id="8327b-118">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="8327b-119">在 Python 中開始使用 Azure 佇列儲存體</span><span class="sxs-lookup"><span data-stu-id="8327b-119">Get started with Azure Queue Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | <span data-ttu-id="8327b-120">從 Azure 儲存體佇列中插入、查看、擷取並刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="8327b-120">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="8327b-121">管理 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="8327b-121">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="8327b-122">建立、更新和刪除儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="8327b-122">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="8327b-123">取出和重新產生儲存體帳戶存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="8327b-123">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="8327b-124">深入探索可在應用程式中使用的 [Python 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=python)。</span><span class="sxs-lookup"><span data-stu-id="8327b-124">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>
