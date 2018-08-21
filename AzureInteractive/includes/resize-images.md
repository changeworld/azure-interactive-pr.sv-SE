---
title: ta med fil
description: ta med fil
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 88b0ac838dfa8e097a30cc6cef591377e4a95ad8
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079600"
---
<span data-ttu-id="b4f92-103">I den föregående modulen lärde du dig hur du kan använda en serverlös funktion för att på ett säkert sätt ladda upp bilder till Blob Storage från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b4f92-103">In the previous module, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="b4f92-104">I den här modulen ska du skapa en annan serverlös funktion för att bevaka uppladdade bilder och skapa miniatyrbilder från dem.</span><span class="sxs-lookup"><span data-stu-id="b4f92-104">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="b4f92-105">Skapa en Blob Storage-container för miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="b4f92-105">Create a blob storage container for thumbnails</span></span>

<span data-ttu-id="b4f92-106">Bilderna i fullstorlek lagras i en container med namnet **images**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-106">The full size images are stored in a container named **images**.</span></span> <span data-ttu-id="b4f92-107">Du behöver en annan container för att lagra miniatyrbilderna för dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="b4f92-107">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="b4f92-108">Kontrollera att du fortfarande är inloggad i Cloud Shell (bash).</span><span class="sxs-lookup"><span data-stu-id="b4f92-108">Ensure you are still signed in to the Cloud Shell (bash).</span></span>  <span data-ttu-id="b4f92-109">Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="b4f92-109">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="b4f92-110">Skapa en ny container med namnet **thumbnails** (bilder) i ditt lagringskonto med offentlig åtkomst till alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="b4f92-110">Create a new container named **thumbnails** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="b4f92-111">Skapa en blobbutlöst serverlös funktion</span><span class="sxs-lookup"><span data-stu-id="b4f92-111">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="b4f92-112">En utlösare definierar hur en funktion anropas.</span><span class="sxs-lookup"><span data-stu-id="b4f92-112">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="b4f92-113">Funktionen som du ska skapa nu använder en blobbutlösare: funktionen anropas automatiskt när en blobb (bildfil) laddas upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-113">The function you create next uses a blob trigger: the function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="b4f92-114">En funktion måste ha en utlösare.</span><span class="sxs-lookup"><span data-stu-id="b4f92-114">A function must have one trigger.</span></span> <span data-ttu-id="b4f92-115">Utlösare har associerade data, vilket vanligtvis är nyttolasten som utlöste funktionen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-115">Triggers have associated data, which is usually the payload that triggered the function.</span></span>

<span data-ttu-id="b4f92-116">Bindningar definierar hur en funktion läser eller skriver data i Azure eller tredjepartstjänster.</span><span class="sxs-lookup"><span data-stu-id="b4f92-116">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="b4f92-117">Den här funktionen skapar en miniatyrbild för bilden som utlöser den och sparar miniatyrbilden i containern *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="b4f92-117">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="b4f92-118">Öppna funktionsappen på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b4f92-118">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="b4f92-119">Hovra över **Funktioner** i det vänstra navigeringsfönstret i funktionsappens fönster och klicka på **+** för att börja skapa en ny serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="b4f92-119">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span> <span data-ttu-id="b4f92-120">Om en snabbstartssida visas klickar du på **Anpassad funktion** för att visa en lista med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="b4f92-120">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="b4f92-121">Leta upp och välj mallen **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-121">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="b4f92-122">Använd dessa värden för att skapa en funktion som genererar miniatyrbilder när bilder laddas upp.</span><span class="sxs-lookup"><span data-stu-id="b4f92-122">Use these values to create a function that creates thumbnails as images are uploaded.</span></span>

    | <span data-ttu-id="b4f92-123">Inställning</span><span class="sxs-lookup"><span data-stu-id="b4f92-123">Setting</span></span>      |  <span data-ttu-id="b4f92-124">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b4f92-124">Suggested value</span></span>   | <span data-ttu-id="b4f92-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f92-125">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="b4f92-126">**Språk**</span><span class="sxs-lookup"><span data-stu-id="b4f92-126">**Language**</span></span> | <span data-ttu-id="b4f92-127">C# eller JavaScript</span><span class="sxs-lookup"><span data-stu-id="b4f92-127">C# or JavaScript</span></span> | <span data-ttu-id="b4f92-128">Välj det språk du föredrar.</span><span class="sxs-lookup"><span data-stu-id="b4f92-128">Choose your preferred language.</span></span> |
    | <span data-ttu-id="b4f92-129">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="b4f92-129">**Name your function**</span></span> | <span data-ttu-id="b4f92-130">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="b4f92-130">ResizeImage</span></span> | <span data-ttu-id="b4f92-131">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-131">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="b4f92-132">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="b4f92-132">**Path**</span></span> | <span data-ttu-id="b4f92-133">images/{name}</span><span class="sxs-lookup"><span data-stu-id="b4f92-133">images/{name}</span></span> | <span data-ttu-id="b4f92-134">Kör funktionen när en fil läggs till i containern **images**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-134">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="b4f92-135">**Lagringskontoinformation**</span><span class="sxs-lookup"><span data-stu-id="b4f92-135">**Storage account information**</span></span> | <span data-ttu-id="b4f92-136">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="b4f92-136">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="b4f92-137">Använd miljövariabeln som skapades tidigare med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-137">Use the environment variable previously created with the connection string.</span></span> |

    ![Ange inställningarna för den nya funktionen](media/functions-first-serverless-web-app/3-new-function.png)

