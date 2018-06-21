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
# <a name="azure-storage-libraries-for-python"></a>適用於 Python 的 Azure 儲存體程式庫

## <a name="overview"></a>概觀
- 從 [Azure Blob 儲存體](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)讀取和寫入物件及檔案
- 使用 [Azure 佇列儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)在雲端連線的應用程式之間傳送和接收訊息
- 使用 [Azure 資料表儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)讀取及寫入大型的結構化資料 
- 使用 [Azure 檔案儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)共用應用程式之間的儲存體

使用管理程式庫來建立、更新和管理 Azure 儲存體帳戶，並從您的 Python 程式碼查詢及重新產生存取金鑰。

## <a name="install-the-libraries"></a>安裝程式庫

### <a name="client"></a>用戶端

Azure 儲存體用戶端程式庫由 4 個套件組成，分別是：Blob、檔案、訂列和資料表。 若要安裝 Blob 套件，請執行：

```bash
pip install azure-storage-blob
```

### <a name="management"></a>管理性

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>範例
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

## <a name="samples"></a>範例

| | |
|--|--|
| [在 Python 中開始使用 Azure Blob 儲存體](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | 在 Azure 儲存體中建立、讀取、更新、限制存取，並刪除檔案和物件。 |
| [在 Python 中開始使用 Azure 佇列儲存體](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | 從 Azure 儲存體佇列中插入、查看、擷取並刪除訊息。 | 
| [管理 Azure 儲存體帳戶](https://azure.microsoft.com/resources/samples/storage-python-manage) | 建立、更新和刪除儲存體帳戶。 取出和重新產生儲存體帳戶存取金鑰。

深入探索可在應用程式中使用的 [Python 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=python)。
