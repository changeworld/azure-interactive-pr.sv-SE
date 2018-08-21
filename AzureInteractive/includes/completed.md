---
title: ta med fil
description: ta med fil
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 1c4aaf7afd9fbc54dd34c2cca13a8b74e947c16a
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079700"
---
Du har skapat ett komplett, serverlöst program med hjälp av Azure-tjänster.

## <a name="clean-up-resources"></a>Rensa resurser

När du har arbetat klart med det här programmet kan du använda följande kommando för att ta bort alla resurser som skapades under självstudien:

```azurecli
az group delete --name first-serverless-app
```

Skriv `y` när du uppmanas till detta.  

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:
> [!div class="checklist"]
> * Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.
> * Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.
> * Ändra storlek på bilder med hjälp av Azure Functions.
> * Lagra bildmetadata i Azure Cosmos DB.
> * Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.
> * Använd Azure Active Directory för att skydda webbappen genom att autentisera användare.

Om du vill lära dig hur du ansluter ännu fler tjänster till Functions fortsätter du med självstudien om Logic Apps. 

> [!div class="nextstepaction"]
> [Functions med Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)