1. <span data-ttu-id="b4f92-139">Skapa funktionen genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-139">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="b4f92-140">När funktionen har skapats klickar du på **Integrera** för att visa funktionens utlösare, indata och utdatabindningar.</span><span class="sxs-lookup"><span data-stu-id="b4f92-140">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="b4f92-141">Klicka på **Nya utdata** för att skapa en ny utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="b4f92-141">Click **New output** to create a new output binding.</span></span>

    ![Välj Nya utdata på fliken Integrera](media/functions-first-serverless-web-app/3-new-output.jpg)

1. <span data-ttu-id="b4f92-143">Välj **Azure Blob Storage** och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-143">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="b4f92-144">Du kan behöva rulla ned för att se knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-144">You may have to scroll down to see the **Select** button.</span></span>

    ![Välj Azure Blob Storage](media/functions-first-serverless-web-app/3-storage-output.jpg)

1. <span data-ttu-id="b4f92-146">Ange följande värden.</span><span class="sxs-lookup"><span data-stu-id="b4f92-146">Enter the following values.</span></span>

    | <span data-ttu-id="b4f92-147">Inställning</span><span class="sxs-lookup"><span data-stu-id="b4f92-147">Setting</span></span>      |  <span data-ttu-id="b4f92-148">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b4f92-148">Suggested value</span></span>   | <span data-ttu-id="b4f92-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f92-149">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="b4f92-150">**Blobbparameternamn**</span><span class="sxs-lookup"><span data-stu-id="b4f92-150">**Blob parameter name**</span></span> | <span data-ttu-id="b4f92-151">thumbnail</span><span class="sxs-lookup"><span data-stu-id="b4f92-151">thumbnail</span></span> | <span data-ttu-id="b4f92-152">Funktionen använder parametern med det här namnet för att skriva miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="b4f92-152">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="b4f92-153">**Använd funktionsreturvärde**</span><span class="sxs-lookup"><span data-stu-id="b4f92-153">**Use function return value**</span></span> | <span data-ttu-id="b4f92-154">Nej</span><span class="sxs-lookup"><span data-stu-id="b4f92-154">No</span></span> |  |
    | <span data-ttu-id="b4f92-155">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="b4f92-155">**Path**</span></span> | <span data-ttu-id="b4f92-156">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="b4f92-156">thumbnails/{name}</span></span> | <span data-ttu-id="b4f92-157">Miniatyrbilderna skrivs till en container med namnet **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-157">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="b4f92-158">**Lagringskontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="b4f92-158">**Storage account connection**</span></span> | <span data-ttu-id="b4f92-159">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="b4f92-159">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="b4f92-160">Använd miljövariabeln som skapades tidigare med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-160">Use the environment variable previously created with the connection string.</span></span> |

    ![Ange inställningarna för blobbutdatabindningen](media/functions-first-serverless-web-app/3-blob-output.png)

1. <span data-ttu-id="b4f92-162">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="b4f92-162">**JavaScript**</span></span>

    1. <span data-ttu-id="b4f92-163">(JavaScript) Klicka på **Avancerad redigerare** i det övre högra hörnet i fönstret för att visa den JSON som representerar bindningarna.</span><span class="sxs-lookup"><span data-stu-id="b4f92-163">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON representing the bindings.</span></span>

    1. <span data-ttu-id="b4f92-164">(JavaScript) Lägg till en egenskap med namnet `dataType` med värdet `binary` i `blobTrigger`-bindningen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-164">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="b4f92-165">När du gör det konfigureras bindningen så att blobbinnehållet överförs till funktionen som binära data.</span><span class="sxs-lookup"><span data-stu-id="b4f92-165">This configures the binding to pass the blob contents to the function as binary data.</span></span>

    ```json
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AZURE_STORAGE_CONNECTION_STRING",
      "dataType": "binary"
    }
    ```

1. <span data-ttu-id="b4f92-166">Skapa den nya bindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-166">Click **Save** to create the new binding.</span></span>

