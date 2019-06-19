---
title: 適用於 Python 的 Azure 認知服務模組
description: 適用於 Python 的 Azure 認知服務模組參考
keywords: Azure, python, SDK, API, 認知服務
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 04/04/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5890c2091f8456dd9b8bcb68f8a34eed3cae6e04
ms.sourcegitcommit: d7ad0e8b4ba4add5e6f63e6b9eac54ecccdc7090
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148165"
---
# <a name="azure-cognitive-services-modules-for-python"></a><span data-ttu-id="844bd-104">適用於 Python 的 Azure 認知服務模組</span><span class="sxs-lookup"><span data-stu-id="844bd-104">Azure Cognitive Services modules for Python</span></span>

<span data-ttu-id="844bd-105">使用適用於 Python 的 Azure 認知服務模組新增影像和臉部辨識、語言分析並搜尋您的 Python 應用程式、網站和工具。</span><span class="sxs-lookup"><span data-stu-id="844bd-105">Add image and face recognition, language analysis, and search to your Python apps, websites, and tools using the Azure Cognitive Services modules for Python.</span></span>

## <a name="vision-modules"></a><span data-ttu-id="844bd-106">願景視覺</span><span class="sxs-lookup"><span data-stu-id="844bd-106">Vision modules</span></span>

### <a name="computer-vision"></a><span data-ttu-id="844bd-107">電腦視覺</span><span class="sxs-lookup"><span data-stu-id="844bd-107">Computer Vision</span></span> 

<span data-ttu-id="844bd-108">傳回在影像中找到的視覺化內容資訊：</span><span class="sxs-lookup"><span data-stu-id="844bd-108">Returns information about visual content found in an image:</span></span>

- <span data-ttu-id="844bd-109">使用標籤、描述及網域特定模型，自信從容地識別內容並加上標籤。</span><span class="sxs-lookup"><span data-stu-id="844bd-109">Use tagging, descriptions, and domain-specific models to identify content and label it with confidence.</span></span>
- <span data-ttu-id="844bd-110">套用成人/猥瑣設定，以啟用自動限制成人內容。</span><span class="sxs-lookup"><span data-stu-id="844bd-110">Apply adult/racy settings to enable automated restriction of adult content.</span></span>
- <span data-ttu-id="844bd-111">識別圖片中的影像類型及色彩配置。</span><span class="sxs-lookup"><span data-stu-id="844bd-111">Identify image types and color schemes in pictures.</span></span>

