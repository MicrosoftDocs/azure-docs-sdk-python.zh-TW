---
title: "安裝"
description: "如何安裝 Azure Python SDK"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
---
# <a name="installation"></a>安裝

## <a name="which-python-and-which-version-to-use"></a>該使用哪個 Python 和哪個版本
可用的 Python 解譯器有數種，範例包括：

* CPython - 標準和最常見的 Python 解譯器
* PyPy - CPython 的快速、相容替代實作
* IronPython - 可在 .Net/CLR 上執行的 Python 解譯器
* Jython - 可在「Java 虛擬機器」上執行的 Python 解譯器

**CPython** v2.7 或 v3.4+ 及 PyPy 5.4.0 已針對 Python Azure SDK 進行過測試並確定支援。

## <a name="where-to-get-python"></a>可在哪裡取得 Python？
取得 CPython 的方法有數種：

* 直接從 [Python](https://www.python.org/) 取得
* 從信譽良好的散發版本取得，例如 [Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/) 或 [ActiveState](https://www.activestate.com/)
* 從原始碼組建！

除非您有特定的需求，否則我們建議您採用前兩個選項。

## <a name="installation-with-pip"></a>使用 pip 進行安裝

您可以個別安裝每個 Azure 服務的程式庫：

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

可以使用 `--pre` 旗標來安裝預覽版套件︰

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

您也可以在單行中使用 `azure` 中繼套件來安裝一組 Azure 程式庫。

```bash
pip install azure
```

我們發佈這個套件的預覽版本，您可以使用 --預先旗標加以存取：

```bash
pip install --pre azure
```

## <a name="install-from-github"></a>從 GitHub 安裝

如果您需要從來源安裝 `azure`：

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install