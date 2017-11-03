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
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a>安裝

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
