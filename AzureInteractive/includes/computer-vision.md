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
Nu är programmet ett fungerande galleri som du kan ladda upp bilder till och visa bilder i. I den här modulen lär du dig hur du använder API:et för visuellt innehåll från Microsoft Cognitive Services för att generera bildtexter för uppladdade bilder och spara dem med bildmetadata i Cosmos DB.

## <a name="create-a-computer-vision-account"></a>Skapa ett konto för visuellt innehåll

Microsoft Cognitive Services är en samling tjänster som utvecklare kan använda för att göra sina program mer intelligenta. API:et för visuellt innehåll är en serverlös tjänst som bearbetar bilder med hjälp av avancerade algoritmer och som returnerar information om varje bild. Det har en kostnadsfri nivå för upp till 5 000 API-anrop per månad.

1. Kontrollera att du fortfarande är inloggad i Cloud Shell. Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster. 

1. Skapa ett nytt Cognitive Services-konto av typen **ComputerVision** med ett unikt namn i resursgruppen. För den kostnadsfria nivån använder du **F0** som SKU. Om du redan har ett befintligt konto för visuellt innehåll kan du behöva skapa ett standardkonto (S1). Detta kan dock medföra vissa kostnader.

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a>Skapa funktionsappinställningar för nyckeln och URL:en för visuellt innehåll

En URL och en nyckel krävs för att anropa API:et för visuellt innehåll.

1. Hämta nyckeln och URL:en för API:et för visuellt innehåll och spara dem i bash-variabler.

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. Skapa appinställningar med namnen **COMP_VISION_KEY** och **COMP_VISION_URL** i funktionsappen.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a>Anropa API:et för visuellt innehåll från funktionen ResizeImage

I följande steg ska du ändra funktionen **ResizeImage** och anropa API:et för visuellt innehåll för att beskriva varje uppladdad bild och spara beskrivningen i Cosmos DB.

1. Öppna funktionsappen på Azure Portal.

1. **C#**

    1. (C#) Leta upp funktionen **ResizeImage** från det vänstra navigeringsfönstret och öppna funktionens kodfönster.

    1. (C#) Ersätt koden med innehållet i [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx). Den här koden använder `HttpClient` för att anropa API:et för visuellt innehåll och spara resultatet i Cosmos DB.

1. **JavaScript**

    1. (JavaScript) Den här funktionen kräver `axios`-paketet från npm för att göra ett HTTP-anrop till API:et för visuellt innehåll. Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.

    1. (JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.

    1. (JavaScript) Kör kommandot `npm install axios` i konsolen. Åtgärden kan ta ett par minuter.

    1. (JavaScript) Klicka på funktionsnamnet (**ResizeImage**) i det vänstra navigeringsfönstret för att visa funktionen och ersätt allt innehåll i **index.js** med innehållet i [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).

1. Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.

1. Klicka på **Spara**. Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.


## <a name="test-the-application"></a>Testa programmet

1. Öppna programmet i en webbläsare. Välj en bildfil och ladda upp den.

1. Efter några sekunder bör miniatyrbilden för den nya bilden visas på sidan. Hovra över bilden för att visa beskrivningen som genererats av Visuellt innehåll.


## <a name="summary"></a>Sammanfattning

I den här modulen har du lärt dig hur du automatiskt skapar bildtexter för uppladdade bilder med hjälp av API:et för visuellt innehåll i Microsoft Cognitive Services. I nästa avsnitt lär du dig hur du lägger till autentisering till programmet med hjälp av Azure App Service-autentisering.
