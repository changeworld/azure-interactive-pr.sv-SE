---
title: ta med fil
description: ta med fil
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a66fcc3a406c79fcf9881ddaaaf8330f5b373043
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460013"
---
<span data-ttu-id="9dfa8-103">Du har skapat ett komplett, serverlöst program med hjälp av Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="9dfa8-104">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="9dfa8-104">Clean up resources</span></span>

<span data-ttu-id="9dfa8-105">När du har arbetat klart med det här programmet kan du använda följande kommando för att ta bort alla resurser som skapades under självstudien:</span><span class="sxs-lookup"><span data-stu-id="9dfa8-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="9dfa8-106">Skriv `y` när du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="9dfa8-107">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9dfa8-107">Next steps</span></span>

<span data-ttu-id="9dfa8-108">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="9dfa8-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="9dfa8-109">Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="9dfa8-110">Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="9dfa8-111">Ändra storlek på bilder med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="9dfa8-112">Lagra bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="9dfa8-113">Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="9dfa8-114">Använd Azure Active Directory för att skydda webbappen genom att autentisera användare.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="9dfa8-115">Om du vill lära dig hur du ansluter ännu fler tjänster till Functions fortsätter du med självstudien om Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="9dfa8-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="9dfa8-116">Functions med Logic Apps</span><span class="sxs-lookup"><span data-stu-id="9dfa8-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
