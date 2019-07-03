---
title: 適用於 Python 的 Azure 儲存體程式庫
description: ''
keywords: Azure, Python, SDK, API, 儲存體
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 1cafbe5558c49e238182b7efabeae6fcb96d43d8
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534199"
---
# <a name="azure-storage-libraries-for-python"></a>適用於 Python 的 Azure 儲存體程式庫

## <a name="overview"></a>概觀
- 從 [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)讀取和寫入物件及檔案
- 使用 [Azure 佇列儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)在雲端連線的應用程式之間傳送和接收訊息
- 使用 [Azure 資料表儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)讀取及寫入大型的結構化資料 
- 使用 [Azure 檔案儲存體](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)共用應用程式之間的儲存體

使用管理程式庫來建立、更新和管理 Azure 儲存體帳戶，並從您的 Python 程式碼查詢及重新產生存取金鑰。

## <a name="install-the-libraries"></a>安裝程式庫

### <a name="client"></a>用戶端

Azure 儲存體用戶端程式庫由 4 個套件組成：Blob、檔案、佇列和資料表。 若要安裝 Blob 套件，請執行：

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
| [在 Python 中開始使用 Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-python-how-to-use-blob-storage) | 在 Azure 儲存體中建立、讀取、更新、限制存取，並刪除檔案和物件。 |
| [在 Python 中開始使用 Azure 佇列儲存體](https://docs.microsoft.com/azure/storage/queues/storage-python-how-to-use-queue-storage) | 從 Azure 儲存體佇列中插入、查看、擷取並刪除訊息。 | 
| [管理 Azure 儲存體帳戶](https://azure.microsoft.com/resources/samples/storage-python-manage) | 建立、更新和刪除儲存體帳戶。 取出和重新產生儲存體帳戶存取金鑰。

深入探索可在應用程式中使用的 [Python 程式碼範例](https://azure.microsoft.com/resources/samples/?platform=python)。
