---
title: 安裝
description: 如何安裝 Azure Python SDK
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
ms.openlocfilehash: 9fd11cbc7b987b970ceee85c7b11b22e3d6299ea
ms.sourcegitcommit: 31d7df367b15ec09a5a610eb333295bba0f6b351
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67395443"
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

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a>使用 pip 安裝較舊版本
您可以指定 'azure==3.0.0' 版本詳細資料，以安裝較舊版本的 `azure`。
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a>使用 pip 檢查 SDK 安裝詳細資料
您可以檢查 `azure` SDK 安裝位置、版本詳細資料等。
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a>使用 pip 解除安裝
您可以在單行中使用 `azure` 中繼套件來安裝所有 Azure 程式庫。
```bash
pip uninstall azure 
```
> [!NOTE]
> `pip uninstall azure`移除 `azure` 中繼套件但保留個別的 `azure-*` 套件 (以及其他項目，像是 `adal` 和 `msrest`)。 Python 和 pip 的概念是所有套件都具備相依性，解除安裝最初的套件並不會解除安裝相依性。 要移除 `azure-` 乗其支援套件，請執行 `pip freeze | grep 'azure-' | xargs pip uninstall -y` 命令 (再分別解除安裝 adal、msrest 和 msrestazure )。

