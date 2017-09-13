---
title: "適用於 Python 的 Azure 儲存體程式庫"
description: 
keywords: "Azure, Python, SDK, API, 儲存體"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 64465964d32934a6a45dec44cb92a0a8a84b9c37
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
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

```bash
pip install azure-storage
```

### <a name="management"></a>管理

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>範例
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

## <a name="samples"></a>範例

| | |
|--|--|
| [在 Python 中開始使用 Azure Blob 儲存體](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | 在 Azure 儲存體中建立、讀取、更新、限制存取，並刪除檔案和物件。 |
| [在 Python 中開始使用 Azure 佇列儲存體](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | 從 Azure 儲存體佇列中插入、查看、擷取並刪除訊息。 | 
| [管理 Azure 儲存體帳戶](https://azure.microsoft.com/resources/samples/storage-python-manage) | 建立、更新和刪除儲存體帳戶。 取出和重新產生儲存體帳戶存取金鑰。

深入探索可在應用程式中使用的 [Python 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=python)。