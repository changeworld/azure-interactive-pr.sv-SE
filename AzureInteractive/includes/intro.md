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
ms.openlocfilehash: d651fd3d03f2678d625e60f9ab1e9f59e623964f
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079683"
---
I den här självstudien ska du distribuera ett enkelt webbprogram med ett HTML-baserat användargränssnitt. En serverlös funktion gör att programmet kan ladda upp bilder och automatiskt hämta bildtexter som beskriver bilderna.

![Webbapp som körs](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

I följande diagram visas de Azure-tjänster som används av programmet:

1. Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.
2. Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.
3. Cosmos DB lagrar bildmetadata.
4. Logic Apps hämtar bildtexter från API:et för visuellt innehåll.
5. Azure Active Directory hanterar användarautentisering.

![Diagram över lösningsarkitektur](media/functions-first-serverless-web-app/0-architecture.jpg)

I den här guiden får du lära dig att:
> [!div class="checklist"]
> * Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.
> * Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.
> * Ändra storlek på bilder med hjälp av Azure Functions.
> * Lagra bildmetadata i Azure Cosmos DB.
> * Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.
> * Använd Azure Active Directory för att skydda webbappen genom att autentisera användare.