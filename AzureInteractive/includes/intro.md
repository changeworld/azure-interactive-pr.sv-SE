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
ms.openlocfilehash: 0f86f2698a3a0c1e20272c335b63faf03b4b92d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460032"
---
<span data-ttu-id="dbf65-103">I den här självstudien ska du distribuera ett enkelt webbprogram med ett HTML-baserat användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="dbf65-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="dbf65-104">En serverlös funktion gör att programmet kan ladda upp bilder och automatiskt hämta bildtexter som beskriver bilderna.</span><span class="sxs-lookup"><span data-stu-id="dbf65-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Webbapp som körs](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="dbf65-106">I följande diagram visas de Azure-tjänster som används av programmet:</span><span class="sxs-lookup"><span data-stu-id="dbf65-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="dbf65-107">Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.</span><span class="sxs-lookup"><span data-stu-id="dbf65-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="dbf65-108">Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.</span><span class="sxs-lookup"><span data-stu-id="dbf65-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="dbf65-109">Cosmos DB lagrar bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="dbf65-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="dbf65-110">Logic Apps hämtar bildtexter från API:et för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="dbf65-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="dbf65-111">Azure Active Directory hanterar användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="dbf65-111">Azure Active Directory manages user authentication.</span></span>

![Diagram över lösningsarkitektur](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="dbf65-113">I den här guiden får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="dbf65-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="dbf65-114">Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.</span><span class="sxs-lookup"><span data-stu-id="dbf65-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="dbf65-115">Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="dbf65-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="dbf65-116">Ändra storlek på bilder med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="dbf65-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="dbf65-117">Lagra bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dbf65-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="dbf65-118">Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="dbf65-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="dbf65-119">Använd Azure Active Directory för att skydda webbappen genom att autentisera användare.</span><span class="sxs-lookup"><span data-stu-id="dbf65-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>
