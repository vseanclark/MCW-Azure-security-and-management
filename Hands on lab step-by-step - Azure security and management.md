# Azure security and management

## Hands-on lab step-by-step

## May 2018

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners

-   [Contents](#contents)
    -   [Abstract and learning objectives ](#abstract-and-learning-objectives)
    -   [Overview](#overview)
    -   [Solution architecture](#solution-architecture)
    -   [Requirements](#requirements)
    -   [Before the hands-on lab](#before-the-hands-on-lab)
        -   [Overview](#overview-1)
        -   [Task 1: Build a Lab Virtual Machine in Azure.](#task-1-build-a-lab-virtual-machine-in-azure.)
        -   [Task 2: Connect to LABVM & download and unzip student files](#task-2-connect-to-labvm-download-and-unzip-student-files)
        -   [Task 3: Create a new Azure portal dashboard](#task-3-create-a-new-azure-portal-dashboard)
        -   [Summary](#summary)
    -   [Exercise 1: Configure Azure automation](#exercise-1-configure-azure-automation)
        -   [Overview](#overview-2)
        -   [Task 1: Create automation account](#task-1-create-automation-account)
        -   [Task 2: Add an Azure Automation credential](#task-2-add-an-azure-automation-credential)
        -   [Task 3: Upload DSC configurations into automation account](#task-3-upload-dsc-configurations-into-automation-account)
        -   [Summary](#summary-1)
    -   [Exercise 2: Build CloudShop environment](#exercise-2-build-cloudshop-environment)
        -   [Overview](#overview-3)
        -   [Task 1: Template deployment](#task-1-template-deployment)
        -   [Task 2: Allow remote desktop to the WEBVM1 & WEBVM2 using NAT rules](#task-2-allow-remote-desktop-to-the-webvm1-webvm2-using-nat-rules)
        -   [Task 3: Configure diagnostics accounts for the VMs](#task-3-configure-diagnostics-accounts-for-the-vms)
        -   [Summary](#summary-2)
    -   [Exercise 3: Build and configure Azure Security Center and Azure Management](#exercise-3-build-and-configure-azure-security-center-and-azure-management)
        -   [Overview](#overview-4)
        -   [Task 1: Provision Log Analytics through Azure Monitor](#task-1-provision-log-analytics-through-azure-monitor)
        -   [Task 2: Explore Security Center](#task-2-explore-security-center)
        -   [Task 3: Add Service Map](#task-3-add-service-map)
        -   [Task 4: Configure Service Map](#task-4-configure-service-map)
        -   [Task 5: Configure Update Management](#task-5-configure-update-management)
        -   [Task 6: Configure Inventory Tracking and Change Management](#task-6-configure-inventory-tracking-and-change-management)
        -   [Summary](#summary-3)
    -   [Exercise 4: Instrument CloudShop using Azure Application Insights](#exercise-4-instrument-cloudshop-using-azure-application-insights)
        -   [Overview](#overview-5)
        -   [Task 1: Install and Configure the Application Insights Status Monitor](#task-1-install-and-configure-the-application-insights-status-monitor)
        -   [Task 3: Simulate a failure of the CloudShop application](#task-3-simulate-a-failure-of-the-cloudshop-application)
        -   [Summary](#summary-4)
    -   [Exercise 5: Explore Azure Security and Operations Management, Application Insights and build a dashboard](#exercise-5-explore-azure-security-and-operations-management-application-insights-and-build-a-dashboard)
        -   [Overview](#overview-6)
        -   [Task 1: Work with Log Analytics data and Azure Monitor](#task-1-work-with-log-analytics-data-and-azure-monitor)
        -   [Task 2: Prevention](#task-2-prevention)
        -   [Task 3: Set up a manual activity log alert](#task-3-set-up-a-manual-activity-log-alert)
        -   [Task 4: Installing & using the Azure mobile application](#task-4-installing-using-the-azure-mobile-application)
        -   [Task 5: Application Insights](#task-5-application-insights)
        -   [Summary](#summary-5)
    -   [After the hands-on lab ](#after-the-hands-on-lab)
        -   [Overview](#overview-7)

## Azure security and management hands-on lab step-by-step

## Abstract and learning objectives 

The student will deploy and monitor a web application that has been deployed to Azure IaaS in this Hands-on Lab (HOL). Azure security and management services will be used to manage and monitor the operational performance and security of the underlying infrastructure. Azure Application Insights will be used to monitor performance, application usage, and identify the cause of any application issues that emerge.

## Overview

FusionTomo (FT) is a multi-national holding company headquartered in Los Angeles, CA that owns 48 manufacturing companies located in North America, Europe and Asia. These companies sell their products primarily to either distributors or large retail organizations around the world. FT, as the parent company, controls the IT systems for the companies that it owns and thus runs their e-commerce based applications. There are about 125 of these e-commerce applications used primarily for business-to-business purchasing by corporate buyers. These apps provide the bulk of FT's 15 billion dollars in revenue per year, so they are mission critical.

Recently FT has started to investigate what it would take to move from on-premises datacenters to the cloud. Most of their applications are ASP.NET running on Windows VMs with SQL Server in a traditional N-tier configuration. Their goal is to lift and shift these applications over to the cloud while gaining more control over the applications and improving their security posture.

They are looking for you to build out a prototype system in Azure using a sample web application they have provided to you called CloudShop.

They are looking for management tools that will allow them to have a full end-to-end view of both the infrastructure and application performance. Their goal will be to effectively lift and shift all applications over to the cloud. They do not have time or money to instrument the applications. Of course, security is on the top of the chain, so they also need a security solution and updated management system.

Per Roberto Milian, VP of Development and IT Operations - "FT's primary concern is how to best: deploy, test, manage, monitor, patch, secure and troubleshoot these applications in Azure IaaS."

## Solution architecture

![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image2.png)

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

#### Overview

Before attending the HOL, you should follow these steps to prepare your environment for an efficient day. Your first task will be to build a **LABVM** to use for the HOL and download some student files that will be used. Then, you will create a new Azure Dashboard to use during the HOL.

#### Task 1: Build a Lab Virtual Machine in Azure.

1.  Launch a browser and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or just a Microsoft Account.

    > Note: You may need to launch an \"in-private\" session in your browser if you have multiple Microsoft Accounts.

2.  Click on **+Create a resource**, and in the search box, type in **Visual Studio Community 2017,** and press enter. Click the Visual Studio Community 2017 image running on Windows Server 2016.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image3.png)

3.  Leave the default of *Resource Manager* deployment model and click **Create**.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image4.png)

4.  Set the following configuration on the Basics tab and click **OK**.

-   Name: **LABVM**

-   VM disk type: **SSD**

-   User name: **demouser**

-   Password: **demo\@pass123**

-   Subscription: **If you have multiple subscriptions, choose the subscription to execute your labs in.**

-   Resource Group: **OPSLABRG**

-   Location: **Choose the closest Azure region to you.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image5.png)

5.  Choose the **DS2\_V2 or D2S\_V3 Standard** instance size on the Size blade.
    > Note: You may have to click the View All link to see the instance sizes.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image6.png)

6.  Accept the default values on the Settings blade and click **OK**. On the Summary page, click **OK**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image7.png)

7.  Once the deployment is complete, move on to the next exercise.

#### Task 2: Connect to LABVM & download and unzip student files

1.  Move back to the Portal page on your local machine and wait for **LABVM** to show the Status of **Running**. Once it is running, click **Connect** to establish a new Remote Desktop Session.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image8.png)

2.  Depending on your remote desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

3.  Login with the credentials specified during creation:

    a.  User: **demouser**

    b.  Password: **demo\@pass123**

4.  You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Click **Yes** to continue with the connection.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image9.png)

5.  When logging on for the first time, you will see a prompt on the right asking about network discovery. Click **No**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image10.png)

6.  Open Server Manager (**Start Server Manager).** On the left, click **Local Server**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image11.png)

NOTE: Server Manager may open by default.

7.  On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image12.png)

8.  Change to **Off** for Administrators and click **OK**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image13.png)

9.  In the lower left corner, click on the **Windows** button to open the **Start Screen**. Then, click **Internet Explorer** to open it. On first use, you will be prompted about security settings. Accept the defaults by clicking **OK**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image14.png)

10. If prompted, choose to Turn Protected mode on.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image15.png)

11. In the URL address window enter the below URL and hit the Enter key. This will download the class files (in a .zip format) needed for the remaining labs. <https://cloudworkshop.blob.core.windows.net/operations-management-suite/StudentFiles.zip>

NOTE: In some Azure VM images, the image is configured so that downloads are disabled. To enable the download of the Student Files, go to Internet Options, select the Security Tab, and on the Internet Zone select \"Custom Level\". Then scroll down to the Downloads section and select the radio button for Enable in the File Download subsection.

12. You will be prompted about what you want to do with the file. Select **Save**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image16.png)

13. Download progress is shown at the bottom of the browser window. When the download is complete, click **Open folder**.

14. The **Downloads** folder opens. ***Right-click*** the zip file and click **Extract All**. In the **Extract Compressed (Zipped) Folders** window, enter **C:\\HOL** in the **Select a Destination and Extract Files** dialog. Click the **Extract** button.

#### Task 3: Create a new Azure portal dashboard

1.  Open Internet Explorer on LABVM and point to <https://portal.azure.com>

2.  Sign in to Azure using your credentials.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image17.png)

3.  Once you are at the Azure Portal Dashboard click **New Dashboard,** and type the name **My Dashboard,** then click **done customizing.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image18.png)\
    \
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image19.png)

4.  Then navigate to your **LABVM** blade and use the "**Pin**" to add it to **My Dashboard**. This Dashboard will be used for the rest of this HOL.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image20.png)

5.  If you're going to be finishing this lab today, then continue to the next exercise. Otherwise, if you won't be finishing the rest of the lab today, then it may be helpful to click **Stop** on your **LABVM** within the Azure Portal. This will put the VM into a Stopped / Deallocated state and save money until it's needed again. When you're ready to continue with the lab, then navigate back to the **LABVM** blade and click **Start** to start it back up again.

#### Summary

In this exercise, you built a LABVM to use for the HOL and downloaded some student files that will be used. Then, you created a new Azure Dashboard to use during the HOL.

**Note: You should follow all steps provided before attending the HOL.**

## Exercise 1: Configure Azure automation

Duration: 15 minutes

#### Overview

In this exercise, you will create and configure an Azure Automation account in the Azure Portal which will be used to configure the application servers using Azure DSC.

#### Task 1: Create automation account

1.  Browse to the Azure Portal, and authenticate at <https://portal.azure.com/>

2.  Click **+Create a resource,** and type **Automation** in the search box. Choose **Automation** from the results.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image21.png)

3.  Click **Create** on the Automation blade. This will display the **Add Automation Account** blade.

4.  On the **Add Automation Account** blade, specify the following information:

-  Name: **Automation-Acct**

-  Resource group: **HOLRGAUTO** (create a new resource group)

-  Location: **Choose the closest Azure region to you**

    > Note: Some features are only supported in East US 2 so please create your Automation Account in that region.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image22.png)

5.  Click the **Pin to Dashboard,** then click **Create**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image23.png)

#### Task 2: Add an Azure Automation credential

1.  The CloudShopSQL DSC configuration requires a credential object to access the local administrator account on the virtual machine. Within the Azure Automation DSC configuration click **Credentials** in the **SHARED RESOURCES** section.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image24.png)

2.  Click the **Add a credential** button.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image25.png)

3.  Specify the following properties and click **Create** to continue.

-  Name: **SQLLocalAdmin**

-  User Name: **demouser**

-  Password & Confirm: **demo\@pass123**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image26.png)

> Important: It is important to use the exact name for the credential, because one of the scripts you upload in the next step references the name directly.

#### Task 3: Upload DSC configurations into automation account

1.  Click **Resource groups \> HOLRG \> Automation-Acct** and click **DSC Configurations**.

> ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image27.png)

2.  Click on the **+ Add a configuration** button.

> ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image28.png)

3.  On the **Import** pane, upload both **C:\\HOL\\CloudShopSQL.ps1** and **C:\\HOL\\CloudShopWeb.ps1** files. You'll need to click on **+ Add a configuration** a second time to upload the second file.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image29.png)

4.  After importing the .ps1 files, click the **CloudShopSQL** DSC Configuration. Then, select **Compile** on the toolbar (*click **Yes** on the overwrite prompt*). Do the same for **CloudShopWeb.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image30.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image31.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image32.png)

5.  Make sure to review the DSC configurations to ensure they have completed the compile prior to moving on to the next step.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image33.png)

#### Summary

In this exercise, you configured an Automation account, and configured DSC configuration scripts that will be leveraged by the virtual machine resources.

## Exercise 2: Build CloudShop environment

Duration: 60 minutes

#### Overview

In this exercise, you will run a template deployment using an ARM template provided which will deploy a Virtual Network, Azure Load balancer, two IIS Servers and a SQL Server. The Servers will check into Azure Automation and run the DSC Configurations that you built in Exercise 1. This will configure the boxes with the CloudShop Application. You will also configure Inbound NAT Rules to allow RDP access to the Web Servers. Azure diagnostics will also be configured into a new storage account for the VMs.

#### Task 1: Template deployment

1.  In the portal, open your Azure Automation Account created earlier.

2.  Under **Account Settings,** locate the and click **Keys.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image34.png)

3.  Open ***Notepad*** and copy both the **Primary Access Key** and the **URL**. These will be needed inputs for the template deployment.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image35.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image36.png)

4.  In the Azure Portal, click the **+Create a resource** button. In the Search box, type **Template Deployment.**

5.  Select **Template Deployment and** click **Create** on the following screen.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image37.png)

6.  On the **Custom deployment** screen, select **Build your own template in the editor**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image38.png)

7.  Click **Load File.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image39.png)

8.  In the Choose File to Upload dialog, navigate to the **C:\\HOL** folder, and locate the **OMSHacakthon-azuredeploy.json** file.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image40.png)

9.  The JSON file will now be in the text window and the Parameters, Variables, and Resources should load in the Window. Click **Save**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image41.png)

10. Once saved, the window will change to a screen which is asking for inputs. Use the following information to complete.

    > NOTE: In your student files C:\\HOL\\parameters.txt there is a parameters file that you can use to quickly copy and paste into the portal.

    a.  Subscription: **Use the current subscription**

    b.  Resource Group: **HOLRG** (create a new resource group)

    c.  Location: **Choose the closest Azure region to you**

    d.  HOL Storage Type: **Premium\_LRS**

    e.  HOL VM Name: **WEBVM1**

    f.  HOL VM Admin User Name: **demouser**

    g.  HOL VM Admin Password: **demo\@pass123**

    h.  HOL VM Windows OS Version: **2016-Datacenter**

    i.  HOL Public IP DNS Name: **hol-then-five-random-lowercase-characters**

    j.  Registration Key: Locate in the Automation Account Blade/Keys

    k.  Registration URL: Locate in the Automation Account Blade/Keys

    l.  Webnode Configuration Name: **CloudShopWeb.WebServer**

    m.  Sqlnode Configuration Name: **CloudShopSQL.SQLSERVER**

    n.  Reboot Node If Needed: **true**

    o.  Allow Module Overwrite: **true**

    p.  Configuration Mode: **ApplyAndMonitor**

    q.  Configuration Mode Frequency Mins: **15**

    r.  Refresh Frequency: **30**

    s.  Action After Reboot: **ContinueConfiguration**

    t.  HOL SQL VM Name: **SQLVM**

    u.  HOL SQL VM Admin Name: **demouser**

    v.  HOL SQL VM Admin Password: **demo\@pass123**

    w.  HOL Sql VMSKU: **SQLDEV**

    x.  VM Size SQL: **Standard\_DS2\_v2**

    y.  HOL VM2Name: **WEBVM2**

    z.  HOL VM2 Admin Name: **demouser**

    a.  HOL VM2 Admin Password: **demo\@pass123**

    b.  HOL VM2 Windows OS Version: **2016-Datacenter**

    c.  Web AV Set Name: **webAVSet**


11. Once completed, click the **I agree to the terms and conditions stated above,** and the **Pin to Dashboard** followed by **Purchase.**\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image42.png)

12. A Deployment tile will appear on your **My Dashboard**. This deployment should take about 25-30 minutes. The servers will take some time to check in with Azure Automation and configure the CloudShop application.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image43.png)

    > NOTE: Wait for the Deployment to successfully complete prior to moving on to the next steps.

13. Now that the servers are built, and the deployment is complete, let's verify the servers are up and running properly.

14. In the Azure Portal, open the **HOLRG,** and locate the Public IP Address **hackathonPublicIP**. Click on the blade to open.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image44.png)

15. On the **hackathonPublicIP** blade, locate the DNS Name you provided during the template deployment. If you hover your mouse over the name, you can **click to copy** the name to the clipboard. Paste this into your Notepad document you opened earlier.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image45.png)

16. Open a new tab in Internet Explorer and paste the URL. This is the DNS name that is attached to the Azure Load Balancer in front of the CloudShop Application web servers **WEBVM1** & **WEBVM2**.

17. When the page loads, the CloudShop application should appear, and it will show which VM is serving the web page. In this screen capture, we see it is running on **WEBVM2**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image46.png)

18. By pressing **F5** on your keyboard, you can refresh the website until you see that **WEBVM1** is also serving webpages.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image47.png)

#### Task 2: Allow remote desktop to the WEBVM1 & WEBVM2 using NAT rules

Now that the deployment and the application is up and running, the next step is to allow RDP to the Web Servers. This will be accomplished by building NAT Rules through the Azure Load Balancer.

1.  Open the **HOLRG** Resource Group and locate **loadBalancer1**. Click to open its administration blade.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image48.png)

2.  On the **loadBalancer1** blade, locate **Inbound NAT rules,** and click in **Settings**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image49.png)

3.  Click **+Add,** and complete the blade using the following information:

    i.  Name: **rdp-webvm1**

    a.  Frontend IP Address: **accept default**

    b.  Service: **RDP**

    c.  Port: **3389**

    d.  Associated to: **webavset**

    e.  Target: **Choose a virtual machine: WEBVM1**

    f.  Network IP configuration: **ipconfig1 (10.0.0.4)**

    g.  Port mapping: **Default**
        ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image50.png)

4.  The portal will give a notice that it is: **"Saving load balancer inbound NAT rule**". Wait until this completes before continuing.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image51.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image52.png)

5.  Click **+Add,** and complete the blade using the following information:

-  Name: **rdp-webvm2**

-  Frontend IP Address: **accept default**

-  Service: **RDP**

-  Port: **3390**

-  Target **Choose a virtual machine: WEBVM2**

-  Network IP configuration: **ipconfig1 (10.0.0.5)**

-  Port mapping: **Custom**

-  Floating IP: **Disabled**

-  Target Port: **3389**

   ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image53.png)

6.  The portal will give a notice that it is: **"Saving load balancer inbound NAT rule**". Wait until this completes before continuing.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image51.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image54.png)

