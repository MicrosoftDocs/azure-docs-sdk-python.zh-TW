---
title: "適用於 Python 的 Azure 程式庫"
description: "適用於 Python 的 Azure 管理和服務程式庫概觀"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 68074d445a21a38fe6ffb6f5f7b7cbd8f24d87a3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="azure-libraries-for-python"></a>適用於 Python 的 Azure 程式庫

適用於 Python 的 Azure 程式庫可讓您使用 Azure 服務，並從應用程式程式碼管理 Azure 資源。 [PyPI](python-sdk-azure-install.md) 中可用的程式庫可在 Python 專案中使用。

## <a name="manage-azure-resources"></a>管理 Azure 資源

使用適用於 Python 的 Azure 程式庫可從 Python 應用程式建立和管理 Azure 資源。

例如，若要建立 SQL Server 執行個體，您可以使用下列程式碼：

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

請檢閱[安裝指示](python-sdk-azure-install.md)以取得完整的程式庫清單，以及了解如何將程式庫匯入專案中，然後閱讀[開始使用文章](python-sdk-azure-get-started.md)，針對您自己的 Azure 訂用帳戶設定驗證和執行程式碼範例。

## <a name="connect-to-azure-services"></a>連線到 Azure 服務

除了使用 Python 程式庫在 Azure 中建立和管理資源外，您也可以使用 Python 程式庫連線到應用程式中的資源並加以使用。 例如，您可以更新資料表 SQL Database，或在 Azure 儲存體中儲存檔案。 從完整程式庫清單選取特定服務所需的程式庫，然後瀏覽 Python 開發人員中心，以取得可協助您在應用程式中使用程式庫的教學課程和程式碼範例。

例如，若要在 blob 上將簡單的 HTML 頁面上傳並取得 URL：

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

## <a name="sample-code-and-reference"></a>程式碼範例和參考
下列範例涵蓋的常見自動化工作，可使用適用於 Python 的 Azure 管理程式庫來執行，而且這些範例中已備有程式碼，可供您自己的應用程式使用：
- [虛擬機器](python-sdk-azure-virtual-machine-samples.md)
- [Web Apps](python-sdk-azure-web-apps-samples.md)
- [SQL Database](python-sdk-azure-sql-database-samples.md)

我們提供了一個適用於服務和管理程式庫中所有套件的[參考](/python/api/overview/azure)。 新功能、重大變更以及從先前版本移轉的指示則會在[版本資訊](python-sdk-azure-release-notes.md)中提供。 

## <a name="get-help-and-give-feedback"></a>獲得協助及提供意見

將對於社群的問題張貼於 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python)，並將對於 SDK 的開放式問題張貼於[專案 GitHub](https://github.com/Azure/azure-sdk-for-python)。
