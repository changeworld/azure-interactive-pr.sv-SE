---
title: ta med fil
description: ta med fil
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f51b864cab14273c1e88dd85d22400e0e76ef770
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460014"
---
<span data-ttu-id="76c66-103">Nu är programmet ett fungerande galleri som du kan ladda upp bilder till och visa bilder i.</span><span class="sxs-lookup"><span data-stu-id="76c66-103">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="76c66-104">I den här modulen lär du dig hur du använder API:et för visuellt innehåll från Microsoft Cognitive Services för att generera bildtexter för uppladdade bilder och spara dem med bildmetadata i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="76c66-104">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="76c66-105">Skapa ett konto för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="76c66-105">Create a Computer Vision account</span></span>

<span data-ttu-id="76c66-106">Microsoft Cognitive Services är en samling tjänster som utvecklare kan använda för att göra sina program mer intelligenta.</span><span class="sxs-lookup"><span data-stu-id="76c66-106">Microsoft Cognitive Services is a collection of services available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="76c66-107">API:et för visuellt innehåll är en serverlös tjänst som bearbetar bilder med hjälp av avancerade algoritmer och som returnerar information om varje bild.</span><span class="sxs-lookup"><span data-stu-id="76c66-107">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="76c66-108">Det har en kostnadsfri nivå för upp till 5 000 API-anrop per månad.</span><span class="sxs-lookup"><span data-stu-id="76c66-108">It has a free tier that provides up to 5000 API calls per month.</span></span>

1. <span data-ttu-id="76c66-109">Kontrollera att du fortfarande är inloggad i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="76c66-109">Ensure you are still signed into the Cloud Shell.</span></span> <span data-ttu-id="76c66-110">Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="76c66-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="76c66-111">Skapa ett nytt Cognitive Services-konto av typen **ComputerVision** med ett unikt namn i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="76c66-111">Create a new Cognitive Services account of kind **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="76c66-112">För den kostnadsfria nivån använder du **F0** som SKU.</span><span class="sxs-lookup"><span data-stu-id="76c66-112">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="76c66-113">Om du redan har ett befintligt konto för visuellt innehåll kan du behöva skapa ett standardkonto (S1). Detta kan dock medföra vissa kostnader.</span><span class="sxs-lookup"><span data-stu-id="76c66-113">If you already have an existing Computer Vision account, you may need to create a Standard account (S1); however, this may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="76c66-114">Skapa funktionsappinställningar för nyckeln och URL:en för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="76c66-114">Create Function App settings for Computer Vision URL and key</span></span>

<span data-ttu-id="76c66-115">En URL och en nyckel krävs för att anropa API:et för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="76c66-115">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="76c66-116">Hämta nyckeln och URL:en för API:et för visuellt innehåll och spara dem i bash-variabler.</span><span class="sxs-lookup"><span data-stu-id="76c66-116">Obtain the Computer Vision API key and URL and save them in bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="76c66-117">Skapa appinställningar med namnen **COMP_VISION_KEY** och **COMP_VISION_URL** i funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="76c66-117">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a><span data-ttu-id="76c66-118">Anropa API:et för visuellt innehåll från funktionen ResizeImage</span><span class="sxs-lookup"><span data-stu-id="76c66-118">Call Computer Vision API from ResizeImage function</span></span>

<span data-ttu-id="76c66-119">I följande steg ska du ändra funktionen **ResizeImage** och anropa API:et för visuellt innehåll för att beskriva varje uppladdad bild och spara beskrivningen i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="76c66-119">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Cosmos DB.</span></span>

1. <span data-ttu-id="76c66-120">Öppna funktionsappen på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="76c66-120">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="76c66-121">**C#**</span><span class="sxs-lookup"><span data-stu-id="76c66-121">**C#**</span></span>

    1. <span data-ttu-id="76c66-122">(C#) Leta upp funktionen **ResizeImage** från det vänstra navigeringsfönstret och öppna funktionens kodfönster.</span><span class="sxs-lookup"><span data-stu-id="76c66-122">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

    1. <span data-ttu-id="76c66-123">(C#) Ersätt koden med innehållet i [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span><span class="sxs-lookup"><span data-stu-id="76c66-123">(C#) Replace the code with the contents of [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span></span> <span data-ttu-id="76c66-124">Den här koden använder `HttpClient` för att anropa API:et för visuellt innehåll och spara resultatet i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="76c66-124">This code uses `HttpClient` to call Computer Vision API and save its result in Cosmos DB.</span></span>

1. <span data-ttu-id="76c66-125">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="76c66-125">**JavaScript**</span></span>

    1. <span data-ttu-id="76c66-126">(JavaScript) Den här funktionen kräver `axios`-paketet från npm för att göra ett HTTP-anrop till API:et för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="76c66-126">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="76c66-127">Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="76c66-127">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="76c66-128">(JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="76c66-128">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="76c66-129">(JavaScript) Kör kommandot `npm install axios` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="76c66-129">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="76c66-130">Åtgärden kan ta ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="76c66-130">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="76c66-131">(JavaScript) Klicka på funktionsnamnet (**ResizeImage**) i det vänstra navigeringsfönstret för att visa funktionen och ersätt allt innehåll i **index.js** med innehållet i [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span><span class="sxs-lookup"><span data-stu-id="76c66-131">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function, and then replace all of **index.js** with the contents of [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span></span>

1. <span data-ttu-id="76c66-132">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="76c66-132">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="76c66-133">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="76c66-133">Click **Save**.</span></span> <span data-ttu-id="76c66-134">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="76c66-134">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="76c66-135">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="76c66-135">Test the application</span></span>

1. <span data-ttu-id="76c66-136">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="76c66-136">Open the application in a browser.</span></span> <span data-ttu-id="76c66-137">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="76c66-137">Select an image file, and upload it.</span></span>

1. <span data-ttu-id="76c66-138">Efter några sekunder bör miniatyrbilden för den nya bilden visas på sidan.</span><span class="sxs-lookup"><span data-stu-id="76c66-138">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="76c66-139">Hovra över bilden för att visa beskrivningen som genererats av Visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="76c66-139">Hover over the image to see the description generated by Computer Vision.</span></span>


## <a name="summary"></a><span data-ttu-id="76c66-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="76c66-140">Summary</span></span>

<span data-ttu-id="76c66-141">I den här modulen har du lärt dig hur du automatiskt skapar bildtexter för uppladdade bilder med hjälp av API:et för visuellt innehåll i Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="76c66-141">In this module, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="76c66-142">I nästa avsnitt lär du dig hur du lägger till autentisering till programmet med hjälp av Azure App Service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="76c66-142">Next, you learn how to add authentication to the application using Azure App Service Authentication.</span></span>
