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
ms.openlocfilehash: 5ce4ef27667d45697200eef67be92c62812b3809
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2017
---
# <a name="installation"></a><span data-ttu-id="c504a-104">安裝</span><span class="sxs-lookup"><span data-stu-id="c504a-104">Installation</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="c504a-105">使用 pip 進行安裝</span><span class="sxs-lookup"><span data-stu-id="c504a-105">Installation with pip</span></span>

<span data-ttu-id="c504a-106">您可以個別安裝每個 Azure 服務的程式庫：</span><span class="sxs-lookup"><span data-stu-id="c504a-106">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="c504a-107">可以使用 `--pre` 旗標來安裝預覽版套件︰</span><span class="sxs-lookup"><span data-stu-id="c504a-107">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="c504a-108">您也可以在單行中使用 `azure` 中繼套件來安裝一組 Azure 程式庫。</span><span class="sxs-lookup"><span data-stu-id="c504a-108">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="c504a-109">我們發佈這個套件的預覽版本，您可以使用 --預先旗標加以存取：</span><span class="sxs-lookup"><span data-stu-id="c504a-109">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="c504a-110">從 GitHub 安裝</span><span class="sxs-lookup"><span data-stu-id="c504a-110">Install from GitHub</span></span>

<span data-ttu-id="c504a-111">如果您需要從來源安裝 `azure`：</span><span class="sxs-lookup"><span data-stu-id="c504a-111">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
