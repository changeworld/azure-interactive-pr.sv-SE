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
ms.openlocfilehash: 51c7d3e64424d499b473f3b138ce249a9cfd0182
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460099"
---
Programmet som du skapar är ett fotogalleri. Programmet använder JavaScript på klientsidan för att anropa API:er för uppladdning och visning av bilder. I den här modulen skapar du ett API med hjälp av en serverlös funktion som genererar en tidsbegränsad URL för uppladdning av en bild. Webbprogrammet använder den genererade URL:en för att ladda upp en avbildning till Blob Storage med hjälp av [REST-API:et för Blob Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

## <a name="create-a-blob-storage-container-for-images"></a>Skapa en Blob Storage-container för bilder

Programmet kräver en separat lagringscontainer för att ladda upp och lagra bilder.

1. Kontrollera att du fortfarande är inloggad i Cloud Shell (bash). Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.

1.  Skapa en ny container med namnet **images** (bilder) i ditt lagringskonto med offentlig åtkomst till alla blobbar.

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

Med tjänsten Azure Functions kan du köra serverlösa funktioner. En serverlös funktion kan utlösas (anropas) av händelser som till exempel en HTTP-begäran eller när en blobb skapas i en lagringscontainer.

En Azure Functions-app är en container för en eller flera serverlösa funktioner.

Skapa en ny Azure Functions-app med ett unikt namn i resursgruppen som du skapade tidigare med namnet **first-serverless-app**. Funktionsappar kräver ett lagringskonto. I den här självstudien använder du det befintliga lagringskontot.

```azurecli
az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
```

## <a name="configure-the-function-app"></a>Konfigurera funktionsappen

För funktionsappen i den här självstudien krävs version 1.x av körmiljön för Functions. Om du anger programinställningen för `FUNCTIONS_WORKER_RUNTIME` som `~1` så fästs funktionsappen till den senaste 1.x-versionen. Ange programinställningar med kommandot [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set).

I följande Azure CLI-kommando är <app_name> namnet på funktionsappen.

```azurecli
az functionapp config appsettings set --name <function app name> --g first-serverless-app --settings FUNCTIONS_WORKER_RUNTIME=~1
```

## <a name="create-an-http-triggered-serverless-function"></a>Skapa en HTTP-utlöst serverlös funktion

Webbprogrammet för fotogalleriet skickar en HTTP-begäran till den serverlösa funktionen för att generera en tidsbegränsad URL för säker uppladdning av en bild till Blob Storage. Funktionen utlöses av en HTTP-begäran och använder Azure Storage SDK för att generera och returnera den säkra URL:en.

1. När funktionsappen har skapats kan du söka efter den på Azure Portal med hjälp av sökrutan och klicka för att öppna den.

    ![Öppna funktionsappen](media/functions-first-serverless-web-app/2-search-function-app.png)

1. Hovra över **Funktioner** i det vänstra navigeringsfönstret i funktionsappens fönster och klicka på **+** för att börja skapa en ny serverlös funktion.

    ![Skapa en ny funktion](media/functions-first-serverless-web-app/2-new-function.png)

1. Klicka på **Anpassad funktion** för att visa en lista med funktionsmallar.

1. Leta upp mallen **HttpTrigger** och klicka på önskat språk (C# eller JavaScript).

1. Använd dessa värden för att skapa en funktion som genererar en blobbuppladdnings-URL.

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **Språk** | C# eller JavaScript | Välj det språk som du vill använda. |
    | **Namnge din funktion** | GetUploadUrl | Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen. |
    | **Auktorisationsnivå** | Anonym | Gör funktionen offentligt tillgänglig. |

    ![Ange inställningarna för en ny HTTP-utlöst funktion](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. Skapa funktionen genom att klicka på **Skapa**.

1. **C#** 

    1. När funktionens källkod visas ersätter du allt innehåll i **run.csx** med innehållet i [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).

1. **JavaScript** 

    1. (JavaScript) Den här funktionen kräver `azure-storage`-paketet från npm för att generera den SAS-token (signatur för delad åtkomst) som krävs för att skapa den säkra URL:en. Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.

    1. (JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.

        ![Öppna ett konsolfönster](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. (JavaScript) Kontrollera att den aktuella katalogen är **d:\home\site\wwwroot** genom att köra kommandot `cd d:\home\site\wwwroot`.

    1. (JavaScript) Kör kommandot `npm init -y` för att skapa en tom **package.json**-fil.

    1. (JavaScript) Kör kommandot `npm install --save azure-storage` i konsolen för att installera paketet och spara det i **package.json**. Åtgärden kan ta ett par minuter.

    1. (JavaScript) Klicka på funktionsnamnet (**GetUploadUrl**) i det vänstra navigeringsfönstret för att visa funktionen och ersätt allt innehåll i **index.js** med innehållet i [ **javascript/GetUploadUrl / index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).
    
        ![index.js efter uppdateringen](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.

1. Klicka på **Spara**. Granska loggpanelen för att bekräfta att funktionen har kompilerats.

Funktionen genererar en så kallad SAS-URL (signatur för delad åtkomst) som används för att ladda upp en fil till Blob Storage. SAS-URL:en är giltig en kort tid och tillåter bara uppladdning av en enda fil. Läs dokumentationen för Blob Storage om du vill veta mer om [hur du använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>Lägg till en miljövariabel för anslutningssträngen för lagring

Funktionen som du nyss skapat behöver en anslutningssträng för lagringskontot för att kunna generera SAS-URL:en. I stället för att hårdkoda anslutningssträngen i funktionens kod, kan du lagra den som en programinställning. Alla funktioner i funktionsappen kan komma åt programinställningar som miljövariabler.

1. Hämta anslutningssträngen för lagringskontot från Cloud Shell och spara den i en bash-variabel med namnet **STORAGE_CONNECTION_STRING**.

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    Bekräfta att variabeln är korrekt angiven.

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. Skapa en ny programinställning med namnet **AZURE_STORAGE_CONNECTION_STRING** genom att använda värdet som sparades i föregående steg.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    Bekräfta att kommandots utdata innehåller den nya programinställningen med rätt värde.


## <a name="test-the-serverless-function"></a>Testa den serverlösa funktionen

Förutom att skapa och redigera funktioner innehåller Azure Portal även ett inbyggt verktyg för att testa funktioner.

1. Testa den serverlösa HTTP-funktionen genom att klicka på fliken **Testa** till höger om kodfönstret för att expandera testpanelen.

1. Ändra **Http-metoden** till **GET**.

1. Klicka på *Lägg till parameter** under **Fråga** och lägg till följande parameter:

    | namn      |  värde   | 
    | --- | --- |
    | filename | image1.jpg |

1. Klicka på **Kör** på testpanelen för att skicka en HTTP-begäran till funktionen.

1. Funktionen returnerar en uppladdnings-URL. Funktionskörningen visas på loggpanelen.

    ![Loggar som visar att funktionen körts utan problem](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a>Konfigurera CORS i funktionsappen

Eftersom appens klientdel finns i Blob Storage har den ett annat domännamn än Azure Functions-appen. För att JavaScript-koden på klientsidan ska kunna anropa funktionen som du precis skapat måste funktionsappen konfigureras för CORS (Cross-Origin Resource Sharing).

1. Klicka på namnet på funktionsappen i det vänstra navigeringsfältet i funktionsappens fönster.

1. Visa en lista med avancerade funktioner genom att klicka på **Plattformsfunktioner**.

1. Klicka på **CORS** under **API**.

    ![Välj CORS](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. Lägg till tillåtna ursprung för program-URL:en från den föregående modulen, men utelämna det avslutande **/** (till exempel: `https://firstserverlessweb.z4.web.core.windows.net`).

    ![CORS-inställningar som visar att den serverlös webbappens URL har lagts till](media/functions-first-serverless-web-app/2-add-cors.png)

1. Klicka på **Spara**.

1. C#

   1. (C#) Gå tillbaka till funktionen `GetUploadUrl` och välj sedan fliken **Integrera**.

   1. (C#) Välj **OPTIONS** under Utvalda HTTP-metoder.

      **GET**, **POST** och **OPTIONS** bör vara markerade. CORS använder OPTIONS-metoden, som inte är markerad som standard för C#-funktioner.  

   1. (C#) Klicka på **Spara**.

1. Navigera till funktionsappen från portalen, välj fliken **Översikt** och klicka sedan på **Starta om** för att bekräfta att ändringarna för CORS har tillämpats.

## <a name="configure-cors-in-the-storage-account"></a>Konfigurera CORS i lagringskontot

Eftersom appen även gör JavaScript-anrop till Blob Storage för filuppladdning måste du även konfigurera lagringskontot för CORS.

1. Kör följande kommando för att tillåta att alla ursprung laddar upp filer till lagringskontot.

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Ändra webbappen för att ladda upp bilder

Webbappen hämtar inställningar från en fil med namnet **settings.js**. I följande steg ska du skapa filen med hjälp av Cloud Shell och sedan ange `window.apiBaseUrl` till URL:en för funktionsappen och `window.blobBaseUrl` till URL:en för Azure Blob Storage-slutpunkten.

1. Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Kör en fråga för att hämta appens URL och lagra den i en bash-variabel med namnet **FUNCTION_APP_URL**.

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    Bekräfta att variabeln är korrekt angiven.

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. Ställ in bas-URI:n för API-anrop till funktionsappen genom att skapa **settings.js** och lägga till funktionsappens URL som du ser nedan.

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    Du kan göra ändringen genom att köra följande kommando eller genom att använda en kommandoradsredigerare som VIM.

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    Kontrollera att filen skrivits korrekt.

    ```azurecli
    cat settings.js
    ```

1. Kör en fråga för att hämta bas-URL:en för Blob Storage och lagra den i en bash-variabel med namnet **BLOB_BASE_URL**.

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    Bekräfta att variabeln är korrekt angiven.

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. Ställ in bas-URI:n för API-anrop till funktionsappen genom att lägga till lagrings-URL:en som i följande kodrad i **settings.js**.

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    Du kan göra ändringen genom att köra följande kommando eller genom att använda en kommandoradsredigerare som VIM.

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    Bekräfta att filen skrivits korrekt och att den nu innehåller två rader.

    ```azurecli
    cat settings.js
    ```

1. Ladda upp filen till Blob Storage.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a>Testa webbprogrammet

Nu kan galleriprogrammet ladda upp en bild till Blob Storage, men den kan inte visa bilder än. Den kommer att försöka anropa en `GetImages`-funktion som inte finns ännu eftersom du skapar den i en senare modul. Anropet misslyckas och sidan verkar ha fastnat på ”Analyserar...”, men bilden du valt kommer att laddas upp korrekt.

Du kan kontrollera att en bild har laddats upp genom att kontrollera innehållet i containern **images** på Azure Portal.

1. Gå till programmet i ett webbläsarfönster. Välj en bildfil och ladda upp den. Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot. (Sidan verkar ha fastnat på ”Analyserar bild...”. Du ska åtgärda detta senare.)

1. Bekräfta i Cloud Shell att bilden har laddats upp till containern **images**.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Ta bort alla filer i containern **images** innan du går vidare till nästa självstudie.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat en Azure Functions-app och lärt dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från ett webbprogram. I nästa avsnitt lär du dig hur du skapar miniatyrbilder för de uppladdade bilderna med hjälp av en blobbutlöst serverlös funktion.
