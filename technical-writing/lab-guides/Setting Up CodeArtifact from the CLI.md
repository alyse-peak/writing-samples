# Setting Up CodeArtifact from the CLI

## Introduction

This hands-on lab walks you through how to create CodeArtifact domains and repositories, and then populate the repositories using the AWS Command Line Interface.

## Solution

Log in to the lab environment with the `cloud_user` credentials provided. Ensure you are using the `us-east-1` Region throughout the lab.

### Prepare the AWS Environment

#### Prepare the EC2 Instance

 1. Navigate to **EC2**.
 1. In the **Launch instance** section of the dashboard, use the **Launch instance** dropdown to select **Launch instance**.
 1. Fill in the instance details:
    * **Name**: In the text field, enter *CodeArtifactDemo*.
    * **Application and OS Images (Amazon Machine Image)**: Select **Amazon Linux**, and then use the corresponding dropdown to ensure that **Amazon Linux 2 AMI** is selected.
    * **Instance type**: Use the dropdown to select **t3.micro**.
 1. In the **Network settings** section, click **Edit** on the right.
 1. In the **Auto-assign public IP** field, use the dropdown to select **Enable**.
 1. Leave all other default settings, and click **Launch instance**.
 1. In the **Create key pair** dialog, select **Proceed without key pair**, and then click **Proceed without key pair** to confirm.
 1. Click **Launch instance** again.

#### Prepare the Access Key

 1. After the instance is successfully launched, navigate to **IAM**.
 1. In the sidebar menu, navigate to **Access management** and then select **Users**.
 1. From the **Users** list, select the **cloud_user** user name.
 1. From the user details, select the **Security credentials** tab.
 1. In the **Access keys** section, click **Create access key**.
 1. Select **Command Line Interface (CLI)**.
 1. Check the **I understand the above recommendation** checkbox toward the bottom of the page, and then click **Next**.
 1. Leave the tag section blank and click **Create access key**.

    Leave this tab open so you can refer back to your access key when you need it.

#### Prepare the AWS CLI Environment

 1. In a new browser tab, navigate to **EC2**.
 1. From the EC2 dashboard, select **Instances (running)**.
 1. Check the checkbox next to the **CodeArtifactDemo** instance, and then refresh the instance details until the **Status check** shows all checks passed.

    >**Note**: You may need to wait a moment and then refresh.

 1. Ensure the **CodeArtifactDemo** instance is still checked, and then click **Connect**.
 1. Click **Connect** again.
 1. In the terminal, set up your AWS CLI installation:
    ```
    aws configure
    ```
 1. When prompted for the access key, navigate back to your **IAM Management Console** tab and copy your access key.
 1. Navigate back to your **EC2 Instance Connect** tab and paste your copied access key into the terminal.
 1. When prompted for the secret key, navigate back to your **IAM Management Console** tab and copy your secret access key.
 1. Navigate back to your **EC2 Instance Connect** tab and paste your copied secret access key into the terminal.
 1. Enter the default Region name:
    ```
    us-east-1
    ```
 1. When prompted for the default output format, press **Enter**.

### Create a CodeArtifact Domain

 1. In the terminal, install Node Version Manager (nvm):
    ```
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
    ```
 1. Activate nvm:
    ```
    . ~/.nvm/nvm.sh
    ```
 1. Install Node.js version 16:
    ```
    nvm install 16
    ```
 1. Create the `demo-domain` CodeArtifact domain:
    ```
    aws codeartifact create-domain --domain demo-domain
    ```

### Create a Repository within the Domain

  - Create a `demo-repo` repository within the `demo-domain` CodeArtifact domain, replacing `<ACCOUNT_NUMBER>` with your account number for the lab (you can see this number next to your `cloud_user` details in the top-right corner):
    ```
    aws codeartifact create-repository --domain demo-domain --domain-owner <ACCOUNT_NUMBER> --repository demo-repo
    ```
    >**Note**: Do not include spaces in your account number.

### Create an Upstream Repository

 1. Create an `npm-store` repository within the `demo-domain` CodeArtifact domain, replacing `<ACCOUNT_NUMBER>` with your account number for the lab:
    ```
    aws codeartifact create-repository --domain demo-domain --domain-owner <ACCOUNT_NUMBER> --repository npm-store
    ```
 1. Link the `npm-store` repository with the public npm repository, replacing `<ACCOUNT_NUMBER>` with your account number for the lab:
    ```
    aws codeartifact associate-external-connection --domain demo-domain --domain-owner <ACCOUNT_NUMBER> --repository npm-store --external-connection "public:npmjs"
     ```
       This will allow you to pull packages from the public repository.
 1. Make `npm-store` an upstream repository to `demo-repo`, replacing `<ACCOUNT_NUMBER>` with your account number for the lab:
    ```
    aws codeartifact update-repository --repository demo-repo --domain demo-domain --domain-owner <ACCOUNT_NUMBER> --upstreams repositoryName=npm-store
    ```
    This allows `demo-repo` to inherit packages from `npm-store`.
 1. Configure npm in `demo-repo`, replacing `<ACCOUNT_NUMBER>` with your account number for the lab:
    ```
    aws codeartifact login --tool npm --repository demo-repo --domain demo-domain --domain-owner <ACCOUNT_NUMBER>
    ```
    You should see a message indicating that npm was successfully configured to use the `demo-repo` repository.
 1. Install a JavaScript library npm package:
    ```
    npm install lodash
    ```
 1. Install the `express`, `eslint`, and `mongo` packages:
    ```
    npm install express eslint mongo
    ```
 1. List the packages in `demo-repo`:
    ```
    aws codeartifact list-packages --domain demo-domain --repository demo-repo
    ```

### Validate and Clean Up Resources

1. Verify the packages in the AWS Management Console:
   * Navigate back to the **IAM Management Console** tab, and then navigate to **CodeArtifact**.
   * In the sidebar menu, navigate to **Artifacts** and then select **Domains**.
   * Select the **demo-domain** domain name.
   * View the contents of each repository one at a time.

     >**Note**: If the **npm-storeclear** repository is present, it will be empty; this is expected.

1. Attempt to delete **demo-domain**:
   * Navigate back to **demo-domain**.
   * On the right, click **Delete domain**.
   * When prompted, type *delete* into the text field to confirm the deletion, and then click **Delete**.

     You should see a warning that the domain must contain 0 repositories before deletion.
1. Delete the repositories:
   * Select one of the repository names.
   * On the right, click **Delete repository**.
   * When prompted, type *delete* into the text field to confirm the deletion, and then click **Delete**.
   * Repeat the process to delete any remaining repositories.
1. Attempt to delete **demo-domain** again:
   * Navigate back to **demo-domain**.
   * On the right, click **Delete domain**.
   * When prompted, type *delete* into the text field to confirm the deletion, and then click **Delete**.
   
     This time, the deletion should be successful.

## Conclusion

Congratulations — you've completed this hands-on lab!