7.  To verify the new NAT Rules are working, move back to the **HOLRG** in the Azure portal. Click **WEBVM1,** and the **Connect** Link should now be available. Click **Connect,** and the portal will download an RDP file. Open this and connect to the VM.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image55.png)

8. Click **Open** when the RDP file downloads.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image56.png)

9. You will get a warning about the publisher of the RDP file being unknown. Click **Don't ask me again for connections to this computer** and click **Connect.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image57.png)

10. When prompted by Windows Security, enter your credentials.
- User Name: **demouser**

- Password: **demo\@pass123**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image58.png)

11. A warning will appear stating: **The identity of the remote computer cannot be verified. Do you want to connect anyway?** Click the checkbox for the disclaimer: **Don't ask me again for connection to this computer.** Then, click **Yes**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image59.png)

    > NOTE: When connecting to machines during this lab for the first time, you may encounter the same warnings etc. Follow these same steps to no longer receive those warnings as they do not apply to our setup.

12. When logging on for the first time, you will see a prompt on the right asking about network discovery. Click **No**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image10.png)

13. Notice the Server Manager opens by default. On the left, click **Local Server**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image11.png)

14. On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image12.png)

15. Change to **Off** for Administrators and click **OK**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image13.png)

16. In the lower left corner, click on the **Windows** button to open the **Start Screen**. Then, click **Internet Explorer** to open it. On first use, you will be prompted about security settings. Accept the defaults by clicking **OK**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image14.png)

