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
<span data-ttu-id="6836e-103">Azure Cosmos DB är Microsofts serverlösa, globalt distribuerade databas för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="6836e-103">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="6836e-104">I den här modulen lär du dig hur du använder Azure Functions för att lagra och hämta bildmetadata som JSON-dokument i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6836e-104">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Cosmos DB.</span></span>

## <a name="create-a-cosmos-db-account-database-and-collection"></a><span data-ttu-id="6836e-105">Skapa ett konto, en databas och en samling i Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6836e-105">Create a Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="6836e-106">Ett Cosmos DB-konto är en Azure-resurs som innehåller Cosmos DB-databaser.</span><span class="sxs-lookup"><span data-stu-id="6836e-106">A Cosmos DB account is an Azure resource that contains Cosmos DB databases.</span></span>

1. <span data-ttu-id="6836e-107">Kontrollera att du fortfarande är inloggad i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6836e-107">Ensure you are still signed into the Cloud Shell.</span></span>  <span data-ttu-id="6836e-108">Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="6836e-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="6836e-109">Skapa ett Cosmos DB-konto med ett unikt namn i samma resursgrupp som de andra resurserna i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="6836e-109">Create a Cosmos DB account with a unique name in the same resource group as the other resources in this tutorial.</span></span>

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. <span data-ttu-id="6836e-110">När Cosmos DB-kontot har skapats skapar du en ny databas med namnet **imagesdb** i kontot.</span><span class="sxs-lookup"><span data-stu-id="6836e-110">After the Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. <span data-ttu-id="6836e-111">När databasen har skapats skapar du en ny samling med namnet **images** i databasen med ett dataflöde på 400 enheter för programbegäran (RU:er).</span><span class="sxs-lookup"><span data-stu-id="6836e-111">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="6836e-112">Spara ett dokument till Cosmos DB när en miniatyrbild skapas</span><span class="sxs-lookup"><span data-stu-id="6836e-112">Save a document to Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="6836e-113">Med Cosmos DB-utdatabindningen kan du skapa dokument i en Cosmos DB-samling från Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="6836e-113">The Cosmos DB output binding lets you create documents in a Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="6836e-114">I följande steg ska du konfigurera en Cosmos DB-utdatabindning i funktionen **ResizeImage** och ändra funktionen för att returnera ett dokument (objekt) som ska sparas.</span><span class="sxs-lookup"><span data-stu-id="6836e-114">In the following steps, you configure a Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="6836e-115">Öppna funktionsappen på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="6836e-115">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="6836e-116">Expandera funktionen **ResizeImage** i det vänstra navigeringsfönstret och välj **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="6836e-116">In the left hand navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="6836e-117">Klicka på **Nya utdata** under **Utdata**.</span><span class="sxs-lookup"><span data-stu-id="6836e-117">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="6836e-118">Leta upp och markera **Azure Cosmos DB**-objektet.</span><span class="sxs-lookup"><span data-stu-id="6836e-118">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="6836e-119">Klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="6836e-119">Then click **Select**.</span></span>

    ![Välj Nya utdata](media/functions-first-serverless-web-app/4-new-output.jpg)

