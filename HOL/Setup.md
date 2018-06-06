
# Azure Security and Management setup

## Requirements

-   A corporate e-mail address (*e.g.*, your \@microsoft.com email)

-   Microsoft Azure subscription must be pay-as-you-go or MSDN

    -   Trial subscriptions will *not* work

-   Local machine or an Azure LABVM virtual machine configured with:

    -   Visual Studio 2017 Community Edition or later

    -   Azure SDK 2.9.+ or Later for Visual Studio

    -   Azure PowerShell 4.0 or later


## Before the hands-on lab

Duration: 30 mins

### Overview

Before attending the HOL, you should follow these steps to prepare your environment for an efficient day. Your first task will be to build a **LABVM** to use for the HOL and download some student files that will be used. Then, you will create a new Azure Dashboard to use during the HOL.

### Task 1: Build a Lab Virtual Machine in Azure.

1.  Launch a browser and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or just a Microsoft Account.

Note: You may need to launch an \"in-private\" session in your browser if you have multiple Microsoft Accounts.

2.  Click on **+Create a resource**, and in the search box, type in **Visual Studio Community 2017,** and press enter. Click the Visual Studio Community 2017 image running on Windows Server 2016.\
    ![In the Azure Portal, Visual Studio Community 2017 on Windows Server 2016 (x64) is selected.](images/Setup/image3.png "Azure Portal")

3.  Leave the default of *Resource Manager* deployment model and click **Create**.\
    ![Under Select a deployment model, the Create button is selected.](images/Setup/image4.png "Create button")

4.  Set the following configuration on the Basics tab and click **OK**.

    -   Name: **LABVM**

    -   VM disk type: **SSD**

    -   User name: **demouser**

    -   Password: **demo\@pass123**

    -   Subscription: **If you have multiple subscriptions, choose the subscription to execute your labs in.**

    -   Resource Group: **OPSLABRG**

    -   Location: **Choose the closest Azure region to you.**

    ![Fields in the Basics blade are set to the previously defined settings.](images/Setup/image5.png "Basics blade")

5.  Choose the **DS2\_V2 or D2S\_V3 Standard** instance size on the Size blade.

    > Note: You may have to click the View All link to see the instance sizes.

    ![The Choose a size blade displays](images/Setup/image6.png "Choose a size blade")

6.  Accept the default values on the Settings blade and click **OK**. On the Summary page, click **OK**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    ![The Submitting deployment for Visual Studio Community button displays.](images/Setup/image7.png "Submitting deployment button")

7.  Once the deployment is complete, move on to the next exercise.

### Task 2: Connect to LABVM & download and unzip student files

1.  Move back to the Portal page on your local machine and wait for **LABVM** to show the Status of **Running**. Once it is running, click **Connect** to establish a new Remote Desktop Session.

    ![In the Virtual Machine blade, the Connect button is selected.](images/Setup/image8.png "Virtual Machine blade")

2.  Depending on your remote desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

3.  Login with the credentials specified during creation:

    a.  User: **demouser **

    b.  Password: **demo\@pass123**

4.  You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Click **Yes** to continue with the connection.

    ![Screenshot of the Remote Desktop Connection dialog box.](images/Setup/image9.png "Remote Desktop Connection dialog box")

5.  When logging on for the first time, you will see a prompt on the right asking about network discovery. Click **No**.

    ![The No button is selected in the Networks prompt.](images/Setup/image10.png "Networks prompt")

6.  Open Server Manager (**Start Server Manager).** On the left, click **Local Server**.

    ![Screenshot of the Local Server option](images/Setup/image11.png "Local Server")

NOTE: Server Manager may open by default.

7.  On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![IE Enhanced Security Configuration is set to On.](images/Setup/image12.png "IE Enhanced Security Configuration option")

8.  Change to **Off** for Administrators and click **OK**.

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box.](images/Setup/image13.png "Internet Explorer Enhanced Security Configuration dialog box")

9.  In the lower left corner, click on the **Windows** button to open the **Start Screen**. Then, click **Internet Explorer** to open it. On first use, you will be prompted about security settings. Accept the defaults by clicking **OK**.

    ![Screenshot of the Internet Explorer 11 dialog box.](images/Setup/image14.png "Internet Explorer 11 dialog box")

10. If prompted, choose to Turn Protected mode on.

    ![The Protected mode is set to Turn on Protected mode.](images/Setup/image15.png "Protected mode button")

11. In the URL address window enter the below URL and hit the Enter key. This will download the class files (in a .zip format) needed for the remaining labs. <https://cloudworkshop.blob.core.windows.net/operations-management-suite/StudentFiles.zip>

NOTE: In some Azure VM images, the image is configured so that downloads are disabled. To enable the download of the Student Files, go to Internet Options, select the Security Tab, and on the Internet Zone select \"Custom Level\". Then scroll down to the Downloads section and select the radio button for Enable in the File Download subsection.

12. You will be prompted about what you want to do with the file. Select **Save**.

    ![The Internet Explorer dialog box asks what you want to do with the StudentFiles.zip file, and Save is selected.](images/Setup/image16.png "Internet Explorer dialog box")

13. Download progress is shown at the bottom of the browser window. When the download is complete, click **Open folder**.

14. The **Downloads** folder opens. ***Right-click*** the zip file and click **Extract All**. In the **Extract Compressed (Zipped) Folders** window, enter **C:\\HOL** in the **Select a Destination and Extract Files** dialog. Click the **Extract** button.

### Task 3: Create a new Azure portal dashboard

1.  Open Internet Explorer on LABVM and point to <https://portal.azure.com>

2.  Sign in to Azure using your credentials.

    ![Screenshot of the Azure sign-in pop-up.](images/Setup/image17.png "Azure sign-in  pop-up")

3.  Once you are at the Azure Portal Dashboard click **New Dashboard,** and type the name **My Dashboard,** then click **done customizing.**

    ![Screenshot of the Azure Portal dashboard.](images/Setup/image18.png "Azure Portal dashboard")

    ![The Tile Gallery search field is set to My Dashboard.](images/Setup/image19.png "Tile Gallery")

4.  Then navigate to your **LABVM** blade and use the "**Pin**" to add it to **My Dashboard**. This Dashboard will be used for the rest of this HOL.

    ![The pin icon is called out on the LabVM blade.](images/Setup/image20.png "LabVM blade")

5.  If you're going to be finishing this lab today, then continue to the next exercise. Otherwise, if you won't be finishing the rest of the lab today, then it may be helpful to click **Stop** on your **LABVM** within the Azure Portal. This will put the VM into a Stopped / Deallocated state and save money until it's needed again. When you're ready to continue with the lab, then navigate back to the **LABVM** blade and click **Start** to start it back up again.

### Summary

In this exercise, you built a LABVM to use for the HOL and downloaded some student files that will be used. Then, you created a new Azure Dashboard to use during the HOL.

> Note: You should follow all steps provided before attending the HOL.