17. Leave your RDP Session to **WEBVM1** open and minimized. Then, repeat this same procedure for **WEBVM2**.

#### Task 3: Configure diagnostics accounts for the VMs

In this task, you will configure the VMs to capture diagnostic data in an Azure Storage Account. Later you will connect this account to the Azure Security Center and Log Analytics.

1.  In the Azure Portal navigate to the **HOLRG** Resource Group and locate **WEBVM1**. Click the name to open the blade.

2.  On the **WEBVM1,** locate the **Monitoring** section, and click on **Diagnostic Settings**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image60.png)

3.  Click the **Enable guest-level monitoring** button. This will load more information to choose from. Click the Storage Account Configure Required Settings, and then, choose the Storage Account you just created.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image61.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image62.png)

4.  Select the Configure performance counters.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image63.png)

5.  Next check the **ASP.NET box,** and click **Save**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image64.png)

6.  This will submit a deployment for **WEBVM1**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image62.png)

> NOTE: You will need to wait for the portal to complete the update before moving to the next step.

7.  Complete the same steps for **WEBVM2**.

NOTE: You will need to wait for the portal to complete the update before moving to the next step.

8.  Next, using the same steps, configure **SQLVM** for Diagnostics as well, Select the following metrics for this SQL Server, and click **Save**.

    a.  SQL Metrics

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image65.png)

