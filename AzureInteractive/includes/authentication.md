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
ms.openlocfilehash: d1f9a07ce3d3b096b498e48b5c4f68c3454b2b37
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079710"
---
<span data-ttu-id="bc81f-103">Med Azure App Service-autentisering kan du använda nyckelfärdig autentisering i en Azure Functions-app.</span><span class="sxs-lookup"><span data-stu-id="bc81f-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="bc81f-104">Autentiseringen integreras sömlöst med Facebook, Twitter, Microsoft Account, Google och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bc81f-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="bc81f-105">Du lägger till App Service-autentisering för att skydda serverdels-API:erna för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="bc81f-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="bc81f-106">Aktivera App Service-autentisering</span><span class="sxs-lookup"><span data-stu-id="bc81f-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="bc81f-107">Öppna funktionsappen på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="bc81f-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="bc81f-108">Välj **Autentisering/auktorisering** under **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="bc81f-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Välj Autentisering/auktorisering](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="bc81f-110">Välj följande värden:</span><span class="sxs-lookup"><span data-stu-id="bc81f-110">Select the following values:</span></span>
    
    | <span data-ttu-id="bc81f-111">Inställning</span><span class="sxs-lookup"><span data-stu-id="bc81f-111">Setting</span></span>      |  <span data-ttu-id="bc81f-112">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="bc81f-112">Suggested value</span></span>   | <span data-ttu-id="bc81f-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc81f-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="bc81f-114">**App Service-autentisering**</span><span class="sxs-lookup"><span data-stu-id="bc81f-114">**App Service Authentication**</span></span> | <span data-ttu-id="bc81f-115">På</span><span class="sxs-lookup"><span data-stu-id="bc81f-115">On</span></span> | <span data-ttu-id="bc81f-116">Aktivera autentisering.</span><span class="sxs-lookup"><span data-stu-id="bc81f-116">Enable authentication.</span></span> |
    | <span data-ttu-id="bc81f-117">**Åtgärd när begäran inte har autentiserats**</span><span class="sxs-lookup"><span data-stu-id="bc81f-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="bc81f-118">Logga in med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc81f-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="bc81f-119">Välj en konfigurerad autentiseringsmetod (nedan).</span><span class="sxs-lookup"><span data-stu-id="bc81f-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="bc81f-120">**Autentiseringsprovidrar**</span><span class="sxs-lookup"><span data-stu-id="bc81f-120">**Authentication Providers**</span></span> | <span data-ttu-id="bc81f-121">Se nedan</span><span class="sxs-lookup"><span data-stu-id="bc81f-121">See below</span></span> | <span data-ttu-id="bc81f-122">Se nedan</span><span class="sxs-lookup"><span data-stu-id="bc81f-122">See below</span></span> |
    | <span data-ttu-id="bc81f-123">**Tokenarkiv**</span><span class="sxs-lookup"><span data-stu-id="bc81f-123">**Token store**</span></span> | <span data-ttu-id="bc81f-124">På</span><span class="sxs-lookup"><span data-stu-id="bc81f-124">On</span></span> | <span data-ttu-id="bc81f-125">Tillåt att App Service lagrar och hanterar token.</span><span class="sxs-lookup"><span data-stu-id="bc81f-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="bc81f-126">**Tillåtna externa omdirigeringswebbadresser**</span><span class="sxs-lookup"><span data-stu-id="bc81f-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="bc81f-127">URL:en för ditt program, till exempel: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="bc81f-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="bc81f-128">URL:er som App Service kan omdirigera till när en användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="bc81f-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="bc81f-129">Visa **Azure Active Directory-inställningar** genom att välja **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bc81f-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="bc81f-130">Välj **Express** som **Hanteringsläge** och fyll i informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="bc81f-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="bc81f-131">Inställning</span><span class="sxs-lookup"><span data-stu-id="bc81f-131">Setting</span></span>      |  <span data-ttu-id="bc81f-132">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="bc81f-132">Suggested value</span></span>   | <span data-ttu-id="bc81f-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc81f-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="bc81f-134">**Hanteringsläge**</span><span class="sxs-lookup"><span data-stu-id="bc81f-134">**Management mode**</span></span> | <span data-ttu-id="bc81f-135">Express, Skapa ny AD-app</span><span class="sxs-lookup"><span data-stu-id="bc81f-135">Express, Create new AD app</span></span> | <span data-ttu-id="bc81f-136">Konfigurera automatiskt ett huvudnamn för tjänsten och Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="bc81f-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="bc81f-137">**Skapa app**</span><span class="sxs-lookup"><span data-stu-id="bc81f-137">**Create app**</span></span> | <span data-ttu-id="bc81f-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="bc81f-138">my-serverless-webapp</span></span> | <span data-ttu-id="bc81f-139">Ange ett unikt programnamn.</span><span class="sxs-lookup"><span data-stu-id="bc81f-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="bc81f-140">Spara Azure Active Directory-inställningarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc81f-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Autentisering/auktorisering och Azure Active Directory-inställningar](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="bc81f-142">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bc81f-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="bc81f-143">Ändra webbappen för att aktivera autentisering</span><span class="sxs-lookup"><span data-stu-id="bc81f-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="bc81f-144">Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="bc81f-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="bc81f-145">Du aktiverar autentisering i funktionsappen genom att lägga till följande kodrad i **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="bc81f-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="bc81f-146">Gör ändringen och visa resultatet genom att köra följande kommandon eller genom att använda en kommandoradsredigerare som VIM.</span><span class="sxs-lookup"><span data-stu-id="bc81f-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="bc81f-147">Bekräfta att ändringen har gjorts i filen.</span><span class="sxs-lookup"><span data-stu-id="bc81f-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="bc81f-148">Ladda upp filen till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="bc81f-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="bc81f-149">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="bc81f-149">Test the application</span></span>

1. <span data-ttu-id="bc81f-150">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bc81f-150">Open the application in a browser.</span></span> <span data-ttu-id="bc81f-151">Klicka på **Logga in** och logga in.</span><span class="sxs-lookup"><span data-stu-id="bc81f-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="bc81f-152">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="bc81f-152">Select an image file and upload it.</span></span>

    ![Inloggningssida](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="bc81f-154">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bc81f-154">Summary</span></span>

<span data-ttu-id="bc81f-155">I den här modulen har du lärt dig hur du lägger till autentisering i programmet med hjälp av Azure App Service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="bc81f-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>
