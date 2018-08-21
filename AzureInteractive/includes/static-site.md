Azure Blob Storage är en tjänst med extremt hög skalbarhet för lagring av statiska filer till låg kostnad. I den här självstudien använder du tjänsten för att hantera statiskt innehåll (till exempel HTML, JavaScript, CSS) för en webbapp som du skapar.

## <a name="create-a-storage-account"></a>Skapa ett Storage-konto

Ett lagringskonto är en Azure-resurs där du kan lagra tabeller, köer, filer, blobbar (objekt) och VM-diskar.

1. Logga in i Cloud Shell (Bash) genom att välja knappen **Enter focus mode** (växla till fokusläge). Du hittar knappen längst upp till höger eller längst ned på sidan, beroende på hur stort ditt webbläsarfönster är. I fokusläge dockas Cloud Shell-fönstret till höger i webbläsarfönstret, så att du enkelt kan köra kommandona som beskrivs i självstudien.

1. I Azure är en resursgrupp en container med relaterade Azure-resurser som underlättar hanteringen. Skapa en ny resursgrupp med namnet **first-serverless-app**.

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. Det statiska innehållet (HTML-, CSS- och JavaScript-filer) för den här självstudiekursen finns i Blob Storage. Blob Storage kräver ett lagringskonto. Skapa ett lagringskonto (generell användning V2) i resursgruppen. Ersätt `<storage account name>` med ett unikt namn.

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. Använd sökfältet överst på [Azure Portal](https://portal.azure.com) för att leta upp lagringskontot som du precis skapat och öppna det.

1. Välj **Statisk webbplats (förhandsversion)** i det vänstra navigeringsfönstret för att konfigurera en container för lagring av statiskt webbplatsinnehåll.
    - Välj **Aktiverad** för att aktivera Statisk webbplats.
    - Ange *index.html* som namn på indexdokumentet. Fältet innehåller redan *index.html* med grå teckenfärg, men detta är endast en exempeltext. Du måste fortfarande ange värdet i fältet.
    - Klicka på **Spara**.
    
    ![Ange inställningarna för Statisk webbplats](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. Spara den **primära slutpunkten** på en plats som du enkelt kan kopiera den från när du går igenom självstudien. Det här är URL:en för ditt webbprogram.

## <a name="upload-the-web-application"></a>Ladda upp webbprogrammet

1. Källfilerna för programmet som du skapar i den här självstudien finns på en [GitHub-lagringsplats](https://github.com/Azure-Samples/functions-first-serverless-web-application). Kontrollera att du är i din hemkatalog i Cloud Shell och klona lagringsplatsen.

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    Lagringsplatsen klonas till `/home/<username>/functions-first-serverless-web-application`.

1. Webbprogrammet på klientsidan finns i mappen **www** och skapas med hjälp av JavaScript-ramverket Vue.js. Växla till mappen och kör npm-kommandon för att installera programmets beroenden och skapa programmet. Det sista av dessa kommandon kan ta flera minuter att slutföra.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    Programmet genereras i mappen **dist**.

1. Ändra den aktuella katalogen till **dist** och ladda upp programmet till blobbcontainern **$web**.

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. Visa programmet genom att öppna URL:en för den primära slutpunkten för statiska webbplatser i lagringskontot i en webbläsare.

    ![Startsidan för den första serverlösa webbappen](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat en resursgrupp med namnet **first-serverless-app** som innehåller ett lagringskonto. En blobbcontainer med namnet **$web** i lagringskontot lagrar webbappens statiska innehåll och gör innehållet offentligt tillgängligt. I nästa avsnitt lär du dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från det här webbprogrammet.