#### Summary

In this exercise, you ran a template deployment using an ARM template provided which created a Virtual Network, Azure Load balancer, two IIS Servers and a SQL Server. The servers checked into Azure Automation and ran the DSC Configurations that configured the boxes with the CloudShop Application. You then configured Inbound NAT Rules to allow RDP access to the Web Servers and successfully connected. Azure diagnostics was also configured into a new storage account for the VMs.

## Exercise 3: Build and configure Azure Security Center and Azure Management

Duration: 30 minutes

#### Overview

The next step is to provision the Azure security and Azure management components of Azure Automation, configure the VMs for the CloudShop application to be managed by the portal, and configure the diagnostics storage account to load data into the Log Analytics platform. Additionally, you will configure Update Management, Inventory Tracking and Change Management as well as install and configure the Service Map solution pack.

#### Task 1: Provision Log Analytics through Azure Monitor

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking **All services**, searching for "*monitor*", and selecting **Monitor**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image66.png)

2.  In the **Overview** blade, click the **Add** button below **Log Analytics**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image67.png)

3.  Complete the OMS Workspace blade using the following information. Then, click **Pin to Dashboard** and click **OK**:

    a.  OMS Workspace: **Unique name**

    b.  Subscription: **Select the current subscription**

    c.  Resource Group: **HOLRG**

    d.  Location: **Closest to your deployment**

    e. Pricing Tier: **Select Per Node (OMS)**
        ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image68.png)

