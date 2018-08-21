<span data-ttu-id="2f350-101">Azure Blob Storage är en tjänst med extremt hög skalbarhet för lagring av statiska filer till låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="2f350-101">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="2f350-102">I den här självstudien använder du tjänsten för att hantera statiskt innehåll (till exempel HTML, JavaScript, CSS) för en webbapp som du skapar.</span><span class="sxs-lookup"><span data-stu-id="2f350-102">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="2f350-103">Skapa ett Storage-konto</span><span class="sxs-lookup"><span data-stu-id="2f350-103">Create a Storage account</span></span>

<span data-ttu-id="2f350-104">Ett lagringskonto är en Azure-resurs där du kan lagra tabeller, köer, filer, blobbar (objekt) och VM-diskar.</span><span class="sxs-lookup"><span data-stu-id="2f350-104">A Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="2f350-105">Logga in i Cloud Shell (Bash) genom att välja knappen **Enter focus mode** (växla till fokusläge).</span><span class="sxs-lookup"><span data-stu-id="2f350-105">Log in to the Cloud Shell (Bash), by selecting the **Enter focus mode** button.</span></span> <span data-ttu-id="2f350-106">Du hittar knappen längst upp till höger eller längst ned på sidan, beroende på hur stort ditt webbläsarfönster är.</span><span class="sxs-lookup"><span data-stu-id="2f350-106">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="2f350-107">I fokusläge dockas Cloud Shell-fönstret till höger i webbläsarfönstret, så att du enkelt kan köra kommandona som beskrivs i självstudien.</span><span class="sxs-lookup"><span data-stu-id="2f350-107">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="2f350-108">I Azure är en resursgrupp en container med relaterade Azure-resurser som underlättar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="2f350-108">In Azure, a Resource Group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="2f350-109">Skapa en ny resursgrupp med namnet **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="2f350-109">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="2f350-110">Det statiska innehållet (HTML-, CSS- och JavaScript-filer) för den här självstudiekursen finns i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2f350-110">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob Storage.</span></span> <span data-ttu-id="2f350-111">Blob Storage kräver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2f350-111">Blob Storage requires a Storage account.</span></span> <span data-ttu-id="2f350-112">Skapa ett lagringskonto (generell användning V2) i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2f350-112">Create a Storage account (general purpose V2) in the resource group.</span></span> <span data-ttu-id="2f350-113">Ersätt `<storage account name>` med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="2f350-113">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. <span data-ttu-id="2f350-114">Använd sökfältet överst på [Azure Portal](https://portal.azure.com) för att leta upp lagringskontot som du precis skapat och öppna det.</span><span class="sxs-lookup"><span data-stu-id="2f350-114">Using the Search bar at the top of the [Azure portal](https://portal.azure.com), find the storage account you just created and open it.</span></span>

1. <span data-ttu-id="2f350-115">Välj **Statisk webbplats (förhandsversion)** i det vänstra navigeringsfönstret för att konfigurera en container för lagring av statiskt webbplatsinnehåll.</span><span class="sxs-lookup"><span data-stu-id="2f350-115">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="2f350-116">Välj **Aktiverad** för att aktivera Statisk webbplats.</span><span class="sxs-lookup"><span data-stu-id="2f350-116">Select **Enabled** to enable static website.</span></span>
    - <span data-ttu-id="2f350-117">Ange *index.html* som namn på indexdokumentet.</span><span class="sxs-lookup"><span data-stu-id="2f350-117">Enter *index.html* as the index document name.</span></span> <span data-ttu-id="2f350-118">Fältet innehåller redan *index.html* med grå teckenfärg, men detta är endast en exempeltext. Du måste fortfarande ange värdet i fältet.</span><span class="sxs-lookup"><span data-stu-id="2f350-118">The field already has *index.html* in a gray font but this is only example text; you still have to enter that value in the field.</span></span>
    - <span data-ttu-id="2f350-119">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2f350-119">Click **Save**.</span></span>
    
    ![Ange inställningarna för Statisk webbplats](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. <span data-ttu-id="2f350-121">Spara den **primära slutpunkten** på en plats som du enkelt kan kopiera den från när du går igenom självstudien.</span><span class="sxs-lookup"><span data-stu-id="2f350-121">Save the **Primary Endpoint** somewhere you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="2f350-122">Det här är URL:en för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2f350-122">This is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="2f350-123">Ladda upp webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="2f350-123">Upload the web application</span></span>

1. <span data-ttu-id="2f350-124">Källfilerna för programmet som du skapar i den här självstudien finns på en [GitHub-lagringsplats](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="2f350-124">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="2f350-125">Kontrollera att du är i din hemkatalog i Cloud Shell och klona lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2f350-125">Make sure that you are in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="2f350-126">Lagringsplatsen klonas till `/home/<username>/functions-first-serverless-web-application`.</span><span class="sxs-lookup"><span data-stu-id="2f350-126">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="2f350-127">Webbprogrammet på klientsidan finns i mappen **www** och skapas med hjälp av JavaScript-ramverket Vue.js.</span><span class="sxs-lookup"><span data-stu-id="2f350-127">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="2f350-128">Växla till mappen och kör npm-kommandon för att installera programmets beroenden och skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="2f350-128">Change into the folder and run npm commands to install the application's dependencies and build the application.</span></span> <span data-ttu-id="2f350-129">Det sista av dessa kommandon kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="2f350-129">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="2f350-130">Programmet genereras i mappen **dist**.</span><span class="sxs-lookup"><span data-stu-id="2f350-130">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="2f350-131">Ändra den aktuella katalogen till **dist** och ladda upp programmet till blobbcontainern **$web**.</span><span class="sxs-lookup"><span data-stu-id="2f350-131">Change the current directory to **dist** and upload the application to the **$web** Blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="2f350-132">Visa programmet genom att öppna URL:en för den primära slutpunkten för statiska webbplatser i lagringskontot i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2f350-132">Open the Storage static websites primary endpoint URL in a web browser to view the application.</span></span>

    ![Startsidan för den första serverlösa webbappen](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="2f350-134">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2f350-134">Summary</span></span>

<span data-ttu-id="2f350-135">I den här modulen har du skapat en resursgrupp med namnet **first-serverless-app** som innehåller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2f350-135">In this module, you created a resource group named **first-serverless-app** containing a Storage account.</span></span> <span data-ttu-id="2f350-136">En blobbcontainer med namnet **$web** i lagringskontot lagrar webbappens statiska innehåll och gör innehållet offentligt tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="2f350-136">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content available publicly.</span></span> <span data-ttu-id="2f350-137">I nästa avsnitt lär du dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2f350-137">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>
