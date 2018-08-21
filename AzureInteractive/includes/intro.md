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
<span data-ttu-id="007fe-103">I den här självstudien ska du distribuera ett enkelt webbprogram med ett HTML-baserat användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="007fe-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="007fe-104">En serverlös funktion gör att programmet kan ladda upp bilder och automatiskt hämta bildtexter som beskriver bilderna.</span><span class="sxs-lookup"><span data-stu-id="007fe-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Webbapp som körs](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="007fe-106">I följande diagram visas de Azure-tjänster som används av programmet:</span><span class="sxs-lookup"><span data-stu-id="007fe-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="007fe-107">Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.</span><span class="sxs-lookup"><span data-stu-id="007fe-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="007fe-108">Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.</span><span class="sxs-lookup"><span data-stu-id="007fe-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="007fe-109">Cosmos DB lagrar bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="007fe-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="007fe-110">Logic Apps hämtar bildtexter från API:et för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="007fe-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="007fe-111">Azure Active Directory hanterar användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="007fe-111">Azure Active Directory manages user authentication.</span></span>

![Diagram över lösningsarkitektur](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="007fe-113">I den här guiden får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="007fe-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="007fe-114">Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.</span><span class="sxs-lookup"><span data-stu-id="007fe-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="007fe-115">Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="007fe-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="007fe-116">Ändra storlek på bilder med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="007fe-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="007fe-117">Lagra bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="007fe-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="007fe-118">Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="007fe-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="007fe-119">Använd Azure Active Directory för att skydda webbappen genom att autentisera användare.</span><span class="sxs-lookup"><span data-stu-id="007fe-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>