4.  The deployment will only take a few moments to complete. Upon completion, open **Log Analytics** by clicking on the Log Analytics resource within the **HOLRG** resource group

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image69.png)

5.  Once it loads in the Azure Portal, move the slider bar down, and click **Virtual Machines** which is found in the **Workspace Data Sources** section.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image70.png)

6.  A list of the VMs in your subscription will be shown in the list. You may want to filter your view to see the VMs for this HOL. You will add the WEB and SQL VMs.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image71.png)

7.  Click the **SQLVM** to load a blade to the right. Then, click **Connect** to add it to this Log Analytics Workspace.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image72.png)

8.  Follow the same steps for the **WEBVM1** & **WEBVM2**.

9.  The portal should update to show that they are now a part of "This workspace" once they have all been added.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image73.png)

#### Task 2: Explore Security Center

1.  Open the Azure portal and navigate to the **Security Center** menu option (**All Services Security Center**).

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image74.png)

2.  This will present the **Security Center - Overview** screen. Notice that it's already collecting data. For this exercise, you want to upgrade to Standard tier which extends the capabilities of the Free tier to workloads running in private and other public clouds. It provides unified security management and thread protection across your hybrid cloud workloads. The Standard tier also adds advanced thread detection capabilities, which uses built-in behavioral analytics and machine learning to identify attacks and zero-day exploits, access and application controls to reduce exposure to network attacks and malware, and more.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image75.png)

3.  Click **Onboard to Advanced Security**.

4.  Select the subscription you would like to upgrade by clicking on the subscription name.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image76.png)

5.  Select the **Standard** pricing tier and click **Save**.\
    \
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image77.png)

6.  Close the panel and navigate back to the **Security Center Overview** screen. Click on **Security policy** under the **General** section. This is where you will enable data collection.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image78.png)

7.  This brings up the **Security policy** screen where you will click on your subscription name which is currently in an Off state for data collection.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image79.png)

8.  This presents the **Data Collection** screen. Turn on data collection by clicking the **On** button, selecting **Use another workspace** and selecting the Log Analytics workspace created in [Task 1: Provision Log Analytics](#task-1-provision-log-analytics-through-azure-monitor) and then clicking **Save** and click on Yes if prompted.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image80.png)

#### Task 3: Add Service Map

In this section, you will add Service Map to Log Analytics.

1.  From the Azure portal, click the **+Create a resource** Link followed by **Monitoring + Management** and **See all**.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image81.png)\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image82.png)

2.  Note there are many solutions available, and more are added frequently. Browse the solutions to gain familiarity with the options.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image83.png)

3.  Locate the **Service Map** solution and select it. If you don't see it on the page, use the *Search Monitoring + Management* field at the top of the screen to search for **Service Map**. On the Details page, you can read about the solution. When ready, click **Create.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image84.png)

4.  Select the OMS Workspace you created and check the **Pin to dashboard** before selecting **Create.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image85.png)

#### Task 4: Configure Service Map

To configure the Service Map functionality, the Microsoft Dependency Agent needs to be installed on each virtual machine.

1.  Open a Remote Desktop Connection to **WEBVM1**. Use the same credentials we configured during the earlier exercise.

2.  Open the web browser to download and run the installer from <https://aka.ms/dependencyagentwindows>

3.  The installer will start. Click **I Agree.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image86.png)

4.  The installer will take a few moments and once it has completed, click on the **Finish** button.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image87.png)

5.  Repeat the same steps above for **WEBVM2**.

6.  Disconnect from any remote desktop connections.

7.  In the Azure Portal, navigate to the **All Resources** menu and locate the **Service Map** resource you created earlier. Click on the resource name.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image88.png)

8.  This will bring up the Service Map screen and you'll see that it's already populated with some data. Since we installed the agent on the two Windows virtual machines, the Service Map is showing both virtual machines now reporting data. Click on the **Service Map** tile.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image89.png)

9.  When the Service Map loads, click on **WEBVM1** to see the data that has been analyzed for that virtual machine.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image90.png)

#### Task 5: Configure Update Management

The Update Management functionality will be configured through your Virtual Machines.

1.  In the **Azure Portal**, browse to **WEBVM1**.

2.  Select **Update management** under **OPERATIONS**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image91.png)

3.  Select your **Log Analytics workspace** and **Automation Account** and click **Enable**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image92.png)

4.  Wait for the deployment to complete. This can take up to 15 minutes.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image93.png)

    > Do not navigate away from the Update Management blade until the deployment message reads "The 'Update Management' solution is being deployed on this virtual machine. This can take a few minutes. You can do other work while this is in progress."

5.  While the solution is deploying, navigate to **WEBVM2** and repeat steps 2-4.

6.  While the solution is deploying, navigate to **SQLVM** and repeat steps 2-4.

7.  Verify that the solution has been deployed by navigating to **WEBVM1** and clicking on **Update management** under **OPERATIONS**.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image91.png)\
    \
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image94.png)

#### Task 6: Configure Inventory Tracking and Change Management

The Update Management functionality will be configured through Azure Automation.

1.  In the **Azure Portal**, browse to **WEBVM1**.

2.  Select **Change tracking** under **OPERATIONS**.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image95.png)

3.  Verify the **Log Analytics workspace** and **Automation Account** and click **Enable**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image96.png)

4.  Wait for the deployment to complete. This can take a few minutes.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image97.png)

Do not navigate away from the Update Management blade until the deployment message reads "The 'Change Tracking and Inventory' solution is being deployed on this virtual machine. This can take a few minutes. You can do other work while this is in progress."

5.  While the solution is deploying, navigate to **WEBVM2** and repeat steps 2-4.

6.  While the solution is deploying, navigate to **SQLVM** and repeat steps 2-4.