1. <span data-ttu-id="b4f92-167">**C#**</span><span class="sxs-lookup"><span data-stu-id="b4f92-167">**C#**</span></span>

    1. <span data-ttu-id="b4f92-168">(C#) Välj funktionsnamnet **ResizeImage** i det vänstra navigeringsfönstret för att öppna källkoden för funktionen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-168">(C#) Select the **ResizeImage** function name on the left navigation to open the function's source code.</span></span>

    1. <span data-ttu-id="b4f92-169">(C#) Funktionen kräver ett NuGet-paket med namnet **ImageResizer** för att generera miniatyrbilderna.</span><span class="sxs-lookup"><span data-stu-id="b4f92-169">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="b4f92-170">NuGet-paket läggs till i C#-funktioner med hjälp av en **project.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="b4f92-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="b4f92-171">Skapa filen genom att först klicka på **Visa filer** till höger för att visa filerna som funktionen består av.</span><span class="sxs-lookup"><span data-stu-id="b4f92-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>
    
    1. <span data-ttu-id="b4f92-172">(C#) Klicka på **Lägg till** för att lägga till en ny fil med namnet **project.json**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-172">(C#) Click **Add** to add a new file named **project.json**.</span></span>
    
    1. <span data-ttu-id="b4f92-173">(C#) Kopiera innehållet i [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) till den nya filen som skapats.</span><span class="sxs-lookup"><span data-stu-id="b4f92-173">(C#) Copy the contents of [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) into the newly created file.</span></span> <span data-ttu-id="b4f92-174">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-174">Save the file.</span></span> <span data-ttu-id="b4f92-175">Paket återställs automatiskt när filen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="b4f92-175">Packages are automatically restored when the file is updated.</span></span>
    
        ![project.json-fil med ImageResizer](media/functions-first-serverless-web-app/3-project-json.png)
    
    1. <span data-ttu-id="b4f92-177">(C#) Välj **run.csx** under **Visa filer** och ersätt dess innehåll med innehållet i [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="b4f92-177">(C#) Select **run.csx** under **View Files** and replace its content with the content in [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span></span>

1. <span data-ttu-id="b4f92-178">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="b4f92-178">**JavaScript**</span></span> 

    1. <span data-ttu-id="b4f92-179">(JavaScript) Den här funktionen kräver `jimp`-paketet från npm för att ändra storlek på fotot.</span><span class="sxs-lookup"><span data-stu-id="b4f92-179">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="b4f92-180">Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-180">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="b4f92-181">(JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-181">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="b4f92-182">(JavaScript) Kör kommandot `npm install jimp` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="b4f92-182">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="b4f92-183">Åtgärden kan ta en eller ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="b4f92-183">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="b4f92-184">(JavaScript) Klicka på **ResizeImage**-funktionsnamnet i det vänstra navigeringsfönstret för att visa funktionen och ersätt allt innehåll i **index.js** med innehållet i [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="b4f92-184">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function, replace all of **index.js** with the content of [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span></span>

1. <span data-ttu-id="b4f92-185">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="b4f92-185">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="b4f92-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-186">Click **Save**.</span></span> <span data-ttu-id="b4f92-187">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="b4f92-187">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="b4f92-188">Testa den serverlösa funktionen</span><span class="sxs-lookup"><span data-stu-id="b4f92-188">Test the serverless function</span></span>

1. <span data-ttu-id="b4f92-189">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b4f92-189">Open the application in a browser.</span></span> <span data-ttu-id="b4f92-190">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="b4f92-190">Select an image file and upload it.</span></span> <span data-ttu-id="b4f92-191">Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot.</span><span class="sxs-lookup"><span data-stu-id="b4f92-191">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="b4f92-192">Bekräfta i Cloud Shell att bilden har laddats upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-192">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="b4f92-193">Kontrollera att miniatyrbilden har skapats i en container med namnet **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="b4f92-193">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="b4f92-194">Hämta miniatyrbildens URL.</span><span class="sxs-lookup"><span data-stu-id="b4f92-194">Obtain the thumbnail's URL.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="b4f92-195">Öppna URL:en i en webbläsare och bekräfta att miniatyrbilden har skapats korrekt.</span><span class="sxs-lookup"><span data-stu-id="b4f92-195">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="b4f92-196">Ta bort alla filer i containern **images** och **thumbnails** innan du går vidare till nästa självstudie.</span><span class="sxs-lookup"><span data-stu-id="b4f92-196">Before moving on to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="b4f92-197">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b4f92-197">Summary</span></span>

<span data-ttu-id="b4f92-198">I den här modulen har du skapat en serverlös funktion som genererar en miniatyrbild varje gång en bild laddas upp till en Blob Storage-container.</span><span class="sxs-lookup"><span data-stu-id="b4f92-198">In this module, you created a serverless function to create a thumbnail whenever an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="b4f92-199">I nästa avsnitt lär du dig hur du använder Azure Cosmos DB för att lagra och visa bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="b4f92-199">Next, you learn how to use Azure Cosmos DB to store and list image metadata.</span></span>
