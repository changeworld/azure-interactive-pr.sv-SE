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
ms.openlocfilehash: 194a25dbf9abda80379aa5aab408ac4ffe9ab7f5
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460066"
---
Azure Cosmos DB är Microsofts serverlösa, globalt distribuerade databas för flera datamodeller. I den här modulen lär du dig hur du använder Azure Functions för att lagra och hämta bildmetadata som JSON-dokument i Cosmos DB.

## <a name="create-a-cosmos-db-account-database-and-collection"></a>Skapa ett konto, en databas och en samling i Cosmos DB

Ett Cosmos DB-konto är en Azure-resurs som innehåller Cosmos DB-databaser.

1. Kontrollera att du fortfarande är inloggad i Cloud Shell.  Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster. 

1. Skapa ett Cosmos DB-konto med ett unikt namn i samma resursgrupp som de andra resurserna i den här självstudien.

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. När Cosmos DB-kontot har skapats skapar du en ny databas med namnet **imagesdb** i kontot.

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. När databasen har skapats skapar du en ny samling med namnet **images** i databasen med ett dataflöde på 400 enheter för programbegäran (RU:er).

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a>Spara ett dokument till Cosmos DB när en miniatyrbild skapas

Med Cosmos DB-utdatabindningen kan du skapa dokument i en Cosmos DB-samling från Azure Functions. I följande steg ska du konfigurera en Cosmos DB-utdatabindning i funktionen **ResizeImage** och ändra funktionen för att returnera ett dokument (objekt) som ska sparas.

1. Öppna funktionsappen på Azure Portal.

1. Expandera funktionen **ResizeImage** i det vänstra navigeringsfönstret och välj **Integrera**.

1. Klicka på **Nya utdata** under **Utdata**.

1. Leta upp och markera **Azure Cosmos DB**-objektet. Klicka sedan på **Välj**.

    ![Välj Nya utdata](media/functions-first-serverless-web-app/4-new-output.jpg)

1. Fyll i fälten under **Azure Cosmos DB-utdata** med följande värden.

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **Dokumentparameternamn** | Välj **Använd funktionsreturvärde** | Värdet i textrutan ställs automatiskt in till **$return**. |
    | **Databasnamn** | imagesdb | Använd namnet på databasen som du skapade. |
    | **Samlingsnamn** | images | Använd namnet på samlingen som du skapat. |

1. Klicka på **Ny** bredvid **Azure Cosmos DB-kontoanslutning**. Välj Cosmos DB-kontot som du skapade tidigare.

    ![Ange inställningar för Azure Cosmos DB-utdatabindningen](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. Skapa Cosmos DB-utdatabindningen genom att klicka på **Spara**.

1. Klicka på funktionsnamnet **ResizeImage** till vänster för att öppna funktionen.

1. **C#**

    1. (C#) Ändra returtypen för funktionen från **void** till **object**.

    1. (C#) Returnera dokumentet som ska sparas genom att lägga till följande kodblock i slutet av funktionen:
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![run.csx för ResizeImages-funktionen efter ändringarna](media/functions-first-serverless-web-app/4-update-function.png)

1. **JavaScript**

    1. (JavaScript) Ändra `context.done()`-instruktionen i `else`-satsen för att returnera dokumentet som ska sparas i Cosmos DB.

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

1. Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.

1. Klicka på **Spara**. Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.


## <a name="create-a-function-to-list-images-from-cosmos-db"></a>Skapa en funktion för att visa bilderna från Cosmos DB

Webbprogrammet kräver ett API för att hämta bildmetadata från Cosmos DB. I följande steg ska du skapa en HTTP-utlöst funktion som använder en Cosmos DB-indatabindning för att fråga databassamlingen.

1. Hovra över **Funktioner** till vänster i funktionsappen och klicka på **+** för att skapa en ny funktion.

1. Leta upp och välj mallen **HttpTrigger**.

1. Använd dessa värden för att skapa en funktion som genererar en URL för att hämta bilder.

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **Namnge din funktion** | GetImages | Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen. |
    | **Auktorisationsnivå** | Anonym | Gör funktionen offentligt tillgänglig. |

1. Klicka på **Skapa**.

1. När den nya funktionen har skapats klickar du på **Integrera** under funktionens namn i det vänstra navigeringsfönstret.

1. Klicka på **Nya indata** och välj **Azure Cosmos DB**. 

    ![Välj Nya indata](media/functions-first-serverless-web-app/4-new-input.jpg)

1. Klicka på **Välj**.

1. Fyll i följande värden:

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **Dokumentparameternamn** | documents | Matchar parameternamnet i funktionen. |
    | **Databasnamn** | imagesdb |  |
    | **Samlingsnamn** | images |  |
    | **SQL-fråga** | select * from c order by c._ts desc | Hämta dokument, de senaste dokumenten först. |
    | **Azure Cosmos DB-kontoanslutning** | Välj den befintliga anslutningssträngen |  |

1. Skapa indatabindningen genom att klicka på **Spara**.

1. **C#**

    1. Klicka på funktionens namn för att öppna kodfönstret och ersätt allt innehåll i **run.csx** med innehållet i [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).

1. **JavaScript**

    1. Klicka på funktionens namn för att öppna kodfönstret och ersätt allt innehåll i **index.js** med innehållet i [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).

1. Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.

1. Klicka på **Spara**. Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.


## <a name="test-the-application"></a>Testa programmet

1. Öppna programmet i en webbläsare. Välj en bildfil och ladda upp den.

1. Efter några sekunder visas miniatyrbilden för den nya bilden på sidan.

1. Använd sökrutan på Azure Portal för att söka efter ditt Cosmos DB-konto efter namn. Öppna det genom att klicka på det.

1. Klicka på **Datautforskaren** till vänster för att hitta samlingar och dokument.

1. Välj samlingen **images** under databasen **imagesdb**.

1. Kontrollera att ett dokument har skapats för den uppladdade bilden.

    ![Datautforskaren visar ett dokument för en uppladdad bild](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a>Sammanfattning

I den här modulen har du lärt dig hur du skapar ett konto, en databas och en samling i Cosmos DB. Du har också lärt dig hur du använder Cosmos DB-bindningar för att spara och hämta bildmetadata i Cosmos DB-samlingen. I nästa avsnitt lär du dig hur du automatiskt genererar en bildtext för varje uppladdad bild med hjälp av Microsoft Cognitive Services.