7.  Verify that the solution has been deployed by navigating to **WEBVM1** and clicking on **Change tracking** under OPERATIONS.\
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image95.png)\
    \
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image98.png)

#### Summary

In this exercise, you provisioned the portal, configured the VMs for the CloudShop application to be managed by the Portal, and configured the diagnostics storage account to load data into the Log Analytics platform. Additionally, solution packs were installed and configured to gather data and provide dashboards for applications deployed in Azure IaaS. You also configured Service Map and now understand how it surfaces data.

## Exercise 4: Instrument CloudShop using Azure Application Insights

Duration: 45 minutes

#### Overview

In this exercise, you will instrument the CloudShop using Application Insights at runtime. This will be accomplished by installing the Applications Insights tool on the web services and configuring an Application Insight workspace in Azure. Then, you will configure Application Insights to perform web tests and alerts. The final task will be to connect the Application Insights workspace to send data to the Portal.

#### Task 1: Install and Configure the Application Insights Status Monitor

To read more about this tool follow this link: <http://bit.ly/2ksdzKV>

1.  Open a Remote Desktop Connection to **WEBVM1**.

2.  Open Internet Explorer and follow this link: <http://bit.ly/2jxQ43z>. Click **Run** on the Question if you want to run the file: **AppliationsInsightsMonitor.exe**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image99.png)

3.  This will start the Web Platform Installer. Click **Install** followed by **I Accept** on the following screen, and **Continue**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image100.png)

4.  Once the Monitor is installed, click the **Sign In** link under the **Configuration**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image101.png)

5.  You will sign-in to Azure as normal.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image102.png)

6.  Under the **Send telemetry to**: Select **New Application Insights resource**. Then, click **Configure settings**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image103.png)

7.  On the **Configuration settings for Application Insights**, complete the information as follows, and click **OK**.

-  Microsoft Azure Subscriptions: **Use the same subscription**

-  Resource Groups: **HOLInsights**

-  Application Insights Resource: **HOLCloudShop**

-  Location: **Select the same region as your deployment**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image104.png)

8.  This will build the Application Insights workspace for you in Azure.

9.  Next, click **Add** **Application Insights**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image105.png)

10. Click **Restart IIS** to complete the Setup.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image106.png)

11. This will only take a few seconds. Now, the CloudShop application running on **WEBVM1** is instrumented and sending data to **Azure Application Insights**. The monitor on **WEBVM1** should now look like below:

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image107.png)

12. Disconnect from **WEBVM1**.

13. Connect to a Remote Desktop Session for **WEBVM2**.

14. Open Internet Explorer and follow this link: <http://bit.ly/2jxQ43z>. Click **Run** on the Question if you want to run the file: **AppliationsInsightsMonitor.exe**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image99.png)

15. This will start the Web Platform Installer. Click **Install** followed by **I Accept** on the following screen, and **Continue**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image100.png)

16. Once the Monitor is installed, click the **Sign In** link under the **Configuration**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image101.png)

17. You will sign-in to Azure as normal.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image102.png)

18. Under the **Send telemetry to**: Select **Existing Application Insights resource**. Then, click **Configure Settings**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image108.png)

19. On the **Configuration settings for Application Insights**, complete the information as follows, and click **OK**.

- Microsoft Azure Subscriptions: **Use the same subscription**

- Resource Groups: **HOLInsights**

- Application Insights Resource: **HOLCloudShop**

- Location: **Select the same region as your deployment**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image109.png)

20. This will attach **WEBVM2** to the Application Insights workspace you created a moment ago in Azure. Next, click **Add** **Application Insights**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image110.png)

21. Click **Restart IIS** to complete the Setup.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image106.png)


22.  Click the ellipse on each '...' tile, and click through each option to see the different data being pulled as a part of the Application Insights data.

     ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image112.png)

     ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image113.png)

     ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image114.png)

23. Close the **App Map** pane, then Click on the **Alerts** in the **Configure** section.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image115.png)

24. Click **+Add Metric Alert,** complete the blade with the following information, and click **OK**.
-  Name: **CloudShopProcessorAlert**
-  Metric: **Processor Time**
-  Condition: **Greater than**
-  Threshold: **80**
-  Period: **Over the last 5 minutes**
- Notify via: Email owners, contributors and readers -- **Check the Box**
    
    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image116.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image117.png)

25. The portal will update with the new Alert.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image118.png)

26.  In your **HOLRG,** locate the **hackathonPublicIP** Public IP Address, and take note of the DNS name which is on the front of the Azure Load Balancer for the CloudShop App running on **WEBVM1** & **WEBVM2**.

27.  Next in the **HOLCloudShop** Application Insights workspace, under the **Investigate** section, and click **Availability**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image119.png)

28.  Click **+Add test**, and complete the blade using the following information. Then, click **Create**.

-  Test name: **CloudShopWebTest**

-  Test type: **URL Ping Test**

-  URL: **http://HOLXXXXXX.southcentralus.cloudapp.azure.com**

-  Test locations: Choose locations from all over the world

-  All other accept defaults

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image120.png)

> Note: If the CloudShop Application becomes unavailable to this WebTest, you will then receive an email alert from Azure Application Insights.

29. Click **Performance Testing** in the **Configure** section.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image121.png)

30. Click **+New**.

31. Click **Configure Test Using** and complete this using these inputs. Then, click **Done**.

    m.  Test Type: Manual Test

    n.  URL: **http://HOLXXXXXX.southcentralus.cloudapp.azure.com**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image122.png)

32. Complete the **New Performance Test** blade using the following information, and click **Run Test**.

-  Name: **CloudShopLoadTest**

-  Generate Load from: **Select a Region**

-  User Load: **2000**

-  Duration: **5**

   ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image123.png)

> NOTE: An error may occur if you do not have a Visual Studio Team Services (VSTS) Account configured. If so, then you'll need to create one before setting up the Performance Test.

