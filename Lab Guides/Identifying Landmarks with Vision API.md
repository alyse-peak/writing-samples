# Identifying Landmarks with Vision API

## Introduction

Google Cloud Vision API is capable of recognizing a range of different types of objects in images, such as faces, text, and even handwriting. In this hands-on lab, you’ll use the API to detect and identify landmarks from around the globe in both a Cloud Storage bucket and via a remote URL.

## Solution

Log in to the GCP console using the credentials provided for the lab.

On the **Welcome to your new account** screen, review the text and click **Accept**. In the **Welcome Cloud Student** pop-up, check to agree to the terms of service, choose your country of residence, and click **AGREE AND CONTINUE**.

### Enable the API

 1. From the GCP console, use the sidebar menu to navigate to **APIs & Services** > **Library**.
 1. Use the API library search bar to search for **Cloud Vision API**.
 1. Select **Cloud Vision API**.
 1. Click **ENABLE** to enable the API.
    >**Note**: After the API is enabled, you might see a warning message indicating you may need credentials to use the API. You can ignore this message, as you won't need credentials for this lab.

### Create the Required Cloud Storage Bucket

 1. Navigate back to the GCP console's homepage.
 1. Use the sidebar menu (click the hamburger menu icon in the top left corner if needed) to navigate to **Cloud Storage** > **Browser**.
 1. Click **CREATE BUCKET**.
 1. In the **Name your bucket** section, enter a globally unique bucket name (e.g., *acg-vision-123*).
 1. Click **CONTINUE**.
 1. Fill in the **Choose where to store your data** section:
    * **Location type**: Region
    * **Region**: us-east1
 1. Click **CONTINUE**.
 1. Leave the other sections as the defaults and click **CREATE**.
      >**Note**: You will receive a warning that public access will be prevented; click  **CONFIRM**.

    Your bucket is created and should be ready to receive data.
 1. Note your bucket name so you can use it later.

### Retrieve Files from the Repo and Copy Them to the Bucket

 1. Click the **Activate Cloud Shell** icon (`>_`) to the right of the top menu bar.
 1. When prompted, click **CONTINUE**.
 1. In the Cloud Shell terminal, clone the GitHub repository provided for the lab:
    ```
    git clone https://github.com/linuxacademy/content-google-cloud-certified-cloud-digital-leader
    ```
 1. List the contents of your directory to verify that the GitHub repo was successfully cloned:
    ```
    ls
    ```

    You should see the `content-google-cloud-certified-cloud-digital-leader` directory listed.
 1. Change into your cloned directory:
    ```
    cd content-google-cloud-certified-cloud-digital-leader
    ```
 1. List the contents of this directory:
    ```
    ls
    ```

    You should see the `vision-api-lab` directory listed.
 1. Change into the `vision-api-lab` directory:
    ```
    cd vision-api-lab
    ```
 1. List the contents of this directory:
    ```
    ls
    ```

    You should see three JPEG files listed.
 1. Copy the image files to your Cloud Storage bucket, replacing `<BUCKET_NAME>` with your own bucket name:
    ```
    gsutil cp *.jpeg gs://<BUCKET_NAME>
    ```
 1. If prompted, click **AUTHORIZE** to authorize API calls.
 1. After the files are successfully copied in the Cloud Shell terminal, click **REFRESH** in the top right of the GCP console to refresh your bucket details.

    You should now see the copied images listed in your bucket.

    >**Note:** You will see and identify **different images** from what is shown in the video.

### Call Vision API

 1. Identify the landmark in **image01.jpeg** using the image file name:
    * Select the **image01.jpeg** file name to preview the image.
    * In the Cloud Shell terminal, call the Vision API using the image name:
      ```
      gcloud ml vision detect-landmarks image01.jpeg
      ```

      The request returns a response in JSON format with the landmark details.
 1. Identify the landmark in **image02.jpeg** using a remote URL:
    * In the GCP console, return to your bucket and select the **image02.jpeg** file name.
    * If needed, click **PREVIEW** to preview your image.
    * In the Cloud Shell terminal, call the Vision API using the bucket name as an absolute URL, and replace `<BUCKET_NAME>` with your own bucket name:
       ```
       gcloud ml vision detect-landmarks gs://<BUCKET_NAME>/image02.jpeg
       ```

       Again, the request returns a response in JSON format with the landmark details.
 1. Identify the landmark in **image03.jpeg** using a remote URL:
    * In the GCP console, return to your bucket and select the **image03.jpeg** file name to preview the image.
    * In the Cloud Shell terminal, call the Vision API using the bucket name, and replace `<BUCKET_NAME>` with your own bucket name:
       ```
       gcloud ml vision detect-landmarks gs://<BUCKET_NAME>/image03.jpeg
       ```

       Again, the request returns a response in JSON format with the landmark details.

## Conclusion

Congratulations — you've completed this hands-on lab!
