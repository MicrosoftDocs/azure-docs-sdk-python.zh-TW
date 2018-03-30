---
title: 適用於 Python 的 Azure SDK 作業組態
description: 適用於 Python 的 Azure SDK 擲回的 C
keywords: Azure, Python, SDK, API, 例外狀況
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 03/07/2018
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: d7be7cd76d019c6741d93c04458376a9352e363b
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="operation-config"></a>作業組態 

作業的方法有可在 kwargs 中提供的額外參數。 這稱為 operation_config。

作業組態的選項如下：

|參數名稱|類型|角色|
|----------------------|------|---------------|
| 驗證 |`bool`|是否要驗證 SSL 憑證。 預設值是 True。|
|  cert |`str`| 進行用戶端驗證的本機憑證路徑。|
|  timeout |`int`| 建立伺服器連線逾時 (以秒為單位)。|
|  allow_redirects |`bool` | 是否允許重新導向。|
|  max_redirects  |`int`| 允許的重新導向數目上限。|
|  Proxy  |`dict` |Proxy 伺服器設定。|
|  use_env_proxies |`bool` |是否從本機環境變數中讀取 Proxy 設定。|
|  重試  |`int` | 嘗試重試的次數。|
|  enable_http_logger | `bool`| 啟用偵錯模式中的的 HTTP 記錄 (預設為 False)。|
