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
Med Azure App Service-autentisering kan du använda nyckelfärdig autentisering i en Azure Functions-app. Autentiseringen integreras sömlöst med Facebook, Twitter, Microsoft Account, Google och Azure Active Directory. Du lägger till App Service-autentisering för att skydda serverdels-API:erna för din webbapp.

## <a name="enable-app-service-authentication"></a>Aktivera App Service-autentisering

1. Öppna funktionsappen på Azure Portal.

1. Välj **Autentisering/auktorisering** under **Plattformsfunktioner**.

    ![Välj Autentisering/auktorisering](media/functions-first-serverless-web-app/6-authorization.jpg)

1. Välj följande värden:
    
    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **App Service-autentisering** | På | Aktivera autentisering. |
    | **Åtgärd när begäran inte har autentiserats** | Logga in med Azure Active Directory | Välj en konfigurerad autentiseringsmetod (nedan). |
    | **Autentiseringsprovidrar** | Se nedan | Se nedan |
    | **Tokenarkiv** | På | Tillåt att App Service lagrar och hanterar token. |
    | **Tillåtna externa omdirigeringswebbadresser** | URL:en för ditt program, till exempel: https://firstserverlessweb.z4.web.core.windows.net/ | URL:er som App Service kan omdirigera till när en användare har autentiserats. |

1. Visa **Azure Active Directory-inställningar** genom att välja **Azure Active Directory**.

    1. Välj **Express** som **Hanteringsläge** och fyll i informationen nedan.
    
        | Inställning      |  Föreslaget värde   | Beskrivning                                        |
        | --- | --- | ---|
        | **Hanteringsläge** | Express, Skapa ny AD-app | Konfigurera automatiskt ett huvudnamn för tjänsten och Azure Active Directory-autentisering. |
        | **Skapa app** | my-serverless-webapp | Ange ett unikt programnamn. |
    
    1. Spara Azure Active Directory-inställningarna genom att klicka på **OK**.

    ![Autentisering/auktorisering och Azure Active Directory-inställningar](media/functions-first-serverless-web-app/6-create-aad.png)

1. Klicka på **Spara**.


## <a name="modify-the-web-app-to-enable-authentication"></a>Ändra webbappen för att aktivera autentisering

1. Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Du aktiverar autentisering i funktionsappen genom att lägga till följande kodrad i **settings.js**.

    `window.authEnabled = true`

    Gör ändringen och visa resultatet genom att köra följande kommandon eller genom att använda en kommandoradsredigerare som VIM.

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    Bekräfta att ändringen har gjorts i filen.

    ```azurecli
    cat settings.js
    ```

1. Ladda upp filen till Blob Storage.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a>Testa programmet

1. Öppna programmet i en webbläsare. Klicka på **Logga in** och logga in.

1. Välj en bildfil och ladda upp den.

    ![Inloggningssida](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a>Sammanfattning

I den här modulen har du lärt dig hur du lägger till autentisering i programmet med hjälp av Azure App Service-autentisering.
