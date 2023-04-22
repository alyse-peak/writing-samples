# Moving an API Endpoint to an Azure Function

## Introduction

In this lab, students will perform an API code migration. To begin, you will deploy an Azure Functions instance before deploying and configuring the Azure Function App. You will also make the necessary API code modifications before copying the code into the new Azure Function App.

## Solution

Log in to the Azure portal using the credentials provided for the lab.

### Deploy and Configure Azure Functions

#### Deploy the Function App and Configure Cloud Shell

 1. Navigate to **Function App** using the **Azure services** menu or the top menu search bar.
 1. Click **Create Function App**.
 1. Fill in the basic app details:
    * **Resource Group**: Use the dropdown to select the provided resource group.
    * **Function App name**: In the text field, enter a unique name for the app.
    * **Publish**: Ensure **Code** is selected.
    * **Runtime stack**: Use the dropdown to select **Node.js**.
    * **Version**: Ensure **16 LTS** is selected.
    * **Region**: Leave the default value.
    * **Operating System**: Ensure **Windows** is selected.
    * **Plan type**: Ensure **Consumption (Serverless)** is selected.
 1. Click **Review + create**.
 1. Review your deployment details and then click **Create**.
 1. While your deployment is in progress, click the **Cloud Shell** icon (`>_`) along the top menu bar to open Cloud Shell.
 1. On the **Welcome** screen, select **Bash**.
 1. On the storage screen, select **Show advanced settings**.
 1. Fill in the storage details:
    * **Storage account**: In the text field, enter a unique name (e.g., *cloudshellstorage1948*).
    * **File share**: In the text field, enter *cloudshell*.
 1. Click **Create storage**.

    Your Cloud Shell environment is provisioned. At this point, your Function App deployment should be complete.

#### Configure the Function App

 1. On your Function App page, click **Go to resource**.
 1. In the sidebar menu, navigate to **Settings** and select **Configuration**.
 1. Create the first variable:
    * In the main pane, click **New application setting**.
    * In the **Add/Edit application setting** window, fill in the variable details:
      * **Name**: In the text field, enter **DB_HOST**.
      * **Value**: In the text field, enter `<YOUR-FUNCTION-APP-NAME>.mysql.database.azure.com`, replacing `<YOUR-FUNCTION-APP-NAME>` with your Function App name.

      This is the endpoint hostname your MySQL database has in Azure.
    * Click **OK**.
 1. Create the second variable:
    * Click **New application setting** again.
    * In the **Add/Edit application settings** window, fill in the variable details:
      * **Name**: In the text field, enter *DB_USER*.
      * **Value**: In the text field, enter *treefarm*.
    * Click **OK**.
 1. Create the third variable:
    * Click **New application setting** again.
    * In the **Add/Edit application settings** window, fill in the variable details:
      * **Name**: In the text field, enter *DB_PASS*.
      * **Value**: In the text field, enter *treefarm*.
    * Click **OK**.
 1. Along the top of the **Configuration** page, click **Save** and then click **Continue**.

    Your changes to the settings will restart your application.
 1. In the sidebar menu, navigate to **Functions** and select **Functions**.
 1. In the main pane, click **Create**.
 1. In the **Create function** window, fill in the function details:
    * **Development environment**: Ensure that **Develop in portal** is selected.
    * **Select a template**: Select **HTTP trigger**.
    * **New Function**: In the text field, replace the existing function name with *treefarmAPI*.
    * **Authorization level**: Ensure **Function** is selected.
 1. Click **Create**.

    The function is created. After it is successfully created, the function details open.

### Access the "On-Premises" API Code

 1. In the function's sidebar menu, select **Code + Test**.
 1. Navigate back to your provisioned Cloud Shell terminal (you may need to select the **Cloud Shell** icon (`>_`) along the top menu bar again).
 1. Clone the [GitHub repo](https://github.com/ACloudGuru-Resources/content-move-application-cloud-azure.git) provided for the lab:
    ```
    git clone https://github.com/ACloudGuru-Resources/content-move-application-cloud-azure.git
    ```
 1. Change directories into the `api/azfunction` directory:
    ```
    cd content-move-application-cloud-azure/
    cd api
    cd azfunction/
    ```
 1. Display the contents of `index.js`:
    ```
    cat index.js
    ```

    This shows the modifications needed to make the "on-premises" API code compatible with your Azure function.
 1. Copy the command's output and then minimize Cloud Shell.

### Migrate the API Code to the Azure Functions App

 1. From your function's **Code + Test** page, replace the existing code (starting with `context.log` and ending with `body: responseMessage`) with your copied code.
 1. Click **Save**.

## Conclusion

Congratulations — you've completed this hands-on lab!