13. Once this is submitted it will show as **Queued.** Click the line and then details about the performance test will be shown.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image124.png)

    > Click the Messages box to see the details of the test.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image125.png)

14. Now, head back to the Overview blade of the **HOLCloudShop** Application Insights and click **Live Stream**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image126.png)

15. Real-time application information can be seen regarding the CloudShop App running in Azure on our IaaS VMs. Here, you can wait for the Performance Test to run, and show how the Web Application performs.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image127.png)

16. If you go back to the **Performance Testing** blade, and click on **CloudShopLoadTest,** you will see the metrics form the run.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image128.png)![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image129.png)

17. Close the Performance Test, and click on the **Performance** under Investigate.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image130.png)

18. Explore the metrics from the CloudShop Application.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image131.png)

19. The Load Test should also have caused the Alert configured to be trigged. An email should have been received.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image132.png)

20. The alert will quickly resolve as the Load Test has completed causing the CPU condition to quiet.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image133.png)

#### Task 3: Simulate a failure of the CloudShop application

1.  Move to your **HOLRG** Resource group and stop both **WEBVM1** and **WEBVM2**.

2.  Navigate back to the **HOLCloudShop** Application Insights portal. Once the website goes down, notice there are now Alerts.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image134.png)

3.  Hover over the tile **Pin** **the** **Alerts** to **My Dashboard**. Click the **Alert Tile**, and this will load the Alert Rules Blade. The first alert will be an alert based on the WebTest. The second will be a Failure Anomalies Alert.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image135.png)

4.  Click the **CLOUDSHOPWEBTEST** alert which will show you more details about the error condition.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image136.png)

5.  A few email alerts should come into your inbox.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image137.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image138.png)

6.  Click the "See the analysis of this issue" which will load the Azure portal.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image139.png)

7.  Move back to your **HOLRG and** restart the VMs.

8.  Once the VMs are back online, the website will come back up. This will initiate responses to the Web Test and sending data to the Applications Insights portal. An email will be sent resolving the Alert. After a period of time, the Smart Detection Alert will also resolve.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image140.png)

#### Summary

In this exercise, you instrumented the CloudShop using Application Insights at runtime. This was accomplished by installing the Applications Insights Monitor for the web services and configuring an Application Insight workspace in Azure. Then, you configured Application Insights to perform web tests and alerts.

## Exercise 5: Explore Azure Security and Operations Management, Application Insights and build a dashboard

Duration: 45 minutes

#### Overview

In this exercise, you will explore the information and data being provided by Azure Security and Operations Management and Application Insights to gain situational awareness of the application and infrastructure. You will look at the Security posture of the infrastructure, the applications performance, and build a dashboard that can be used to manage it moving forward.

#### Task 1: Work with Log Analytics data and Azure Monitor

In this section, we will perform an ad-hoc search in Log Analytics data to see where our servers are not in compliance with security baselines. In the Log Search interface, we can perform ad-hoc searches against the log data being ingested into the Log Analytics service. Because the data is indexed, searching is very fast.

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking **All services**, searching for "*monitor*", and selecting **Monitor**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image66.png)

2.  Select **Log Analytics** under **SHARED SERVICES** to navigate to your workspace.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image141.png)

3.  In the search field, enter the following query: *Update \| where OSType!=\"Linux\" and Optional==false*. Click Run. This will search the Update management logs and report results. There are many other data sources we can query.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image142.png)

    > Note: you may have less or more data, but keep in mind, the service has only been collecting data since you began the lab.

4.  Notice in the left, there are different types of data. Click **Windows Defender** followed by **Apply**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image143.png)

5.  Notice the query dialog (where you entered a search before) has a search string in it querying for the type "Product==Windows Defender".

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image144.png)

6.  As you click on options in Log Search, queries are built automatically. Also, notice the results are paired down to a smaller subset of data. Click on **Table** to view the data in a column and row format.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image145.png)

7.  This is a list of Windows virtual machines tracked in Update management that have pending updates for Windows Defender. To further refine the list, scroll on the left down to **UPDATESTATE** and select **Needed**. Notice the query automatically updates as it is a single point to refine.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image146.png)

8.  Further refinements can be made as needed. Select additional refiners to update the search.

9.  Next, sort the findings, so the Computers are sorted alphabetically. Click on the column header **Computer** until the virtual machines are sorted correctly.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image147.png)

10. Now we will export the list. This may be helpful if we wanted to divide the findings list out amongst administrators, so several people can help remediate the findings. Click on **Export**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image148.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image149.png)

11. Once the report is exported, you are prompted what to do with the file. Click on **Save**. Once completed, click on **Open**. Choose **Notepad** to open the file**.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image150.png)

12. Review the text file. Then, close it. You can also copy the file to your local PC and view in Excel.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image151.png)

13. Because this is a useful query, we can save it to be able to quickly run it again anytime we wish. First, copy the search query to the clipboard.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image152.png)

14. Click the **Saved Searches** followed by **+Add.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image153.png)

15. Complete the Add Saved Search Blade with this information:
    -  Display Name: **Computers Needing Windows Defender Updates**
    -  Category: **My Queries**
    -  Query: paste the query from your clipboard
    -  Function Alias: **WindowsDefNeeded**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image154.png)

16. Click **OK.**

#### Task 2: Prevention

In this section, we will use the Security Center Overview screen to review what preventative steps we can take to protect our environment.

1.  Within **Security Center** **Overview** page, there are four tiles in the middle of the screen under **Prevention**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image155.png)

2.  Click on the **Compute** tile to drill into the security health of your compute resources.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image156.png)

3.  Notice there are several recommendations at the bottom of the screen. Let's go ahead and drill into these recommendations. Click on the **Remediate security configuration** item.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image157.png)

4.  This brings up the **Remediate security configurations** panel where we can see there are many configuration items to address. Take some time to explore the items called out in this list. As you click on each item, you'll see more details that allow you better understand the recommendation, the vulnerability, and the potential impact.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image158.png)

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image159.png)

