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
ms.openlocfilehash: 426a7287458a48d1bda220ad1a5f067be2ce77d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460049"
---
<span data-ttu-id="5d606-103">Med Azure App Service-autentisering kan du använda nyckelfärdig autentisering i en Azure Functions-app.</span><span class="sxs-lookup"><span data-stu-id="5d606-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="5d606-104">Autentiseringen integreras sömlöst med Facebook, Twitter, Microsoft Account, Google och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d606-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="5d606-105">Du lägger till App Service-autentisering för att skydda serverdels-API:erna för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="5d606-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="5d606-106">Aktivera App Service-autentisering</span><span class="sxs-lookup"><span data-stu-id="5d606-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="5d606-107">Öppna funktionsappen på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5d606-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="5d606-108">Välj **Autentisering/auktorisering** under **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="5d606-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Välj Autentisering/auktorisering](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="5d606-110">Välj följande värden:</span><span class="sxs-lookup"><span data-stu-id="5d606-110">Select the following values:</span></span>
    
    | <span data-ttu-id="5d606-111">Inställning</span><span class="sxs-lookup"><span data-stu-id="5d606-111">Setting</span></span>      |  <span data-ttu-id="5d606-112">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="5d606-112">Suggested value</span></span>   | <span data-ttu-id="5d606-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5d606-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="5d606-114">**App Service-autentisering**</span><span class="sxs-lookup"><span data-stu-id="5d606-114">**App Service Authentication**</span></span> | <span data-ttu-id="5d606-115">På</span><span class="sxs-lookup"><span data-stu-id="5d606-115">On</span></span> | <span data-ttu-id="5d606-116">Aktivera autentisering.</span><span class="sxs-lookup"><span data-stu-id="5d606-116">Enable authentication.</span></span> |
    | <span data-ttu-id="5d606-117">**Åtgärd när begäran inte har autentiserats**</span><span class="sxs-lookup"><span data-stu-id="5d606-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="5d606-118">Logga in med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d606-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="5d606-119">Välj en konfigurerad autentiseringsmetod (nedan).</span><span class="sxs-lookup"><span data-stu-id="5d606-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="5d606-120">**Autentiseringsprovidrar**</span><span class="sxs-lookup"><span data-stu-id="5d606-120">**Authentication Providers**</span></span> | <span data-ttu-id="5d606-121">Se nedan</span><span class="sxs-lookup"><span data-stu-id="5d606-121">See below</span></span> | <span data-ttu-id="5d606-122">Se nedan</span><span class="sxs-lookup"><span data-stu-id="5d606-122">See below</span></span> |
    | <span data-ttu-id="5d606-123">**Tokenarkiv**</span><span class="sxs-lookup"><span data-stu-id="5d606-123">**Token store**</span></span> | <span data-ttu-id="5d606-124">På</span><span class="sxs-lookup"><span data-stu-id="5d606-124">On</span></span> | <span data-ttu-id="5d606-125">Tillåt att App Service lagrar och hanterar token.</span><span class="sxs-lookup"><span data-stu-id="5d606-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="5d606-126">**Tillåtna externa omdirigeringswebbadresser**</span><span class="sxs-lookup"><span data-stu-id="5d606-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="5d606-127">URL:en för ditt program, till exempel: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="5d606-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="5d606-128">URL:er som App Service kan omdirigera till när en användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="5d606-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="5d606-129">Visa **Azure Active Directory-inställningar** genom att välja **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5d606-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="5d606-130">Välj **Express** som **Hanteringsläge** och fyll i informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="5d606-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="5d606-131">Inställning</span><span class="sxs-lookup"><span data-stu-id="5d606-131">Setting</span></span>      |  <span data-ttu-id="5d606-132">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="5d606-132">Suggested value</span></span>   | <span data-ttu-id="5d606-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5d606-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="5d606-134">**Hanteringsläge**</span><span class="sxs-lookup"><span data-stu-id="5d606-134">**Management mode**</span></span> | <span data-ttu-id="5d606-135">Express, Skapa ny AD-app</span><span class="sxs-lookup"><span data-stu-id="5d606-135">Express, Create new AD app</span></span> | <span data-ttu-id="5d606-136">Konfigurera automatiskt ett huvudnamn för tjänsten och Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="5d606-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="5d606-137">**Skapa app**</span><span class="sxs-lookup"><span data-stu-id="5d606-137">**Create app**</span></span> | <span data-ttu-id="5d606-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="5d606-138">my-serverless-webapp</span></span> | <span data-ttu-id="5d606-139">Ange ett unikt programnamn.</span><span class="sxs-lookup"><span data-stu-id="5d606-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="5d606-140">Spara Azure Active Directory-inställningarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d606-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Autentisering/auktorisering och Azure Active Directory-inställningar](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="5d606-142">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5d606-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="5d606-143">Ändra webbappen för att aktivera autentisering</span><span class="sxs-lookup"><span data-stu-id="5d606-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="5d606-144">Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="5d606-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="5d606-145">Du aktiverar autentisering i funktionsappen genom att lägga till följande kodrad i **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="5d606-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="5d606-146">Gör ändringen och visa resultatet genom att köra följande kommandon eller genom att använda en kommandoradsredigerare som VIM.</span><span class="sxs-lookup"><span data-stu-id="5d606-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="5d606-147">Bekräfta att ändringen har gjorts i filen.</span><span class="sxs-lookup"><span data-stu-id="5d606-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="5d606-148">Ladda upp filen till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="5d606-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="5d606-149">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="5d606-149">Test the application</span></span>

1. <span data-ttu-id="5d606-150">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="5d606-150">Open the application in a browser.</span></span> <span data-ttu-id="5d606-151">Klicka på **Logga in** och logga in.</span><span class="sxs-lookup"><span data-stu-id="5d606-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="5d606-152">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="5d606-152">Select an image file and upload it.</span></span>

    ![Inloggningssida](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="5d606-154">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5d606-154">Summary</span></span>

<span data-ttu-id="5d606-155">I den här modulen har du lärt dig hur du lägger till autentisering i programmet med hjälp av Azure App Service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="5d606-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>