<span data-ttu-id="844bd-112">在您的瀏覽器中免費[試用電腦視覺](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-112">[Try Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) for free in your browser.</span></span>

<span data-ttu-id="844bd-113">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-113">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-vision-computervision
```

<span data-ttu-id="844bd-114">[進一步了解](/azure/cognitive-services/computer-vision/home)電腦視覺 API 相關資訊，並參考[電腦視覺 API Python 快速入門](/azure/cognitive-services/computer-vision/quickstarts/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-114">[Learn more](/azure/cognitive-services/computer-vision/home) about the Computer Vision API and get started with the [Computer Vision API Python quickstart](/azure/cognitive-services/computer-vision/quickstarts/python).</span></span>

### <a name="content-moderator"></a><span data-ttu-id="844bd-115">內容仲裁</span><span class="sxs-lookup"><span data-stu-id="844bd-115">Content Moderator</span></span>

<span data-ttu-id="844bd-116">由機器協助進行文字、影片及影像審核，並透過人工審核工具加強。</span><span class="sxs-lookup"><span data-stu-id="844bd-116">Machine-assisted moderation of text, video and images, augmented with human review tools.</span></span>

<span data-ttu-id="844bd-117">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-117">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-vision-contentmoderator
```

<span data-ttu-id="844bd-118">[深入了解 ](/azure/cognitive-services/content-moderator/overview)Content Moderator API。</span><span class="sxs-lookup"><span data-stu-id="844bd-118">[Learn more](/azure/cognitive-services/content-moderator/overview) about the Content Moderator service.</span></span>

### <a name="custom-vision-service"></a><span data-ttu-id="844bd-119">自訂辨識服務</span><span class="sxs-lookup"><span data-stu-id="844bd-119">Custom Vision Service</span></span>

<span data-ttu-id="844bd-120">上傳影像，為特殊使用案例定型和自訂電腦視覺模型。</span><span class="sxs-lookup"><span data-stu-id="844bd-120">Upload images to train and customize a computer vision model for your specific use case.</span></span> <span data-ttu-id="844bd-121">一旦模型已定型，您可以使用 API 來標記使用模型的影像，並評估結果以改善您的分類器。</span><span class="sxs-lookup"><span data-stu-id="844bd-121">Once the model is trained, you can use the API to tag images using the model and evaluate the results to improve your classifier.</span></span>

<span data-ttu-id="844bd-122">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-122">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-vision-customvision
```

<span data-ttu-id="844bd-123">[進一步了解](/azure/cognitive-services/Custom-Vision-Service/home)自訂視覺服務，並參考[自訂視覺 Python 教學課程](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)開始使用</span><span class="sxs-lookup"><span data-stu-id="844bd-123">[Learn more](/azure/cognitive-services/Custom-Vision-Service/home) about the Custom Vision service and get started with the [Custom Vision Python tutorial](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)</span></span>

### <a name="face-api"></a><span data-ttu-id="844bd-124">人臉識別 API</span><span class="sxs-lookup"><span data-stu-id="844bd-124">Face API</span></span>

<span data-ttu-id="844bd-125">偵測、識別、分析、組織和標記相片中的臉孔。</span><span class="sxs-lookup"><span data-stu-id="844bd-125">Detect, identify, analyze, organize, and tag faces in photos.</span></span> 

<span data-ttu-id="844bd-126">在瀏覽器中[試用臉部 API](https://azure.microsoft.com/en-us/services/cognitive-services/face/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-126">[Try the Face API](https://azure.microsoft.com/en-us/services/cognitive-services/face/) in your browser.</span></span>

<span data-ttu-id="844bd-127">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-127">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install cognitive-face
```

<span data-ttu-id="844bd-128">[進一步了解](/azure/cognitive-services/face/overview)臉部 API，並參考[臉部 API Python 快速入門](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-128">[Learn more](/azure/cognitive-services/face/overview) about the Face API and get started with the [Face API Python quickstart](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial).</span></span>

## <a name="search-modules"></a><span data-ttu-id="844bd-129">搜尋模組</span><span class="sxs-lookup"><span data-stu-id="844bd-129">Search modules</span></span>

### <a name="web-search"></a><span data-ttu-id="844bd-130">Web 搜尋</span><span class="sxs-lookup"><span data-stu-id="844bd-130">Web search</span></span>

<span data-ttu-id="844bd-131">擷取由 Bing 網路搜尋 API 編製索引的網路文件，並透過結果類型、新鮮度等等，縮小結果範圍。</span><span class="sxs-lookup"><span data-stu-id="844bd-131">Retrieve web documents indexed by the Bing Web Search API and narrow down the results by result type, freshness and more.</span></span> 

<span data-ttu-id="844bd-132">在瀏覽器中[試用 Web 搜尋 API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-132">[Try the Web Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/) in your browser.</span></span>

<span data-ttu-id="844bd-133">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-133">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-websearch
```

<span data-ttu-id="844bd-134">[進一步了解 ](/azure/cognitive-services/bing-web-search/overview)Bing Web 搜尋 API，並參考 [Web 搜尋 API Python 快速入門](/azure/cognitive-services/bing-web-search/quickstarts/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-134">[Learn more](/azure/cognitive-services/bing-web-search/overview) about the Bing Web Search API and get started with the [Web Search API Python quickstart](/azure/cognitive-services/bing-web-search/quickstarts/python).</span></span>

### <a name="image-search"></a><span data-ttu-id="844bd-135">影像搜尋</span><span class="sxs-lookup"><span data-stu-id="844bd-135">Image search</span></span>

<span data-ttu-id="844bd-136">搜尋影像並在結果中取得縮圖、完整的影像 URL、影像中繼資料等資訊。</span><span class="sxs-lookup"><span data-stu-id="844bd-136">Search for images and get thumbnails, full image URLs, image metadata and more in your results.</span></span>

<span data-ttu-id="844bd-137">在瀏覽器中[試用影像搜尋 API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-137">[Try the Image Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) in your browser.</span></span>

<span data-ttu-id="844bd-138">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-138">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-imagesearch
```

<span data-ttu-id="844bd-139">[進一步了解 ](/azure/cognitive-services/bing-image-search/overview)Bing 影像搜尋 API，並參考[影像搜尋 API Python 快速入門](/azure/cognitive-services/bing-image-search/quickstarts/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-139">[Learn more](/azure/cognitive-services/bing-image-search/overview) about the Bing Image Search API and get started with the [Image Search API Python quickstart](/azure/cognitive-services/bing-image-search/quickstarts/python).</span></span>


### <a name="entity-search"></a><span data-ttu-id="844bd-140">實體搜尋</span><span class="sxs-lookup"><span data-stu-id="844bd-140">Entity search</span></span>

<span data-ttu-id="844bd-141">針對給定的搜尋字詞或位置，搜尋最相關的實體 (位置、個人或事物) 動作。</span><span class="sxs-lookup"><span data-stu-id="844bd-141">Search for the most relevant entity (place, person, or thing) for a given search term or location.</span></span>

<span data-ttu-id="844bd-142">在瀏覽器中[試用實體搜尋 API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-142">[Try the Entity Search API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) in your browser.</span></span>

<span data-ttu-id="844bd-143">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-143">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-entitysearch
```

<span data-ttu-id="844bd-144">[進一步了解 ](/azure/cognitive-services/bing-entities-search/search-the-web)Bing 實體搜尋 API，並參考[實體搜尋 API Python 快速入門](/azure/cognitive-services/bing-entities-search/quickstarts/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-144">[Learn more](/azure/cognitive-services/bing-entities-search/search-the-web) about the Bing Entity Search API and get started with the [Entity Search API Python quickstart](/azure/cognitive-services/bing-entities-search/quickstarts/python).</span></span>

### <a name="custom-search"></a><span data-ttu-id="844bd-145">自訂搜尋</span><span class="sxs-lookup"><span data-stu-id="844bd-145">Custom search</span></span>

<span data-ttu-id="844bd-146">建置符合特定搜尋範圍的自訂 Web 搜尋。</span><span class="sxs-lookup"><span data-stu-id="844bd-146">Build and a custom web search that meets your specific search domain.</span></span>

<span data-ttu-id="844bd-147">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-147">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-customsearch
```

<span data-ttu-id="844bd-148">[進一步了解 ](/azure/cognitive-services/bing-custom-search/)Bing 自訂搜尋服務，並參考[自訂搜尋 API Python 快速入門](/azure/cognitive-services/bing-custom-search/call-endpoint-python)從 Python 查詢您的自訂搜尋。</span><span class="sxs-lookup"><span data-stu-id="844bd-148">[Learn more](/azure/cognitive-services/bing-custom-search/) about the Bing Custom Search service and get started with querying your custom search from Python with the [Custom Search API Python quickstart](/azure/cognitive-services/bing-custom-search/call-endpoint-python).</span></span>

### <a name="video-search"></a><span data-ttu-id="844bd-149">影片搜尋</span><span class="sxs-lookup"><span data-stu-id="844bd-149">Video search</span></span>

<span data-ttu-id="844bd-150">在網路上尋找影片，並取得建立者、編碼、長度和檢視計數中繼資料等結果。</span><span class="sxs-lookup"><span data-stu-id="844bd-150">Find videos across the web and get results with creator, encoding, length, and view count metadata.</span></span>

<span data-ttu-id="844bd-151">在瀏覽器中[試用影片搜尋 API](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-151">[Try the Video Search API](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) in your browser.</span></span>

<span data-ttu-id="844bd-152">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-152">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-videosearch
```

<span data-ttu-id="844bd-153">[進一步了解 ](/azure/cognitive-services/bing-video-search/search-the-web)Bing 影片搜尋服務，並參考[影片搜尋 API Python 快速入門](/azure/cognitive-services/bing-video-search/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-153">[Learn more](/azure/cognitive-services/bing-video-search/search-the-web) about the Bing Video Search service and get started with the [Video Search API Python quickstart](/azure/cognitive-services/bing-video-search/python).</span></span>


### <a name="news-search"></a><span data-ttu-id="844bd-154">新聞搜尋</span><span class="sxs-lookup"><span data-stu-id="844bd-154">News search</span></span>

<span data-ttu-id="844bd-155">搜尋網站上的新聞文章，並與文章、相關的新聞、影像和提供者資訊中繼資料搭配使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-155">Search the web for news articles and work with article, related news, images, and provider info metadata.</span></span>

<span data-ttu-id="844bd-156">在瀏覽器中[試用新聞搜尋 API](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-156">[Try the News Search API](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) in your browser.</span></span>

<span data-ttu-id="844bd-157">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-157">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-newssearch
```

<span data-ttu-id="844bd-158">[進一步了解 ](/azure/cognitive-services/bing-news-search/search-the-web)Bing 新聞搜尋服務，並參考[新聞搜尋 API Python 快速入門](//azure/cognitive-services/bing-news-search/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-158">[Learn more](/azure/cognitive-services/bing-news-search/search-the-web) about the Bing News Search service and get started with the [News Search API Python quickstart](//azure/cognitive-services/bing-news-search/python).</span></span>


## <a name="language-modules"></a><span data-ttu-id="844bd-159">語言模組</span><span class="sxs-lookup"><span data-stu-id="844bd-159">Language modules</span></span>

### <a name="text-analytics"></a><span data-ttu-id="844bd-160">文字分析</span><span class="sxs-lookup"><span data-stu-id="844bd-160">Text Analytics</span></span> 

<span data-ttu-id="844bd-161">文字分析 API 是雲端式服務，對未經處理的文字提供自然語言處理。</span><span class="sxs-lookup"><span data-stu-id="844bd-161">The Text Analytics API is a cloud-based service that provides  natural language processing over raw text.</span></span> <span data-ttu-id="844bd-162">此 API 包含三項主要功能：</span><span class="sxs-lookup"><span data-stu-id="844bd-162">The API includes three main functions:</span></span>

- <span data-ttu-id="844bd-163">情感分析</span><span class="sxs-lookup"><span data-stu-id="844bd-163">Sentiment analysis</span></span>
- <span data-ttu-id="844bd-164">關鍵片語擷取</span><span class="sxs-lookup"><span data-stu-id="844bd-164">Key phrase extraction</span></span>
- <span data-ttu-id="844bd-165">語言偵測</span><span class="sxs-lookup"><span data-stu-id="844bd-165">Language detection</span></span>

<span data-ttu-id="844bd-166">在瀏覽器中[試用文字分析 API](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-166">[Try the Text Analytics API](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) in your browser.</span></span>

<span data-ttu-id="844bd-167">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-167">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-language-textanalytics
```

<span data-ttu-id="844bd-168">[進一步了解](/azure/cognitive-services/text-analytics/overview)文字分析 API，並參考 [文字分析 API Python 快速入門](/azure/cognitive-services/text-analytics/quickstarts/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-168">[Learn more](/azure/cognitive-services/text-analytics/overview) about the Text Analytics API and get started with the [Text Analytics API Python quickstart](/azure/cognitive-services/text-analytics/quickstarts/python).</span></span>


### <a name="spell-check"></a><span data-ttu-id="844bd-169">拼字檢查</span><span class="sxs-lookup"><span data-stu-id="844bd-169">Spell Check</span></span>

<span data-ttu-id="844bd-170">透過 Bing 拼字檢查 API 執行內容相關的文法與拼字檢查。</span><span class="sxs-lookup"><span data-stu-id="844bd-170">Perform contextual grammar and spell checking with the Bing Spell Check API.</span></span>

<span data-ttu-id="844bd-171">在瀏覽器中[試用拼字檢查 API](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/)。</span><span class="sxs-lookup"><span data-stu-id="844bd-171">[Try the Spell Check API](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/) in your browser.</span></span>

<span data-ttu-id="844bd-172">取得內含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模組：</span><span class="sxs-lookup"><span data-stu-id="844bd-172">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-language-spellcheck
```

<span data-ttu-id="844bd-173">[進一步了解](/azure/cognitive-services/bing-spell-check/proof-text)拼字檢查 API，並參考[拼字檢查 API Python 快速入門](/azure/cognitive-services/bing-spell-check/quickstarts/python)開始使用。</span><span class="sxs-lookup"><span data-stu-id="844bd-173">[Learn more](/azure/cognitive-services/bing-spell-check/proof-text) about the Spell Check API and get started with the [Spell Check API Python quickstart](/azure/cognitive-services/bing-spell-check/quickstarts/python).</span></span>