5.  Close the panel and navigate back to the **Security Center Overview** page. This time, click on the **Networking** tile to drill into the security health of your networking resources.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image160.png)

6.  Based on the details showing in the **Networking Recommendations**, it appears we don't have a Next Generation Firewall installed. We won't implement this recommendation today, but if we wanted to, we could click on the item and implement the recommendation from this panel.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image161.png)

7.  Close the panel and navigate back to the **Security Center Overview** screen. This time, click on the **Storage & data** tile.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image162.png)

8.  Notice this tile is green which is an indication the resources are in a healthy state. Azure Security Center will flag the tile as red if there are critical recommendations that need to be addressed.

9.  As a last step, let's navigate back to the **Security Center Overview** screen and click on the **Applications** tile.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image163.png)

10. As you review the Applications Security Health, notice there's a recommendation to implement a Web application firewall.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image164.png)

11. Go ahead and close this panel and return to the **Security Center Overview** page.

12. Azure Security Center provides a quick way to see all the recommendations we just saw in a single view. Click on the **Recommendations** tile under the Overview section.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image165.png)

13. This presents the same recommendations you saw by navigating to each resource type in the previous steps. Clicking the ellipse by each of the recommendations will provide you with additional steps you can take to remediate the recommendation.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image166.png)

#### Task 3: Set up a manual activity log alert

If one of the virtual machines in the resource group were to be restarted, that's a condition you would want to be notified of. In this section, we will set up a manual alert to detect this condition.

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking **All services**, searching for "*monitor*", and selecting **Monitor**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image66.png)

2.  Select **Alerts** under **SHARED SERVICES**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image167.png)

3.  Then, click **Add activity log alert** and fill out the form to match the screen shot below for the **Source**, **Criteria**, and **Alert via** sections. We're creating the alert to notify us anytime a virtual machine in the **HOLRG** is restarted. Note that we are configuring it to notify us via the Azure mobile application which will configure shortly.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image168.png)

4.  In the Actions section, enter a unique name in the **Action Name** field and select the **Action Type** of **Email/SMS/Push/Voice**. The details blade will open automatically.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image169.png)

5.  Selecting the **Email/SMS/Push/Voice** Action Type pops up a panel for entering the recipients email address. This is the same address you will use when you login to the Azure mobile application shortly. This is also the same address you are using to login to your Azure subscription. Click **OK**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image170.png)

6.  Confirmation messages will display once the action group and the alert rule have been created successfully by clicking **OK**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image171.png)

#### Task 4: Installing & using the Azure mobile application

In this section, you will take your monitoring solution mobile by installing and configuring the OMS Mobile Application.

1.  Open the **AppStore** or **Google Play** on your mobile device. Locate the search box and type **Microsoft Azure,** and press enter.

2.  When you locate the Microsoft Azure application, install it to your device.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image172.png)

3.  Once the application has been installed, you will need to allow the application to send you notifications.

4.  Next, touch the Sign In Button.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image173.png)

5.  Once the login page loads, enter your Azure Credentials.

6.  Once you are logged into the app, navigate to the Notifications menu to see there are currently no notifications.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image174.png)

7.  Putting the phone aside for a moment, in your desktop web browser, navigate to the **HOLRG** resource group list in the Azure portal.

8.  Click on the WEBVM1 that you created earlier. We will restart this virtual machine, which should cause a trigger to the Azure mobile application.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image175.png)

9.  Click the **Restart** button the toolbar to restart the virtual machine.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image176.png)

10. Click **Yes** to confirm that you want to restart this virtual machine.

11. In a few moments, you will receive an alert through the Azure mobile app that the virtual machine was restarted.

#### Task 5: Application Insights

Understanding what is happening within an application can be very challenging, but with the Application Insights configured for CloudShop, there is great telemetry being fed to the Azure Portal. Here, you will investigate that data regarding how the CloudShop is performing.

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking **All services**, searching for "*monitor*", and selecting **Monitor**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image66.png)

2.  Locate the **HOLCloudShop** Applications Insight resource in the **Overview** blade and click **HOLCloudShop.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image177.png)

1.  From the **Overview** blade, click **Pin to Dashboard**.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image178.png)

2.  Under the **INVESTIGATE** section, click the **Performance (preview)** item**.**

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image179.png)

3.  The **Performance** feature of Application Insights allows us to get rich performance monitoring and easy to consume dashboards.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image180.png)

4.  This dashboard gives near real-time insight into what's happening with your application. In the screenshot below, you can see this application appears to have a performance issue with the Home/Index page. It appears to be taking 20.5 seconds on average to load. NOTE: Your numbers may not match. By clicking on the GET Home/Index, you will see the other sections of the dashboard will filter to performance data focused on that page.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image181.png)

5.  Feel free to experiment with this dashboard to understand the performance considerations of your application.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image182.png)

6.  Pin this dashboard to My Dashboard by clicking on the pin in the top right of the screen.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image183.png)

7.  At this point, navigate back to **My Dashboard** and click **Edit Dashboard,** and arrange the many tiles you have added to give yourself an all up view of the environment you have built. Feel free to open the various solutions, find interesting data and Pin it to the Dashboard.

    ![](images/Handsonlabstep-by-step-Azuresecurityandmanagementimages/media/image184.png)

#### Summary

In this exercise, you explored the information and data being provided by Azure Security and Operations Management and Application Insights to gain situational awareness of the application and infrastructure. You looked at the Security Posture of the infrastructure, the performance of applications, and you built a dashboard that can be used to manage it moving forward.

## After the hands-on lab 

Duration: 10 mins

#### Overview

In this exercise, attendees will de-provision any Azure resources that were created in support of the lab.

1.  Delete the **HOLRG, HOLInsights**, and **OPSLABRG** resource groups.