1. <span data-ttu-id="6836e-121">Fyll i fälten under **Azure Cosmos DB-utdata** med följande värden.</span><span class="sxs-lookup"><span data-stu-id="6836e-121">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="6836e-122">Inställning</span><span class="sxs-lookup"><span data-stu-id="6836e-122">Setting</span></span>      |  <span data-ttu-id="6836e-123">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="6836e-123">Suggested value</span></span>   | <span data-ttu-id="6836e-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6836e-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6836e-125">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="6836e-125">**Document parameter name**</span></span> | <span data-ttu-id="6836e-126">Välj **Använd funktionsreturvärde**</span><span class="sxs-lookup"><span data-stu-id="6836e-126">Select **Use function return value**</span></span> | <span data-ttu-id="6836e-127">Värdet i textrutan ställs automatiskt in till **$return**.</span><span class="sxs-lookup"><span data-stu-id="6836e-127">The value of the textbox is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="6836e-128">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="6836e-128">**Database name**</span></span> | <span data-ttu-id="6836e-129">imagesdb</span><span class="sxs-lookup"><span data-stu-id="6836e-129">imagesdb</span></span> | <span data-ttu-id="6836e-130">Använd namnet på databasen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="6836e-130">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="6836e-131">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="6836e-131">**Collection name**</span></span> | <span data-ttu-id="6836e-132">images</span><span class="sxs-lookup"><span data-stu-id="6836e-132">images</span></span> | <span data-ttu-id="6836e-133">Använd namnet på samlingen som du skapat.</span><span class="sxs-lookup"><span data-stu-id="6836e-133">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="6836e-134">Klicka på **Ny** bredvid **Azure Cosmos DB-kontoanslutning**.</span><span class="sxs-lookup"><span data-stu-id="6836e-134">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="6836e-135">Välj Cosmos DB-kontot som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="6836e-135">Select the Cosmos DB account you previously created.</span></span>

    ![Ange inställningar för Azure Cosmos DB-utdatabindningen](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. <span data-ttu-id="6836e-137">Skapa Cosmos DB-utdatabindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6836e-137">Click **Save** to create the Cosmos DB output binding.</span></span>

1. <span data-ttu-id="6836e-138">Klicka på funktionsnamnet **ResizeImage** till vänster för att öppna funktionen.</span><span class="sxs-lookup"><span data-stu-id="6836e-138">Click on the **ResizeImage** function name on the left to open the function.</span></span>

1. <span data-ttu-id="6836e-139">**C#**</span><span class="sxs-lookup"><span data-stu-id="6836e-139">**C#**</span></span>

    1. <span data-ttu-id="6836e-140">(C#) Ändra returtypen för funktionen från **void** till **object**.</span><span class="sxs-lookup"><span data-stu-id="6836e-140">(C#) Change the return type of the function from **void** to **object**.</span></span>

    1. <span data-ttu-id="6836e-141">(C#) Returnera dokumentet som ska sparas genom att lägga till följande kodblock i slutet av funktionen:</span><span class="sxs-lookup"><span data-stu-id="6836e-141">(C#) At the end of the function, add the following code block to return the document to be saved:</span></span>
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![run.csx för ResizeImages-funktionen efter ändringarna](media/functions-first-serverless-web-app/4-update-function.png)

1. <span data-ttu-id="6836e-143">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6836e-143">**JavaScript**</span></span>

    1. <span data-ttu-id="6836e-144">(JavaScript) Ändra `context.done()`-instruktionen i `else`-satsen för att returnera dokumentet som ska sparas i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6836e-144">(JavaScript) Change the `context.done()` statement in the `else` clause to return the document to be saved to Cosmos DB.</span></span>

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

1. <span data-ttu-id="6836e-145">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="6836e-145">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="6836e-146">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6836e-146">Click **Save**.</span></span> <span data-ttu-id="6836e-147">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="6836e-147">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="create-a-function-to-list-images-from-cosmos-db"></a><span data-ttu-id="6836e-148">Skapa en funktion för att visa bilderna från Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6836e-148">Create a function to list images from Cosmos DB</span></span>

<span data-ttu-id="6836e-149">Webbprogrammet kräver ett API för att hämta bildmetadata från Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6836e-149">The web application requires an API to retrieve image metadata from Cosmos DB.</span></span> <span data-ttu-id="6836e-150">I följande steg</span><span class="sxs-lookup"><span data-stu-id="6836e-150">In the following steps.</span></span> <span data-ttu-id="6836e-151">ska du skapa en HTTP-utlöst funktion som använder en Cosmos DB-indatabindning för att fråga databassamlingen.</span><span class="sxs-lookup"><span data-stu-id="6836e-151">you create an HTTP triggered function that uses a Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="6836e-152">Hovra över **Funktioner** till vänster i funktionsappen och klicka på **+** för att skapa en ny funktion.</span><span class="sxs-lookup"><span data-stu-id="6836e-152">In your function app, hover over **Functions** on the left and click **+** to create a new function.</span></span>

1. <span data-ttu-id="6836e-153">Leta upp och välj mallen **HttpTrigger**.</span><span class="sxs-lookup"><span data-stu-id="6836e-153">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="6836e-154">Använd dessa värden för att skapa en funktion som genererar en URL för att hämta bilder.</span><span class="sxs-lookup"><span data-stu-id="6836e-154">Use these values to create a function that generates a get images URL.</span></span>

    | <span data-ttu-id="6836e-155">Inställning</span><span class="sxs-lookup"><span data-stu-id="6836e-155">Setting</span></span>      |  <span data-ttu-id="6836e-156">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="6836e-156">Suggested value</span></span>   | <span data-ttu-id="6836e-157">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6836e-157">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6836e-158">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="6836e-158">**Name your function**</span></span> | <span data-ttu-id="6836e-159">GetImages</span><span class="sxs-lookup"><span data-stu-id="6836e-159">GetImages</span></span> | <span data-ttu-id="6836e-160">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="6836e-160">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="6836e-161">**Auktorisationsnivå**</span><span class="sxs-lookup"><span data-stu-id="6836e-161">**Authorization level**</span></span> | <span data-ttu-id="6836e-162">Anonym</span><span class="sxs-lookup"><span data-stu-id="6836e-162">Anonymous</span></span> | <span data-ttu-id="6836e-163">Gör funktionen offentligt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6836e-163">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="6836e-164">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6836e-164">Click **Create**.</span></span>

1. <span data-ttu-id="6836e-165">När den nya funktionen har skapats klickar du på **Integrera** under funktionens namn i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="6836e-165">When the new function is created, click **Integrate** under the function's name on the left navigation.</span></span>

1. <span data-ttu-id="6836e-166">Klicka på **Nya indata** och välj **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="6836e-166">Click **New Input** and select **Azure Cosmos DB**.</span></span> 

    ![Välj Nya indata](media/functions-first-serverless-web-app/4-new-input.jpg)

1. <span data-ttu-id="6836e-168">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="6836e-168">Click **Select**.</span></span>

1. <span data-ttu-id="6836e-169">Fyll i följande värden:</span><span class="sxs-lookup"><span data-stu-id="6836e-169">Fill out the following values:</span></span>

    | <span data-ttu-id="6836e-170">Inställning</span><span class="sxs-lookup"><span data-stu-id="6836e-170">Setting</span></span>      |  <span data-ttu-id="6836e-171">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="6836e-171">Suggested value</span></span>   | <span data-ttu-id="6836e-172">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6836e-172">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6836e-173">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="6836e-173">**Document parameter name**</span></span> | <span data-ttu-id="6836e-174">documents</span><span class="sxs-lookup"><span data-stu-id="6836e-174">documents</span></span> | <span data-ttu-id="6836e-175">Matchar parameternamnet i funktionen.</span><span class="sxs-lookup"><span data-stu-id="6836e-175">Matches parameter name in the function.</span></span> |
    | <span data-ttu-id="6836e-176">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="6836e-176">**Database name**</span></span> | <span data-ttu-id="6836e-177">imagesdb</span><span class="sxs-lookup"><span data-stu-id="6836e-177">imagesdb</span></span> |  |
    | <span data-ttu-id="6836e-178">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="6836e-178">**Collection name**</span></span> | <span data-ttu-id="6836e-179">images</span><span class="sxs-lookup"><span data-stu-id="6836e-179">images</span></span> |  |
    | <span data-ttu-id="6836e-180">**SQL-fråga**</span><span class="sxs-lookup"><span data-stu-id="6836e-180">**SQL query**</span></span> | <span data-ttu-id="6836e-181">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="6836e-181">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="6836e-182">Hämta dokument, de senaste dokumenten först.</span><span class="sxs-lookup"><span data-stu-id="6836e-182">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="6836e-183">**Azure Cosmos DB-kontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="6836e-183">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="6836e-184">Välj den befintliga anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="6836e-184">Select existing connection string</span></span> |  |

1. <span data-ttu-id="6836e-185">Skapa indatabindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6836e-185">Click **Save** to create the input binding.</span></span>

1. <span data-ttu-id="6836e-186">**C#**</span><span class="sxs-lookup"><span data-stu-id="6836e-186">**C#**</span></span>

    1. <span data-ttu-id="6836e-187">Klicka på funktionens namn för att öppna kodfönstret och ersätt allt innehåll i **run.csx** med innehållet i [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span><span class="sxs-lookup"><span data-stu-id="6836e-187">Click the function's name to open the code window, and then replace all of **run.csx** with the content in [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span></span>

1. <span data-ttu-id="6836e-188">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6836e-188">**JavaScript**</span></span>

    1. <span data-ttu-id="6836e-189">Klicka på funktionens namn för att öppna kodfönstret och ersätt allt innehåll i **index.js** med innehållet i [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span><span class="sxs-lookup"><span data-stu-id="6836e-189">Click the function's name to open the code window, and then replace all of **index.js** with the content in [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span></span>

1. <span data-ttu-id="6836e-190">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="6836e-190">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="6836e-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6836e-191">Click **Save**.</span></span> <span data-ttu-id="6836e-192">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="6836e-192">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="6836e-193">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="6836e-193">Test the application</span></span>

1. <span data-ttu-id="6836e-194">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6836e-194">Open the application in a browser.</span></span> <span data-ttu-id="6836e-195">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="6836e-195">Select an image file and upload it.</span></span>

1. <span data-ttu-id="6836e-196">Efter några sekunder visas miniatyrbilden för den nya bilden på sidan.</span><span class="sxs-lookup"><span data-stu-id="6836e-196">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="6836e-197">Använd sökrutan på Azure Portal för att söka efter ditt Cosmos DB-konto efter namn.</span><span class="sxs-lookup"><span data-stu-id="6836e-197">In the Azure portal, use the Search box to search for your Cosmos DB account by name.</span></span> <span data-ttu-id="6836e-198">Öppna det genom att klicka på det.</span><span class="sxs-lookup"><span data-stu-id="6836e-198">Click it to open it.</span></span>

1. <span data-ttu-id="6836e-199">Klicka på **Datautforskaren** till vänster för att hitta samlingar och dokument.</span><span class="sxs-lookup"><span data-stu-id="6836e-199">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="6836e-200">Välj samlingen **images** under databasen **imagesdb**.</span><span class="sxs-lookup"><span data-stu-id="6836e-200">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="6836e-201">Kontrollera att ett dokument har skapats för den uppladdade bilden.</span><span class="sxs-lookup"><span data-stu-id="6836e-201">Confirm that a document was created for the uploaded image.</span></span>

    ![Datautforskaren visar ett dokument för en uppladdad bild](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a><span data-ttu-id="6836e-203">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6836e-203">Summary</span></span>

<span data-ttu-id="6836e-204">I den här modulen har du lärt dig hur du skapar ett konto, en databas och en samling i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6836e-204">In this module, you learned how to create a Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="6836e-205">Du har också lärt dig hur du använder Cosmos DB-bindningar för att spara och hämta bildmetadata i Cosmos DB-samlingen.</span><span class="sxs-lookup"><span data-stu-id="6836e-205">You also learned how to use the Cosmos DB bindings to save and retrieve image metadata in the Cosmos DB collection.</span></span> <span data-ttu-id="6836e-206">I nästa avsnitt lär du dig hur du automatiskt genererar en bildtext för varje uppladdad bild med hjälp av Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="6836e-206">Next, you learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>
