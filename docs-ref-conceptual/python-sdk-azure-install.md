---
title: 安裝
description: 如何安裝 Azure Python SDK
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 478118642122d7c0c80b1ddf37b13f71d8ca3adc
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534452"
---
# <a name="installation"></a><span data-ttu-id="dc44e-104">安裝</span><span class="sxs-lookup"><span data-stu-id="dc44e-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="dc44e-105">該使用哪個 Python 和哪個版本</span><span class="sxs-lookup"><span data-stu-id="dc44e-105">Which Python and which version to use</span></span>

<span data-ttu-id="dc44e-106">可用的 Python 解譯器有數種，範例包括：</span><span class="sxs-lookup"><span data-stu-id="dc44e-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="dc44e-107">CPython - 標準和最常見的 Python 解譯器</span><span class="sxs-lookup"><span data-stu-id="dc44e-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="dc44e-108">PyPy - CPython 的快速、相容替代實作</span><span class="sxs-lookup"><span data-stu-id="dc44e-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="dc44e-109">IronPython - 可在 .Net/CLR 上執行的 Python 解譯器</span><span class="sxs-lookup"><span data-stu-id="dc44e-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="dc44e-110">Jython - 可在「Java 虛擬機器」上執行的 Python 解譯器</span><span class="sxs-lookup"><span data-stu-id="dc44e-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="dc44e-111">**CPython** v2.7 或 v3.4+ 及 PyPy 5.4.0 已針對 Python Azure SDK 進行過測試並確定支援。</span><span class="sxs-lookup"><span data-stu-id="dc44e-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="dc44e-112">可在哪裡取得 Python？</span><span class="sxs-lookup"><span data-stu-id="dc44e-112">Where to get Python?</span></span>

<span data-ttu-id="dc44e-113">取得 CPython 的方法有數種：</span><span class="sxs-lookup"><span data-stu-id="dc44e-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="dc44e-114">直接從 [Python](https://www.python.org/) 取得</span><span class="sxs-lookup"><span data-stu-id="dc44e-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="dc44e-115">從信譽良好的散發版本取得，例如 [Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/) 或 [ActiveState](https://www.activestate.com/)</span><span class="sxs-lookup"><span data-stu-id="dc44e-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="dc44e-116">從原始碼組建！</span><span class="sxs-lookup"><span data-stu-id="dc44e-116">Build from source!</span></span>

<span data-ttu-id="dc44e-117">除非您有特定的需求，否則我們建議您採用前兩個選項。</span><span class="sxs-lookup"><span data-stu-id="dc44e-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="dc44e-118">使用 pip 進行安裝</span><span class="sxs-lookup"><span data-stu-id="dc44e-118">Installation with pip</span></span>

<span data-ttu-id="dc44e-119">您可以個別安裝每個 Azure 服務的程式庫：</span><span class="sxs-lookup"><span data-stu-id="dc44e-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="dc44e-120">可以使用 `--pre` 旗標來安裝預覽版套件︰</span><span class="sxs-lookup"><span data-stu-id="dc44e-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="dc44e-121">您也可以在單行中使用 `azure` 中繼套件來安裝一組 Azure 程式庫。</span><span class="sxs-lookup"><span data-stu-id="dc44e-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="dc44e-122">我們發佈這個套件的預覽版本，您可以使用 --預先旗標加以存取：</span><span class="sxs-lookup"><span data-stu-id="dc44e-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="dc44e-123">從 GitHub 安裝</span><span class="sxs-lookup"><span data-stu-id="dc44e-123">Install from GitHub</span></span>

<span data-ttu-id="dc44e-124">如果您需要從來源安裝 `azure`：</span><span class="sxs-lookup"><span data-stu-id="dc44e-124">If you want to install `azure` from source:</span></span>

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a><span data-ttu-id="dc44e-125">使用 pip 安裝較舊版本</span><span class="sxs-lookup"><span data-stu-id="dc44e-125">Install an older version with pip</span></span>
<span data-ttu-id="dc44e-126">您可以指定 'azure==3.0.0' 版本詳細資料，以安裝較舊版本的 `azure`。</span><span class="sxs-lookup"><span data-stu-id="dc44e-126">You can install an older version of `azure` by specifying 'azure==3.0.0' version details.</span></span>
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a><span data-ttu-id="dc44e-127">使用 pip 檢查 SDK 安裝詳細資料</span><span class="sxs-lookup"><span data-stu-id="dc44e-127">Check SDK installation details with pip</span></span>
<span data-ttu-id="dc44e-128">您可以檢查 `azure` SDK 安裝位置、版本詳細資料等。</span><span class="sxs-lookup"><span data-stu-id="dc44e-128">You can check `azure` SDK installation location, version details etc.</span></span>
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a><span data-ttu-id="dc44e-129">使用 pip 解除安裝</span><span class="sxs-lookup"><span data-stu-id="dc44e-129">To uninstall with pip</span></span>
<span data-ttu-id="dc44e-130">您可以在單行中使用 `azure` 中繼套件來安裝所有 Azure 程式庫。</span><span class="sxs-lookup"><span data-stu-id="dc44e-130">You can uninstall all Azure libraries in a single line using the `azure` meta-package.</span></span>
```bash
pip uninstall azure 
```
> [!NOTE]
> <span data-ttu-id="dc44e-131">`pip uninstall azure`移除 `azure` 中繼套件但保留個別的 `azure-*` 套件 (以及其他項目，像是 `adal` 和 `msrest`)。</span><span class="sxs-lookup"><span data-stu-id="dc44e-131">`pip uninstall azure`removes the `azure` meta-package but leaves the individual `azure-*` packages behind (and others, like `adal` and `msrest` ).</span></span> <span data-ttu-id="dc44e-132">Python 和 pip 的概念是所有套件都具備相依性，解除安裝最初的套件並不會解除安裝相依性。</span><span class="sxs-lookup"><span data-stu-id="dc44e-132">An aspect of Python and pip is that for all packages that have dependencies, uninstalling the initial package does not uninstall the dependencies.</span></span> <span data-ttu-id="dc44e-133">要移除 `azure-` 乗其支援套件，請執行 `pip freeze | grep 'azure-' | xargs pip uninstall -y` 命令 (再分別解除安裝 adal、msrest 和 msrestazure )。</span><span class="sxs-lookup"><span data-stu-id="dc44e-133">To remove `azure-` and its supporting packages, run the command `pip freeze | grep 'azure-' | xargs pip uninstall -y` (and then perform individual uninstalls for adal, msrest, and msrestazure).</span></span>

