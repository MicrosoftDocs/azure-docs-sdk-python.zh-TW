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
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
ms.locfileid: "29565817"
---
# <a name="installation"></a><span data-ttu-id="a1611-104">安裝</span><span class="sxs-lookup"><span data-stu-id="a1611-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="a1611-105">該使用哪個 Python 和哪個版本</span><span class="sxs-lookup"><span data-stu-id="a1611-105">Which Python and which version to use</span></span>
<span data-ttu-id="a1611-106">可用的 Python 解譯器有數種，範例包括：</span><span class="sxs-lookup"><span data-stu-id="a1611-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="a1611-107">CPython - 標準和最常見的 Python 解譯器</span><span class="sxs-lookup"><span data-stu-id="a1611-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="a1611-108">PyPy - CPython 的快速、相容替代實作</span><span class="sxs-lookup"><span data-stu-id="a1611-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="a1611-109">IronPython - 可在 .Net/CLR 上執行的 Python 解譯器</span><span class="sxs-lookup"><span data-stu-id="a1611-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="a1611-110">Jython - 可在「Java 虛擬機器」上執行的 Python 解譯器</span><span class="sxs-lookup"><span data-stu-id="a1611-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="a1611-111">**CPython** v2.7 或 v3.4+ 及 PyPy 5.4.0 已針對 Python Azure SDK 進行過測試並確定支援。</span><span class="sxs-lookup"><span data-stu-id="a1611-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="a1611-112">可在哪裡取得 Python？</span><span class="sxs-lookup"><span data-stu-id="a1611-112">Where to get Python?</span></span>
<span data-ttu-id="a1611-113">取得 CPython 的方法有數種：</span><span class="sxs-lookup"><span data-stu-id="a1611-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="a1611-114">直接從 [Python](https://www.python.org/) 取得</span><span class="sxs-lookup"><span data-stu-id="a1611-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="a1611-115">從信譽良好的散發版本取得，例如 [Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/) 或 [ActiveState](https://www.activestate.com/)</span><span class="sxs-lookup"><span data-stu-id="a1611-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="a1611-116">從原始碼組建！</span><span class="sxs-lookup"><span data-stu-id="a1611-116">Build from source!</span></span>

<span data-ttu-id="a1611-117">除非您有特定的需求，否則我們建議您採用前兩個選項。</span><span class="sxs-lookup"><span data-stu-id="a1611-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="a1611-118">使用 pip 進行安裝</span><span class="sxs-lookup"><span data-stu-id="a1611-118">Installation with pip</span></span>

<span data-ttu-id="a1611-119">您可以個別安裝每個 Azure 服務的程式庫：</span><span class="sxs-lookup"><span data-stu-id="a1611-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="a1611-120">可以使用 `--pre` 旗標來安裝預覽版套件︰</span><span class="sxs-lookup"><span data-stu-id="a1611-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="a1611-121">您也可以在單行中使用 `azure` 中繼套件來安裝一組 Azure 程式庫。</span><span class="sxs-lookup"><span data-stu-id="a1611-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="a1611-122">我們發佈這個套件的預覽版本，您可以使用 --預先旗標加以存取：</span><span class="sxs-lookup"><span data-stu-id="a1611-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="a1611-123">從 GitHub 安裝</span><span class="sxs-lookup"><span data-stu-id="a1611-123">Install from GitHub</span></span>

<span data-ttu-id="a1611-124">如果您需要從來源安裝 `azure`：</span><span class="sxs-lookup"><span data-stu-id="a1611-124">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install