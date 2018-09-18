![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Azure security and management
</div>

<div class="MCWHeader2">
Hands-on lab Lab-guide
</div>

<div class="MCWHeader3">
September 2018
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners

**Contents**

<!-- TOC -->

- [Azure security and management hands-on lab step-by-step](#azure-security-and-management-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Configure Azure automation](#exercise-1--configure-azure-automation)
        - [Overview](#overview)
        - [Task 1: Create automation account](#task-1--create-automation-account)
        - [Task 2: Add an Azure Automation credential](#task-2--add-an-azure-automation-credential)
        - [Task 3: Upload DSC configurations into automation account](#task-3--upload-dsc-configurations-into-automation-account)
        - [Summary](#summary)
    - [Exercise 2: Build CloudShop environment](#exercise-2--build-cloudshop-environment)
        - [Overview](#overview)
        - [Task 1: Template deployment](#task-1--template-deployment)
        - [Task 2: Allow remote desktop to the WEBVM1 & WEBVM2 using NAT rules](#task-2--allow-remote-desktop-to-the-webvm1-webvm2-using-nat-rules)
        - [Task 3: Configure diagnostics accounts for the VMs](#task-3--configure-diagnostics-accounts-for-the-vms)
        - [Summary](#summary)
    - [Exercise 3: Build and configure Azure Security Center and Azure Management](#exercise-3--build-and-configure-azure-security-center-and-azure-management)
        - [Overview](#overview)
        - [Task 1: Provision Log Analytics through Azure Monitor](#task-1--provision-log-analytics-through-azure-monitor)
        - [Task 2: Explore Security Center](#task-2--explore-security-center)
        - [Task 3: Add Service Map](#task-3--add-service-map)
        - [Task 4: Configure Service Map](#task-4--configure-service-map)
        - [Task 5: Configure Update Management](#task-5--configure-update-management)
        - [Task 6: Configure Inventory Tracking and Change Management](#task-6--configure-inventory-tracking-and-change-management)
        - [Summary](#summary)
    - [Exercise 4: Instrument CloudShop using Azure Application Insights](#exercise-4--instrument-cloudshop-using-azure-application-insights)
        - [Overview](#overview)
        - [Task 1: Install and Configure the Application Insights Status Monitor](#task-1--install-and-configure-the-application-insights-status-monitor)
        - [Task 2: Explore the Application Map, configure alerts, availability tests, and performance tests](#task-2--explore-the-application-map--configure-alerts--availability-tests--and-performance-tests)
        - [Task 3: Simulate a failure of the CloudShop application](#task-3--simulate-a-failure-of-the-cloudshop-application)
        - [Summary](#summary)
    - [Exercise 5: Explore Azure Security and Operations Management, Application Insights and build a dashboard](#exercise-5--explore-azure-security-and-operations-management--application-insights-and-build-a-dashboard)
        - [Overview](#overview)
        - [Task 1: Work with Log Analytics queries](#task-1--work-with-log-analytics-queries)
        - [Task 2: Preventive maintenance using Security Center](#task-2--preventive-maintenance-using-security-center)
        - [Task 3: Set up an Activity Log alert](#task-3--set-up-an-activity-log-alert)
        - [Task 4: Installing & using the Azure mobile application](#task-4--installing-using-the-azure-mobile-application)
        - [Task 5: Application Insights](#task-5--application-insights)
        - [Summary](#summary)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Overview](#overview)

<!-- /TOC -->

# Azure security and management hands-on lab step-by-step

## Abstract and learning objectives 

In this hands-on lab, you will first deploy a simple web application and database to Azure IaaS VMs, using a Resource Manager Template and Azure Automation DSC. You will then configure a range of infrastructure management capabilities on this deployment, including Update Management, Security Center, Service Map, Change Tracking and Application Insights. You will use Azure Monitor to configure application alerts and send, via both email and mobile application notifications. You will also learn how to further investigate infrastructure status using Log Analytics queries. In doing so, you will learn both how to deploy these solutions and be introduced to their capabilities.

At the end of this hands-on lab, you will be better able to design, implement and use a wide range of infrastructure management systems in Azure.

NOTE: The setup tasks should be completed in advance of the hands-on lab to save deployment time.

## Overview

Contoso Holdings is a multi-national holding company headquartered in Los Angeles, CA that owns 48 manufacturing companies located in North America, Europe and Asia. These companies sell their products primarily to either distributors or large retail organizations around the world. Contoso, as the parent company, controls the IT systems for the companies that it owns and thus runs their e-commerce based applications. There are about 125 of these e-commerce applications used primarily for business-to-business purchasing by corporate buyers. These apps provide the bulk of Contoso's 15 billion dollars in revenue per year, so they are mission critical.

Recently Contoso has started to investigate what it would take to move from on-premises datacenters to the cloud. Most of their applications are ASP.NET running on Windows VMs with SQL Server in a traditional N-tier configuration. Their goal is to lift and shift these applications over to the cloud while gaining more control over the applications and improving their security posture.

They are looking for you to build out a prototype system in Azure using a sample web application they have provided to you called CloudShop.

They are looking for management tools that will allow them to have a full end-to-end view of both the infrastructure and application performance. Their goal will be to effectively lift and shift all applications over to the cloud. They do not have time or money to instrument the applications. Of course, security is on the top of the chain, so they also need a security solution and updated management system.

Per Roberto Milian, VP of Development and IT Operations - "Contoso's primary concern is how to best: deploy, test, manage, monitor, patch, secure and troubleshoot these applications in Azure IaaS."

## Solution architecture

![The Proof of Concept Solution diagram includes Cloud Shop Application and Azure Management and Monitoring.](images/Lab-guide/image2.png "Proof of Concept Solution diagram")

## Requirements

-   A corporate e-mail address (*e.g.*, your \@microsoft.com email)

-   Microsoft Azure subscription must be pay-as-you-go or MSDN

    -   Trial subscriptions will *not* work

-   Local machine or an Azure LABVM virtual machine configured with:

    -   Visual Studio 2017 Community Edition or later

    -   Azure SDK 2.9.+ or Later for Visual Studio

    -   Azure PowerShell 4.0 or later


## Exercise 1: Configure Azure automation

Duration: 15 minutes

### Overview

In this exercise, you will create and configure an Azure Automation account in the Azure Portal which will be used to configure the application servers using Azure DSC.

### Task 1: Create automation account

1.  Browse to the Azure Portal, and authenticate at <https://portal.azure.com/>

2.  Click **+Create a resource,** and type **Automation** in the search box. Choose **Automation** from the results.\
    ![Fields in the Everything blade are set to the previously defined settings.](images/Lab-guide/image21.png "Everything blade")

3.  Select **Create** on the Automation blade. This will display the **Add Automation Account** blade.

4.  On the **Add Automation Account** blade, specify the following information:

    a.  Name: **Automation-Acct**

    b.  Resource group: **HOLRGAUTO** (create a new resource group)

    c.  Location: **East US 2** or **West Europe**

    NOTE: Not all Azure Automation features are supported in all regions. We suggest using East US 2 or West Europe, whichever is closer to you.

    ![Fields in the Add Automation Account blade are set to the previously defined settings.](images/Lab-guide/image22.png "Add Automation Account blade")

5.  Click **Create**



### Task 2: Add an Azure Automation credential

1.  The CloudShopSQL DSC configuration requires a credential object to access the local administrator account on the virtual machine. Within the newly created Azure Automation DSC configuration select **Credentials** in the **SHARED RESOURCES** section.

    ![Under Shared Resources, Credentials is selected.](images/Lab-guide/image24.png "Shared Resources section")

2.  Select the **Add a credential** button

    ![Screenshot of the Add a credential button.](images/Lab-guide/image25.png "Add a credential button")

3.  Specify the following properties and click **Create** to continue.

    a.  Name: **SQLLocalAdmin**

    b.  User Name: **demouser**

    c.  Password & Confirm: **demo\@pass123**

    ![New Credential blade fields are set to the previously defined settings.](images/Lab-guide/image26.png "New Credential blade")

Important: It is important to use the exact name for the credential, because one of the scripts you upload in the next step references the name directly.

### Task 3: Upload DSC configurations into automation account

1.  Select **Resource groups \> HOLRGAUTO \> Automation-Acct** and click **State Configurations (DSC) in Configuration Management** then select **configuration** then selet **Add**

> ![Screenshot of the Automation Account blade.](images/Lab-guide/image27.png "Automation Account blade")

2.  Select the **+ Add a configuration** button

    ![In the Automation Account blade, the Add a configuration button is selected.](images/Lab-guide/image28.png "Automation Account blade")

3.  On the **Import** pane, upload both **C:\\HOL\\CloudShopSQL.ps1** and **C:\\HOL\\CloudShopWeb.ps1** files. You'll need to select **+ Add a configuration** a second time to upload the second file.

    ![Screenshot of the Import blade with fields set to the previously defined settings.](images/Lab-guide/image29.png "Import blade")

4.  After importing the .ps1 files, select the **CloudShopSQL** DSC Configuration. Then, select **Compile** on the toolbar (*click **Yes** on the overwrite prompt*). Do the same for **CloudShopWeb.**

    ![The name field is set to CloudShopSQL in the Automation Account blade.](images/Lab-guide/image30.png "Automation Account blade")

    ![Screenshot of the Configuration blade.](images/Lab-guide/image31.png "Configuration blade")

    ![In the Compile DSC Configuration box, the Yes button is selected.](images/Lab-guide/image32.png "Compile DSC Configuration box")

5.  Make sure to review the DSC configurations to ensure they have completed the compile prior to moving on to the next step

    ![Screenshot of the Configuration blade.](images/Lab-guide/image33.png "Configuration blade")

### Summary

In this exercise, you configured an Automation account, and configured DSC configuration scripts that will be leveraged by the virtual machine resources.

## Exercise 2: Build CloudShop environment

Duration: 60 minutes

### Overview

In this exercise, you will run a template deployment using an ARM template provided which will deploy a Virtual Network, Azure Load balancer, two IIS Servers and a SQL Server. The Servers will check into Azure Automation and run the DSC Configurations that you built in Exercise 1. This will configure the boxes with the CloudShop Application. You will also configure Inbound NAT Rules to allow RDP access to the Web Servers. Azure diagnostics will also be configured into a new storage account for the VMs.

### Task 1: Template deployment

1.  In the portal, open your Azure Automation Account created earlier

2.  Under **Account Settings,** locate the and select **Keys**

    ![Under Account Settings, Keys is selected.](images/Lab-guide/image34.png "Account Settings section")

3.  Open ***Notepad*** and copy both the **Primary Access Key** and the **URL**. These will be needed inputs for the template deployment.

    ![The copy buttons are selected next to the Primary Access Key and URL.](images/Lab-guide/image35.png "Access keys")

    ![The primary key and URL displays in Notepad.](images/Lab-guide/image36.png "Notepad")

4.  In the Azure Portal, select the **+Create a resource** button. In the Search box, type **Template Deployment.**

5.  Select **Template Deployment and** click **Create** on the following screen

    ![Template deployment is circled in the Everything blade.](images/Lab-guide/image37.png "Everything blade")

6.  On the **Custom deployment** screen, select **Build your own template in the editor**



![In the Custom deployment blade, Build your own template in the editor is selected.](images/Lab-guide/image38.png "Custom deployment blade")

7.  Select **Load File**

    ![The Load file button is selected in teh Edit template blade.](images/Lab-guide/image39.png "Edit template blade")

8.  In the Choose File to Upload dialog, navigate to the **C:\\HOL** folder, and locate the **OMSHacakthon-azuredeploy.json** file

    ![Screenshot of the Choose File to Upload dialog box with the previously mentioned path expanded.](images/Lab-guide/image40.png "Choose File to Upload dialog box")

9.  The JSON file will now be in the text window and the Parameters, Variables, and Resources should load in the Window. Select **Save**.

    ![Screenshot of the Edit template blade with JSON code.](images/Lab-guide/image41.png "Edit template blade")

10. Once saved, the window will change to a screen which is asking for inputs. Use the following information to complete:

NOTE: In your student files C:\\HOL\\parameters.txt there is a parameters file that you can use to quickly copy and paste into the portal.

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

aa.  HOL VM2 Admin Password: **demo\@pass123**

bb.  HOL VM2 Windows OS Version: **2016-Datacenter**

cc.  Web AV Set Name: **webAVSet**

<!-- -->

11. Once completed, choose the **I agree to the terms and conditions stated above,** and the **Pin to Dashboard** followed by **Purchase**

    ![The Pin to dashboard checkbox and Purchase button are selected in the Terms and Conditions section.](images/Lab-guide/image42.png "Terms and Conditions section")

12. A Deployment tile will appear on your **My Dashboard**. This deployment should take about 25-30 minutes. The servers will take some time to check in with Azure Automation and configure the CloudShop application.

> ![Screenshot of the Deployment tile.](images/Lab-guide/image43.png "Deployment tile")

NOTE: Wait for the Deployment to successfully complete prior to moving on to the next steps.

13. Now that the servers are built, and the deployment is complete, let's verify the servers are up and running properly...

14. In the Azure Portal, open the **HOLRG,** and locate the Public IP Address **hackathonPublicIP**. Click on the blade to open.

> ![In the Azure Portal, in the Search results list, hackathonPublicIP is selected.](images/Lab-guide/image44.png "Azure Portal")

15. On the **hackathonPublicIP** blade, locate the DNS Name you provided during the template deployment. If you hover your mouse over the name, you can **click to copy** the name to the clipboard. Paste this into your Notepad document you opened earlier.

    ![The DNS name is selected in the HackathonPublicIP blade.](images/Lab-guide/image45.png "HackathonPublicIP blade")

16. Open a new tab in Internet Explorer and paste the URL. This is the DNS name that is attached to the Azure Load Balancer in front of the CloudShop Application web servers **WEBVM1** & **WEBVM2**.

17. When the page loads, the CloudShop application should appear, and it will show which VM is serving the web page. In this screen capture, we see it is running on **WEBVM2**.

    ![Screenshot of the Cloud Shop webpage with a callout pointing to the WEBVM2 virtual machine name.](images/Lab-guide/image46.png "Cloud Shop webpage")

18. By pressing **F5** on your keyboard, you can refresh the website until you see that **WEBVM1** is also serving webpages

    ![Screenshot of the Cloud Shop webpage with a callout pointing to the WEBVM1 virtual machine name.](images/Lab-guide/image47.png "Cloud Shop webpage")

### Task 2: Allow remote desktop to the WEBVM1 & WEBVM2 using NAT rules

Now that the deployment and the application is up and running, the next step is to allow RDP to the Web Servers. This will be accomplished by building NAT Rules through the Azure Load Balancer.

1.  Open the **HOLRG** Resource Group and locate **loadBalancer1**. Click to open its administration blade.

    ![In the Resource group blade, under Name, loadBalancer1 is selected.](images/Lab-guide/image48.png "Resource group blade")

2.  On the **loadBalancer1** blade, locate **Inbound NAT rules in Settings**

    ![In the LoadBalancer 1 blade, under Settings, Inbound NAT rules is selected.](images/Lab-guide/image49.png "LoadBalancer 1 blade")

3.  Click **+Add,** and complete the blade using the following information:

    a.  Name: **rdp-webvm1**

    b.  Frontend IP Address: **accept default**

    c.  Service: **RDP**

    d.  Port: **3389**

    e.  Associated to: **webavset**

    f.  Target: **Choose a virtual machine: WEBVM1**

    g.  Network IP configuration: **ipconfig1 (10.0.0.4)**

    h.  Port mapping: **Default**

    ![Add inbound NAT rule blade fields are set to the previously defined settings.](images/Lab-guide/image50.png "Add inbound NAT rule blade")

4.  The portal will give a notice that it is: **"Saving load balancer inbound NAT rule**". Wait until this completes before continuing.

    ![Screenshot of the Saving load balancer inbound NAT rule notice.](images/Lab-guide/image51.png "Saving load balancer inbound NAT rule notice")

    ![Screenshot of the Saved load balancer inbound NAT rule message.](images/Lab-guide/image52.png "Saved load balancer inbound NAT rule message")

5.  Click **+Add,** and complete the blade using the following information:

    a.  Name: **rdp-webvm2**

    b.  Frontend IP Address: **accept default**

    c.  Service: **RDP**

    d.  Port: **3390**

    e.  Target **Choose a virtual machine: WEBVM2**

    f.  Network IP configuration: **ipconfig1 (10.0.0.5)**

    g.  Port mapping: **Custom**

    h.  Floating IP: **Disabled**

    i.  Target Port: **3389**

    ![Add inbound NAT rule blade fields are set to the previously defined settings.](images/Lab-guide/image53.png "Add inbound NAT rule blade")

6.  The portal will give a notice that it is: **"Saving load balancer inbound NAT rule**". Wait until this completes before continuing.

    ![Screenshot of the Saving load balancer inbound NAT rule notice.](images/Lab-guide/image51.png "Saving load balancer inbound NAT rule notice")

    ![Screenshot of the Saved load balancer inbound NAT rule message.](images/Lab-guide/image54.png "Saved load balancer inbound NAT rule message")

7.  To verify the new NAT Rules are working, move back to the **HOLRG** in the Azure portal. Select **WEBVM1,** and the **Connect** Link should now be available. Select **Connect**.

    ![The Connect button is selected in the Virtual machine blade.](images/Lab-guide/image55.png "Virtual machine blade")

8.  On the 'Connect to virtual machine' blade, check the **RDP** tab is selected and choose **Download RDP File**

    ![The Download RDP File button is highlighted on the RDP tab of the Connect to virtual machine blade](images/Lab-guide/image55b.png "Connect to virtual machine blade")
  
8.  Select **Open** when the RDP file downloads

    ![The Open button is selected in the Open or Save message.](images/Lab-guide/image56.png "Open or Save option")

9.  You will get a warning about the publisher of the RDP file being unknown. Select **Don't ask me again for connections to this computer** and click **Connect.**

    ![In the Remote Desktop Connection dialog box, the Don\'t ask me again checkbox and the Connect button are both selected.](images/Lab-guide/image57.png "Remote Desktop Connection dialog box")

10. When prompted by Windows Security, enter your credentials:

    a.  User Name: **demouser**

    b.  Password: **demo\@pass123**

        ![Screenshot of the Windows Security prompt.](images/Lab-guide/image58.png "Windows Security prompt")

11. A warning will appear stating: **The identity of the remote computer cannot be verified. Do you want to connect anyway?** Select the checkbox for the disclaimer: **Don't ask me again for connection to this computer.** Then, select **Yes**.

    ![The Remote Desktop Connection warning dialog box displays with the Don\'t ask me again checkbox and the Yes button selected.](images/Lab-guide/image59.png "Remote Desktop Connection warning dialog box ")

NOTE: When connecting to machines during this lab for the first time, you may encounter the same warnings etc. Follow these same steps to no longer receive those warnings as they do not apply to our setup.

12. When logging on for the first time, you will see a prompt on the right asking about network discovery. Select **No**.

	![The No button is selected in the Networks prompt.](images/Setup/image10.png "Networks prompt")

13. Notice the Server Manager opens by default. On the left, select **Local Server**

![Screenshot of the Local Server option.](images/Setup/image11.png "Local Server option")

14. On the right side of the pane, select **On** by **IE Enhanced Security Configuration**

![The IE Enhanced Security Configuration is set to On.](images/Setup/image12.png "IE Enhanced Security Configuration setting")

15. Change to **Off** for Administrators and select **OK**

![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box.](images/Setup/image13.png "Internet Explorer Enhanced Security Configuration dialog box")

16. In the lower left corner, select the **Windows** button to open the **Start Screen**. Then, **Internet Explorer** to open it. On first use, you will be prompted about security settings. Accept the defaults by selecting **OK**.

![Screenshot of the Internet Explorer 11 security settings dialog box with Use recommended settings selected.](images/Setup/image14.png "Internet Explorer 11 security settings dialog box")

17. Leave your RDP Session to **WEBVM1** open and minimized. Then, repeat this same procedure for **WEBVM2**.

### Task 3: Configure diagnostics accounts for the VMs

In this task, you will configure the VMs to capture diagnostic data in an Azure Storage Account. Later you will connect this account to the Azure Security Center and Log Analytics.

1.  In the Azure Portal navigate to the **HOLRG** Resource Group and locate **WEBVM1**. Select the name to open the blade.

2.  On the **WEBVM1,** locate the **Monitoring** section, and select **Diagnostic Settings**

> ![On the WEBVM1 blade under Monitoring, Diagnostic settings is selected.](images/Lab-guide/image60.png "WEBVM1 blade")

3.  Select the **Enable guest-level monitoring** button. This will load more information to choose from. Select the Storage Account Configure Required Settings, and then, choose the Storage Account you just created.

    ![On the Overview tab, the Enable guest-level monitoring button is selected.](images/Lab-guide/image61.png "Overview tab")

    ![Screenshot of the Updating diagnostics settings notification.](images/Lab-guide/image62.png "Updating diagnostics settings notification")

4.  Select the Configure performance counters

    ![Under Performance counters, the Configure performance counters link is selected.](images/Lab-guide/image63.png "Performance counters section")

5.  Next check the **ASP.NET box,** and select **Save**

    ![On the Performance counters tab, the ASP.NET checkbox is called out.](images/Lab-guide/image64.png "Performance counters tab")

6.  This will submit a deployment for **WEBVM1**

    ![Screenshot of the Updating diagnostics settings notice.](images/Lab-guide/image62.png "Updating diagnostics settings notice")

7.  Complete the same steps for **WEBVM2**

8.  Next, using the same steps, configure **SQLVM** for Diagnostics as well, Select the following metrics for this SQL Server, and select **Save**.

    a.  SQL Metrics

    ![On the Performance counters tab, the SQL Server checkbox is called out.](images/Lab-guide/image65.png "Performance counters tab")

NOTE: You will need to wait for the portal to complete the updates to all VMs before moving to the next exercise.

### Summary

In this exercise, you ran a template deployment using an ARM template provided which created a Virtual Network, Azure Load balancer, two IIS Servers and a SQL Server. The servers checked into Azure Automation and ran the DSC Configurations that configured the boxes with the CloudShop Application. You then configured Inbound NAT Rules to allow RDP access to the Web Servers and successfully connected. Azure diagnostics was also configured into a new storage account for the VMs.

## Exercise 3: Build and configure Azure Security Center and Azure Management

Duration: 30 minutes

### Overview

The next step is to provision the Azure security and Azure management components of Azure Automation, configure the VMs for the CloudShop application to be managed by the portal, and configure the diagnostics storage account to load data into the Log Analytics platform. Additionally, you will configure Update Management, Inventory Tracking and Change Management as well as install and configure the Service Map solution pack.

### Task 1: Provision Log Analytics through Azure Monitor

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking **All services**, searching for "*Log Analytics*", and selecting **Monitor**

> ![Screenshot of the Azure portal with the previous selections displaying.](images/Lab-guide/image66.png "Azure portal")

2.  In the Log Analytics blade, select **+Add**

3.  Complete the OMS Workspace blade using the following information. Then, select  **OK**:

    a.  OMS Workspace: **Unique name**

    b.  Subscription: **Select the current subscription**

    c.  Resource Group: **HOLRG**

    d.  Location: **Closest to your deployment**

    e.  Pricing Tier: **Select Per GB**

    ![Fields in the OMS Workspace and Pricing Tier blades are set to the prevoiusly defined settings.](images/Lab-guide/image68.png "OMS Workspace and Pricing Tier blades")

4.  The deployment will only take a few moments to complete. Upon completion, open **Log Analytics** by clicking on the Log Analytics resource within the **HOLRG** resource group

    ![Screenshot of the Log Analytics resource.](images/Lab-guide/image69.png "Log Analytics resource")

5.  Once it loads in the Azure Portal, move the slider bar down, and select **Virtual Machines** which is found in the **Workspace Data Sources** section

    ![Under Workspace Data Sources, Virtual machines is selected.](images/Lab-guide/image70.png "Workspace Data Sources section")

6.  A list of the VMs in your subscription will be shown in the list. You may want to filter your view to see the VMs for this HOL. You will add the WEB and SQL VMs.

    ![In the list of virtual machines, SQLVM, WEBVM1, and WEBVM2 are called out.](images/Lab-guide/image71.png "Virtual machines list")

7.  Select the **SQLVM** to load a blade to the right. Then, click on **Connect** to add it to this Log Analytics Workspace.

    ![The Connect button is selected in the SQLVM blade.](images/Lab-guide/image72.png "SQLVM blade")

8.  Follow the same steps for the **WEBVM1** & **WEBVM2**

9.  The portal should update to show that they are now a part of "This workspace" once they have all been added

    ![SQLVM, WEBVM1, and WEBVM2 display in the portal.](images/Lab-guide/image73.png "Portal")

### Task 2: Explore Security Center

1.  Open the Azure portal and navigate to the **Security Center** menu option (**All Services Security Center**)

    ![Within the Azure Portal, Security Center is selected.](images/Lab-guide/image74.png "Azure Portal")

2.  This will present the **Security Center - Overview** screen. Notice that it's already collecting data. For this exercise, you want to upgrade to Standard tier which extends the capabilities of the Free tier to workloads running in private and other public clouds. It provides unified security management and thread protection across your hybrid cloud workloads. The Standard tier also adds advanced thread detection capabilities, which uses built-in behavioral analytics and machine learning to identify attacks and zero-day exploits, access and application controls to reduce exposure to network attacks and malware, and more.

    ![Screenshot of the Security Center - Overview screen.](images/Lab-guide/image75.png "Security Center - Overview screen")

3.  Select **Onboard to Advanced Security**

4.  Select the subscription you would like to upgrade by clicking on the subscription name

> ![Screenshot of the Enable advanced security for subscriptions page with a subscription selected.](images/Lab-guide/image76.png "Enable advanced security for subscriptions page")

5.  Select the **Standard** pricing tier and click on **Save**

    ![Screenshot of the Pricing blade with the Standard option and the Save button selected.](images/Lab-guide/image77.png "Pricing blade")

6.  Close the panel and navigate back to the **Security Center Overview** screen. Click on **Security policy** under the **General** section. This is where you will enable data collection.

    ![Under General, Security policy is selected.](images/Lab-guide/image78.png "General section")

7.  This brings up the **Security policy** screen where you will click on your subscription name which is currently in an Off state for data collection.

    ![Screenshot of the Demo subscription name, with Automatic provisioning set to Off.](images/Lab-guide/image79.png "Subscription name")

8.  This presents the **Data Collection** screen. Turn on data collection by clicking the **On** button, selecting **Use another workspace** and selecting the Log Analytics workspace created in [Task 1: Provision Log Analytics](#task-1-provision-log-analytics-through-azure-monitor) and then clicking **Save** and click on Yes if prompted.

    ![Screenshot of the Data Collection screen with the previously defined settings.](images/Lab-guide/image80.png "Data Collection screen")

### Task 3: Add Service Map

In this section, you will add Service Map to Log Analytics.

1.  From the Azure portal, click the **+Create a resource** Link followed by **Management Tools** and **See all**

    ![Create a resource > Management Tools > See All click path screenshot.](images/Lab-guide/image81.png "Create a resource > Management Tools > See all")
        
2.  Note there are many solutions available, and more are added frequently. Browse the solutions to gain familiarity with the options.

    ![Screenshot of the Dynatrace homepage.](images/Lab-guide/image83.png "Dynatrace homepage")

3.  Locate the **Service Map** solution and select it. If you don't see it on the page, use the *Search Monitoring + Management* field at the top of the screen to search for **Service Map**. On the Details page, you can read about the solution. When ready, select **Create.**

    ![In the Monitoring and Management blade, in the search results, Service Map is selected.](images/Lab-guide/image84.png "Monitoring and Management blade")

4.  Select the OMS Workspace you created and check the **Pin to dashboard** before selecting **Create**

    ![Screenshot of the Service Map blade.](images/Lab-guide/image85.png "Service Map blade")

### Task 4: Configure Service Map

To configure the Service Map functionality, the Microsoft Dependency Agent needs to be installed on each virtual machine.
This can be installed as a VM extension, however this VM extension is not currently available via the Azure portal.
For this lab, we'll install the VM extension using the Azure CLI.

1.  Open the Azure Cloud Shell by clicking on the 'Cloud Shell' icon

    ![Screenshot of Cloud Shell button](images/Lab-guide/image86.png "Cloud Shell button")

	NOTE: If this is your first time using the Azure Cloud Shell, you will be prompted to choose between a Bash Shell and PowerShell. Choose **Bash Shell**. Also, you will be prompted to create a storage account for use by the Cloud Shell.

2.  The Cloud Shell will open at the bottom of the Azure portal browser window. (If a PowerShell has been selected, change to the **Bash Shell**.) In the Bash Shell window, type the following command:

	```
	az vm extension set --publisher Microsoft.Azure.Monitoring.DependencyAgent --version 9.1 --name DependencyAgentWindows --vm-name WEBVM1 --resource-group HOLRG
	```

	This command will take a minute or two to run.

3.  Repeat the above command two more times, replacing the `--vm-name` parameter with **WEBVM2** and then with **SQLVM**

4.  In the Azure Portal, navigate to the **All Resources** menu and locate the **Service Map** resource you created earlier. Click on the resource name.

    ![Screenshot of the All resources blade with ServiceMap selected.](images/Lab-guide/image88.png "All resources blade")

5.  This will bring up the Service Map screen and you'll see that it's already populated with some data. Since we installed the agent on the three Windows virtual machines, the Service Map is showing these virtual machines now reporting data. Click on the **Service Map** tile.

    ![The Summary section displays information for both the Service Map and Solution Resources.](images/Lab-guide/image89.png "Service Map Summary section")

6.  When the Service Map loads, click on **WEBVM1** to see the data that has been analyzed for that virtual machine.

    ![Screenshot of the Service Map for WEBVM1.](images/Lab-guide/image90.png "Service Map")

### Task 5: Configure Update Management

The Update Management functionality will be configured through your Virtual Machines.

1.  In the **Azure Portal**, browse to **WEBVM1**

2.  Select **Update management** under **OPERATIONS**

    ![Under Operations, Update management is selected.](images/Lab-guide/image91.png "Operations section")

3.  Select your **Log Analytics workspace** and **Automation Account** and select **Enable**

    ![Screenshot of the Update Management window with the Enable button selected.](images/Lab-guide/image92.png "Update Management window")

	NOTE: In some cases, your Log Analytics workspace may not be shown if it is located in a different region or geography. In this case, use the option provided to create a new Log Analytics workspace.

4.  Wait for the deployment to complete. This can take up to 15 minutes.

    ![Screenshot of the Update Manager being enabled message.](images/Lab-guide/image93.png "Update Manager being enabled message")

    Do not navigate away from the Update Management blade until the deployment message reads "The 'Update Management' solution is being deployed on this virtual machine. This can take a few minutes. You can do other work while this is in progress."

5.  While the solution is deploying, navigate to **WEBVM2** and repeat steps 2-4

6.  While the solution is deploying, navigate to **SQLVM** and repeat steps 2-4

7.  Verify that the solution has been deployed by navigating to **WEBVM1** and clicking on **Update management** under **OPERATIONS**

    ![Under Operations, Update management is selected.](images/Lab-guide/image91.png "Operations section")

8.  The Portal shows the Update Management dashboard for the virtual machine. Here you can see which updates are pending, and the overall update compliance status of the VM.

    ![Missing updates shows as zero.](images/Lab-guide/image94.png "Missing updates message")

9.  Click on **Schedule update deployment**. The portal shows the 'New update deployment' blade, where you can choose which updates to deploy (based on classification), which to exclude (based on KnowledgeBase ID), when to deploy, and the duration of the maintenance window (updates not deployed within 20 minutes of the end of this time window are omitted so the VM can reboot.)

     ![The New Update Deployment blade shows the settings described in the preceeding text.](images/Lab-guide/image186.png "New update deployment")

    Review the update settings, but do *not* configure an update deployment (to save time during the lab). Close the New update deployment blade.

10.  Select on **Manage multiple machines**. This brings you to the Update Management view within the Azure Automation portal experience. This gives you an overview of update compliance across all of your VMs.

     ![Screenshot of the Log Analytics Update Managemnent experience. The VMs we have created are listed, together with metrics showing the number of updates pending/failed.](images/Lab-guide/image188.png "Update Management experience")


11.  Select **New update deployment** brings a similar update deployment blade, except in this case you can configure the update deployment to be either Windows or Linux (previously the OS was inferred from the VM), and select which VMs will be updated (previously the blade applied only to a single VM).

    ![The New Update Deployment blade shows the settings described in the preceeding text.](images/Lab-guide/image187.png "New update deployment")

    Updating VMs via this Log Analytics experience enables central management of updates.
    

### Task 6: Configure Inventory Tracking and Change Management

The Update Management functionality will be configured through Azure Automation.

1.  In the **Azure Portal**, browse to **WEBVM1**

2.  Select **Change tracking** under **OPERATIONS**

    ![Under Operations, Change tracking is selected.](images/Lab-guide/image95.png "Operations section")

3.  Verify the **Log Analytics workspace** and **Automation Account** and select **Enable**

> ![The Enable button is selected in the Change Tracking window.](images/Lab-guide/image96.png "Change Tracking window")

4.  Wait for the deployment to complete. This can take a few minutes.

> ![Screenshot of the Change Tracking being enabled message.](images/Lab-guide/image97.png "Change Tracking being enabled message")

Do not navigate away from the Update Management blade until the deployment message reads "The 'Change Tracking and Inventory' solution is being deployed on this virtual machine. This can take a few minutes. You can do other work while this is in progress."

5.  While the solution is deploying, navigate to **WEBVM2** and repeat steps 2-4

6.  While the solution is deploying, navigate to **SQLVM** and repeat steps 2-4

7.  Verify that the solution has been deployed by navigating to **WEBVM1** and clicking on **Change tracking** under OPERATIONS

    ![Under Operations, Change tracking is selected.](images/Lab-guide/image95.png "Operations section")

    ![Changes shows as zero.](images/Lab-guide/image98.png "Changes status")

### Summary

In this exercise, you provisioned the portal, configured the VMs for the CloudShop application to be managed by the Portal, and configured the diagnostics storage account to load data into the Log Analytics platform. Additionally, solution packs were installed and configured to gather data and provide dashboards for applications deployed in Azure IaaS. You also configured Service Map and now understand how it surfaces data.

## Exercise 4: Instrument CloudShop using Azure Application Insights

Duration: 45 minutes

### Overview

In this exercise, you will instrument the CloudShop using Application Insights at runtime. This will be accomplished by installing the Applications Insights tool on the web services and configuring an Application Insight workspace in Azure. Then, you will configure Application Insights to perform web tests and alerts. The final task will be to connect the Application Insights workspace to send data to the Portal.

### Task 1: Install and Configure the Application Insights Status Monitor

To read more about this tool follow this link: <http://bit.ly/2ksdzKV>

1.  Open a Remote Desktop Connection to **WEBVM1**

2.  Open Internet Explorer and follow this link: <http://bit.ly/2jxQ43z>. Select **Run** on the Question if you want to run the file: **AppliationsInsightsMonitor.exe**.

    ![The Run button is selected next to the question asking if you want to run or save the file.](images/Lab-guide/image99.png "Run or Save option")

3.  This will start the Web Platform Installer. Select **Install** followed by **I Accept** on the following screen, and **Continue**.

    ![Screenshot of the Web Platform installer with the Install button selected.](images/Lab-guide/image100.png "Web Platform installer")

4.  Once the Monitor is installed, select the **Sign In** link under the **Configuration**

    ![Sign in is selected in the Application Insights Status Monitor window.](images/Lab-guide/image101.png "Application Insights Status Monitor window")

5.  You will sign-in to Azure as normal

    ![Screenshot of the Azure sign in box.](images/Lab-guide/image102.png "Azure sign in box")

6.  Under the **Send telemetry to**: Select **Default website under New Application Insights resource**. Then, click **Configure settings**.

    ![Under Configuration, Send telemetry to is set to New Application Insights resource and the Configure settings button is selected.](images/Lab-guide/image103.png "Configuration section")

7.  On the **Configuration settings for Application Insights**, complete the information as follows, and select **OK**

    a.  Microsoft Azure Subscriptions: **Use the same subscription**

    b.  Resource Groups: **HOLInsights**

    c.  Application Insights Resource: **HOLCloudShop**

    d.  Location: **Select the same region as your deployment**

        ![Fields in the Configuration settings for Application Insights dialog box are set to the previously defined settings.](images/Lab-guide/image104.png "Configuration settings dialog box")

8.  This will build the Application Insights workspace for you in Azure

9.  Next, select **Add Application Insights**

    ![In the Default Web Site section, the Add Application Insights button is selected.](images/Lab-guide/image105.png "Default Web Site section")

10. Click **Restart IIS** to complete the Setup

    ![Screenshot of the Restart IIS button.](images/Lab-guide/image106.png "Restart IIS button")

11. This will only take a few seconds. Now, the CloudShop application running on **WEBVM1** is instrumented and sending data to **Azure Application Insights**. The monitor on **WEBVM1** should now look like below:

    ![IIS applications show as enabled in the Application Insights Status Monitor, and under Default Web Site, Status is Application Insights enabled, and Data is set to the specified Application Insights resource.](images/Lab-guide/image107.png "Application Insights Status Monitor")

12. Disconnect from **WEBVM1**

13. Connect to a Remote Desktop Session for **WEBVM2**. If you RDP to the public IP it may take you to the WEBVM1 again because of Loadbalancer rule. So in order to connect to the WEBVM2, You can RDP to its private IP from the WEBVM1. 

14. Open Internet Explorer and follow this link: <http://bit.ly/2jxQ43z>. Select **Run** on the Question if you want to run the file: **AppliationsInsightsMonitor.exe**. You may need to change the internet explorer security settings to allow download file in order to install.

    ![Screenshot of the Run button.](images/Lab-guide/image99.png "Run button")

15. This will start the Web Platform Installer. Click on **Install** followed by **I Accept** on the following screen, and **Continue**.

    ![On the Web Platform Installer, under Application Insights Status Monitor, the Install button is selected.](images/Lab-guide/image100.png "Web Platform Installer")

16. Once the Monitor is installed, select the **Sign In** link under the **Configuration**

    ![In the Application Insights Status Monitor, Sign in is selected.](images/Lab-guide/image101.png "Application Insights Status Monitor ")

17. You will sign-in to Azure as normal

> ![Screenshot of the Microsoft Sign in box.](images/Lab-guide/image102.png "Microsoft Sign in box")

18. Under the **Send telemetry to**: Select **Default website under Existing Application Insights resource**. Then, click **Configure Settings**.

    ![Send telemetry to is set to Existing Application Insights resource, and Configure settings is selected.](images/Lab-guide/image108.png "Send telemetry to section")

19. On the **Configuration settings for Application Insights**, complete the information as follows, and select **OK**

    a.  Microsoft Azure Subscriptions: **Use the same subscription**

    b.  Resource Groups: **HOLInsights**

    c.  Application Insights Resource: **HOLCloudShop**

    d.  Location: **Select the same region as your deployment**

        ![Fields in the Configuration Settings for Application Insights dialog box are set to the previously defined settings.](images/Lab-guide/image109.png "Configuration Settings for Application Insights dialog box")

20. This will attach **WEBVM2** to the Application Insights workspace you created a moment ago in Azure. Next, select **Add** **Application Insights**.

    ![Under Default Web Site, the Add Application Insights button is selected.](images/Lab-guide/image110.png "Default Web Site section")

21. Select **Restart IIS** to complete the Setup

    ![Screenshot of the Restart IIS button.](images/Lab-guide/image106.png "Restart IIS button")

    This will only take a few seconds. You can disconnect from WEBVM2 when done.

### Task 2: Explore the Application Map, configure alerts, availability tests, and performance tests

1.  Open the Application Insights blade by clicking **All services**, followed by **Application Insights** (use the search filter to help you find it)

    ![Screenshot showing click path to open Applicaiton Insights.](images/Lab-guide/image111.png "Open Application Insights")

2.  Select **HOLCloudShop**, followed by **Application map**

    ![Screenshot showing clicks for HOLCloudShop (1) and Application map (2).](images/Lab-guide/image112.png "Open HOLCloudShop Application Map")


3.  Take a few minutes to explore the Application map. Click on each node in the application map, and look at the data available.

	![Screenshot of the Application Map.](images/Lab-guide/image114.png "Application Map")

4.  Close the **Application Map** pane, then Click on the **Alerts** in the **Configure** section of the Application Insights blade.

    ![Under Configure, Alerts (Classic) is selected.](images/Lab-guide/image115.png "Alerts (Classic) selection")

5.  Select **View Classic Alert (next to the bell icon)** then select **+Add Metric Alert (Classic),** complete the blade with the following information, and select **OK**

    a.  Name: **CloudShopProcessorAlert**

    b.  Metric: **Processor Time**

    c.  Condition: **Greater than**

    d.  Threshold: **80**

    e.  Period: **Over the last 5 minutes**

    f.  Notify via: Email owners, contributors and readers -- **Check the Box**

        ![Add rule blade fields are set to the previously defined settings.](images/Lab-guide/image116.png "Add rule blade")

        ![Screenshot of the rest of the Add rule blade fields.](images/Lab-guide/image117.png "Add rule blade fields - continued")

6.  Select **OK** to save the alert.  The portal will update with the new Alert.

    ![Alert settings for CloudShopProcessorAlert display.](images/Lab-guide/image118.png "Alert settings")

7.  In your **HOLRG,** locate the **hackathonPublicIP** Public IP Address, take note of the DNS name which is on the front of the Azure Load Balancer for the CloudShop App running on **WEBVM1** & **WEBVM2**

8.  Next in the **HOLCloudShop** Application Insights workspace, under the **Investigate** section, select **Availability**

    ![Under Investigate, Availability is selected.](images/Lab-guide/image119.png "Investigate section")

9.  Select **+Add test**, and complete the blade using the following information. Then, select **Create**.

    a.  Test name: **CloudShopWebTest**

    b.  Test type: **URL Ping Test**

    c.  URL: **http://HOLXXXXXX.southcentralus.cloudapp.azure.com**

    d.  Test locations: Choose locations from all over the world

    e.  All other accept defaults

    ![Create test blade fields are set to the previously defined settings.](images/Lab-guide/image120.png "Create test blade")

10.  Select **Create** to crate the availability test

	NOTE: If the CloudShop Application becomes unavailable to this WebTest, you will then receive an email alert from Azure Application Insights.

11.  Select **Performance Testing** in the **Configure** section

![Under Configure, Performance Testing is selected.](images/Lab-guide/image121.png "Configure section")

12.  Select **+New**

13.  Click on **Configure Test Using** and complete this using these inputs. Then, select **Done**.

    a.  Test Type: Manual Test

    b.  URL: **http://HOLXXXXXX.southcentralus.cloudapp.azure.com**

![Fields in the Configure test using blade are set to the previously defined settings.](images/Lab-guide/image122.png "Configure test using blade")

14.  Complete the **New Performance Test** blade using the following information, and click **Run Test**.

    a.  Name: **CloudShopLoadTest**

    b.  Generate Load from: **Select a Region**

    c.  User Load: **2000**

    d.  Duration: **5**

![Fields in the New performance test blade are set to the previously defined settings.](images/Lab-guide/image123.png "New performance test blade")

15.  Select **Run Test** to start the performance test

	NOTE: An error may occur if you do not have a Visual Studio Team Services (VSTS) Account configured. If so, then you'll need to create one before setting up the Performance Test.

16.  Once this is submitted it will show as **Queued.** Select the line and then details about the performance test will be shown.

![CloudShopLoadTest is selected under Recent runs in the Performance Testing blade.](images/Lab-guide/image124.png "Performance Testing blade")

17.  Select the Messages box to see the details of the test

	![Screenshot of the CloudShopLoadTest and Status Messages blades. In the CloudShopLoadTest blade, the Messages box is selected.](images/Lab-guide/image125.png "CloudShopLoadTest and Status Messages blades")

18.  Now, head back to the Overview blade of the **HOLCloudShop** Application Insights and select **Live Metrics Stream**

![The Live Stream tile lists two servers.](images/Lab-guide/image126.png "Live Stream tile")

19.  Real-time application information can be seen regarding the CloudShop App running in Azure on our IaaS VMs. Here, you can wait for the Performance Test to run, and show how the Web Application performs.

![Screenshot of the Live Metrics Stream page, with Incoming Requests, Outgoing Requests, and Overall Health, line and scatter graphs, and Server information.](images/Lab-guide/image127.png "Live Metrics Stream page")

20.  If you go back to the **Performance Testing** blade, and click on **CloudShopLoadTest,** you will see the metrics form the run

![Screenshot of Performance under load metrics.](images/Lab-guide/image128.png "Performance under load metrics")![Screenshot of the Requests donut chart.](images/Lab-guide/image129.png "Requests graph")

21.  Close the Performance Test, and click on the **Performance** under Investigate

![Under Investigate, Performance (preview) is selected.](images/Lab-guide/image130.png "Investigate section")

22.  Explore the metrics from the CloudShop Application

![Screenshot of the CloudShop Application desktop.](images/Lab-guide/image131.png "CloudShop Application desktop")

23.  The Load Test should also have caused the alert on high processor usage to be trigged. An email should have been received.

![Screenshot of an Azure Application Insights warning alert.](images/Lab-guide/image132.png "Azure Application Insights alert")

	NOTE: The email may take several minutes to arrive. You can proceed with the lab and check for the email later.

24.  The alert will quickly resolve as the Load Test has completed causing the CPU condition to quiet

![Screenshot of an Azure Application Insights success message.](images/Lab-guide/image133.png "Azure Application Insights success message")

### Task 3: Simulate a failure of the CloudShop application

1.  Move to your **HOLRG** Resource group and stop both **WEBVM1** and **WEBVM2**

2.  Navigate back to the **HOLCloudShop** Application Insights portal. Select **Availability**, and notice that the availability tests have start to fail once the web VMs are stopped.

    ![On the Application Insights blade, the Availability blade shows the web tests failing](images/Lab-guide/image134.png "Application Insights Availability blade")

3.  Select **CloudShopWebTest** to open the test summary blade. Notice how the tests are failing from all regions.

    ![Screenshot showing CloudShopWebTest details chart and test location status.](images/Lab-guide/image135.png "CloudShopWebTest summary")

5.  A few email alerts should come into your inbox:

    ![Azure Application Insights warning alert screenshot](images/Lab-guide/image137.png "Azure Application Insights warning alert")

    ![Azure Application Insights details screenshot](images/Lab-guide/image138.png "Azure Application Insights details")

6.  Select the "See the analysis of this issue" which will load the Azure portal

    ![Both the Smart Detection and An abnormal rise in failed request rate blades display.](images/Lab-guide/image139.png "Smart Detection and An abnormal rise in failed request rate blades")

7.  Move back to your **HOLRG and** restart the VMs

8.  Once the VMs are back online, the website will come back up. This will initiate responses to the Web Test and sending data to the Applications Insights portal. An email will be sent resolving the Alert. After a period of time, the Smart Detection Alert will also resolve.

    ![Screenshot of the Azure Application Insights success message.](images/Lab-guide/image140.png "Azure Application Insights success message")

### Summary

In this exercise, you instrumented the CloudShop using Application Insights at runtime. This was accomplished by installing the Applications Insights Monitor for the web services and configuring an Application Insight workspace in Azure. Then, you configured Application Insights to perform web tests and alerts.

## Exercise 5: Explore Azure Security and Operations Management, Application Insights and build a dashboard

Duration: 45 minutes

### Overview

In this exercise, you will explore the information and data being provided by Azure Security and Operations Management and Application Insights to gain situational awareness of the application and infrastructure. You will look at the Security posture of the infrastructure, the applications performance, and build a dashboard that can be used to manage it moving forward.

### Task 1: Work with Log Analytics queries

In this section, we will perform an ad-hoc search in Log Analytics data to see where our servers are not in compliance with security baselines. In the Log Search interface, we can perform ad-hoc searches against the log data being ingested into the Log Analytics service. Because the data is indexed, searching is very fast.

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking on **All services**, searching for "*log analytics*", and selecting **Log Analytics**

    ![Selections in the Azure Portal display as previously mentioned.](images/Lab-guide/image66.png "Azure Portal")

2.  Select **Log** from the left under **General**

    ![Under Shared Services, Log Analytics is selected.](images/Lab-guide/image141.png "Shared Services section")

3.  In the query editor, enter the following query: *Update \| where OSType!=\"Linux\" and Optional==false*. Select Run. This will search the Update management logs and report results. There are many other data sources we can query.

    ![Screenshot of the Log Search blade.](images/Lab-guide/image142.png "Log Search blade")

    NOTE: you may have less or more data, but keep in mind, the service has only been collecting data since you began the lab.

4.  Notice in the left, there are different types of data. Select **Windows Defender** followed by **Apply**.

    ![Under product, both the Windows Defender checkbox and the Apply button are selected.](images/Lab-guide/image143.png "Product section")

5.  Notice the query dialog (where you entered a search before) has a search string in it querying for the type "Product==Windows Defender"

    ![An updated search string displays in the Query dialog box.](images/Lab-guide/image144.png "Query dialog box")

6.  As you click on options in Log Search, queries are built automatically. Also, notice the results are paired down to a smaller subset of data. Click on **Table** to view the data in a column and row format.

    ![A Table of search results displays.](images/Lab-guide/image145.png "Table")

7.  This is a list of Windows virtual machines tracked in Update management that have pending updates for Windows Defender. To further refine the list, scroll on the left down to **UPDATESTATE** and select **Needed** (this option may not be shown if all updates are already installed). Notice the query automatically updates as it is a single point to refine.

    ![An updated search string displays in the Query dialog box. Below, in the Table, a callout points out that UpdateState for both table entries displays as Needed.](images/Lab-guide/image146.png "Qyery dialog box and Table")

8.  Further refinements can be made as needed. Select additional refiners to update the search.

9.  Next, sort the findings, so the Computers are sorted alphabetically. Click on the column header **Computer** until the virtual machines are sorted correctly.

    ![In the Table, on the header row, Computer is selected.](images/Lab-guide/image147.png "Table")

10. Now we will export the list. This may be helpful if we wanted to divide the findings list out amongst administrators, so several people can help remediate the findings. Click on **Export**.

    ![On the Log Search blade top menu, Export is selected.](images/Lab-guide/image148.png "Log Search blade top menu")

    ![The Exporting results to Excel message displays.](images/Lab-guide/image149.png "Exporting results message")

11. Once the report is exported, you are prompted what to do with the file. Select **Save**. Once completed, click on **Open**. Choose **Notepad** to open the file**.

   ![The first message asking if you want to save SearchResults.csv has the Save button selected. The second message, download has completed has the Open button selected. The third message asks how you want to open the file, and Notepad is selected.](images/Lab-guide/image150.png "Multiple messages")

12. Review the text file. Then, close it. You can also copy the file to your local PC and view in Excel.

    ![Screenshot of the .csv information displaying in a Microsoft Excel window.](images/Lab-guide/image151.png "Excel window")

13. Because this is a useful query, we can save it to be able to quickly run it again anytime we wish. First, copy the search query to the clipboard.

    ![Screenshot of a Search query with the Copy option selected.](images/Lab-guide/image152.png "Search query")

14. Click the **Saved Searches** followed by **+Add**

    ![Saved Searches is selected from the Log Search blade top menu.](images/Lab-guide/image153.png "Log Search blade top menu")

15. Complete the Add Saved Search Blade with this information:

    a.  Display Name: **Computers Needing Windows Defender Updates**

    b.  Category: **My Queries**

    c.  Query: paste the query from your clipboard

    d.  Function Alias: **WindowsDefNeeded**

    ![Add Saved Search blade fields are set to the previously defined settings.](images/Lab-guide/image154.png "Add Saved Search blade")

16. Select **OK.**

17.  Let's explore some more sample queries. These are taken from the repository at <https://github.com/MicrosoftDocs/LogAnalyticsExamples/tree/master/log-analytics>.

    Replace the current query with the following:
    ```
    let start_time=ago(23h);
    let end_time=now();
    Heartbeat
    | summarize heartbeat_per_hour=count() by bin_at(TimeGenerated, 1h, start_time), Computer
    | extend available_per_hour=iff(heartbeat_per_hour>0, true, false)
    | summarize total_available_hours=countif(available_per_hour==true) by Computer 
    | extend total_number_of_buckets=round((end_time-start_time)/1h)+1
    | extend availability_rate=total_available_hours*100/total_number_of_buckets
    ```
    Click **Run**.

18.  Select **Table** output. Notice how this query calculates VM availability, based on heartbeats.

![Screenshot showing the preceeding query in Log Analytics, with a table showing the %age availability of each VM.](images/Lab-guide/image185.png "Log Search showing VM availability query")

    For more information on how this query works, see <https://github.com/MicrosoftDocs/LogAnalyticsExamples/blob/master/log-analytics/server-availability-rate.md>.

19.  Replace the current query with the following:
    ```
    // Find all processes that started in the last 3 days. ID 4688: A new process has been created.
    let RunProcesses = 
        SecurityEvent
        | where TimeGenerated > ago(3d)
        | where EventID == "4688";
    // Find the 5 processes that were run the most
    let Top5Processes =
        RunProcesses
        | summarize count() by Process
        | top 5 by count_;
    // Create a time chart of these 5 processes - hour by hour
    RunProcesses 
    | where Process in (Top5Processes) 
    | summarize count() by bin (TimeGenerated, 1h), Process
    | render timechart
    ```
    Click **Run**.

    This query uses security events (only available when using the Standard tier of Security Center) to identify how often each process was run in the past 3 days, and then calcualtes the most commonly run processes. For more information, see <https://github.com/MicrosoftDocs/LogAnalyticsExamples/blob/master/log-analytics/top-5-running-processes-in-the-last-3-days.md>.

### Task 2: Preventive maintenance using Security Center

In this section, we will use the Security Center Overview screen to review what preventative steps we can take to protect our environment.

1.  Within **Security Center** **Overview** page, there are five tiles in the middle of the screen under **Resource security hygiene**

    ![On the Security Center Overview page, under Prevention, four tiles display: Recommendations, Compute & Apps, Networking, Data & storage, and Identity & access.](images/Lab-guide/image155.png "Security Center Overview page Resource security hygiene tiles")

2.  Click on the **Compute** tile to drill into the security health of your compute resources

    ![Screenshot of the Compute & apps tile.](images/Lab-guide/image156.png "Compute & apps tile")

3.  Notice there are several recommendations at the bottom of the screen. Let's go ahead and drill into these recommendations. Click on the **Missing disk encryption** item.

    ![Screenshot of the Compute page, Overview tab information.](images/Lab-guide/image157.png "Compute page, Overview tab")

4.  This brings up the Apply disk encryption blade. This gives a description of Azure Disk Encryption, links to instructions, and a list of virtual machines affected.

    ![On the Apply disk encryption blade, a description is given and the virtual machines created in this lab are displayed.](images/Lab-guide/image159.png "Apply disk encryption blade")

5.  Close the panel and navigate back to the **Security Center Overview** page. This time, click on the **Networking** tile to drill into the security health of your networking resources.

> ![Screenshot of the Networking tile.](images/Lab-guide/image160.png "Networking tile")

6.  Review the items showing in the **Networking Recommendations**. We won't implement these recommendations today, but if we wanted to, we could click on the item and implement each recommendation from this panel.

    ![Screenshot of the Security Health Networking page.](images/Lab-guide/image161.png "Security Health Networking page")

7.  Close the panel and navigate back to the **Security Center Overview** screen. This time, click on the **Data & storage** tile.

    ![Data & storage tile screenshot.](images/Lab-guide/image162.png "Data & storage tile")

8.  Notice this tile is green which is an indication the resources are in a healthy state. Azure Security Center will flag the tile as red if there are critical recommendations that need to be addressed.

9.  As a last step, navigate back to the **Security Center Overview** screen and click on the **Identity & access** tile

    ![Identity & access tile screenshot.](images/Lab-guide/image163.png "Identity & access tile")

10. As you review the Applications Security Health, notice in this example there's a recommendation to assign a second subscription owner

    ![Under Identity & access, there is a recommendation to assign more than one subscription owner.](images/Lab-guide/image164.png "Identity & access recommendations")

11. Go ahead and close this panel and return to the **Security Center Overview** page

12. Azure Security Center provides a quick way to see all the recommendations we just saw in a single view. Click on the **Recommendations** tile under the Overview section.

    ![Screenshot of the Recommendations tile.](images/Lab-guide/image165.png "Recommendations tile")

13. This presents the same recommendations you saw by navigating to each resource type in the previous steps. Clicking each of the recommendations will provide you with additional steps you can take to remediate the issue.

    ![The Recommendations page displays a list of recommendations, their resource, state, and severity.](images/Lab-guide/image166.png "Recommendations page")

### Task 3: Set up an Activity Log alert

If one of the virtual machines in the resource group were to be stopped (deallocated), that's a condition you would want to be notified of. In this section, we will set up an activity log alert to detect this condition.

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking **All services**, searching for "*monitor*", and selecting **Monitor**

    ![The previously mentioned selections are made in the Azure Portal.](images/Lab-guide/image66.png "Azure Portal")

2.  Select **Alerts** under **SHARED SERVICES**, followed by **New Alert Rule**

    ![Under Shared services, Alerts and then New Alert Rule are highlighted.](images/Lab-guide/image167.png "New Alert Rule")


3.  Under **Define alert condition**, click on **+ Select target**

    ![+ Select Target is highlighted on the blade defining a new Azure alert.](images/Lab-guide/image167b.png "Select target screenshot")

4.  In the 'Select a resource' blade, open the **Filter by resource type** drop-down and select **Virtual machines**. The UI will show each virtual machine in your subscription, grouped into resource groups. Click on the **HOLRG** resource group to select all the virtual machines in that resource group as the target resources monitored by this alert rule. Verify this selection in the 'Selection preview', then click **Done**.

    ![Screenshot showing the 'select a resource' blade, with selections matching the preceeding text.](images/Lab-guide/image167c.png "'Select a resource' screenshot")

5.  Under **Define alert condition**, select **+ Add criteria**

    ![+ Add Criteria is highlighted on the blade defining a new Azure alert.](images/Lab-guide/image167d.png "Add criteria screenshot")

6.  In the 'Configure signal logic' blade, open the **Monitor service** drop-down and select **Activity Log - Administrative**. In the search field, enter **deallocate**. Click on **Deallocate Virtual Machine (virtualMachines)**.

    ![Screenshot showing the 'Configure signal logic' blade, with selections matching the preceeding text.](images/Lab-guide/image167e.png "'Configure signal logic' screenshot")

7.  If you have recently stopped the virtual machines, you will see a history of those events, otherwise the chart will show 'No data to display'. Leave the settings at their default values, and click **Done**.

    ![Screenshot showing the 'Configure signal logic' blade, showing a chart of past 'Deallocate Virtual Machine' events. At the bottom of the screenshot, the 'Done' button is highlighted.](images/Lab-guide/image167f.png "'Configure signal logic' screenshot")

8.  Under **Define alert details**, fill in as follows:
    - Alert rule name: **Alert on VM deallocate**
    - Description: **Raise an alert any time any VM in the HOLRG resource group is stop-deallocated**
    - Save alert to resource group: **HOLRG**
    - Enable rule upon creation: **Yes**

    ![Screenshot showing the 'Define alert details' settings filled in as described in the preceeding text.](images/Lab-guide/image167g.png "'Define alert details' screenshot")

9.  Under **Define action group**, select **+ New action group**

    ![+ New action group is highlighted on the blade defining a new Azure alert.](images/Lab-guide/image167h.png "New action group screenshot")

    NOTE: Action groups define what action is taken when an alert is fired. They are defined separately from the alert rule, so that the same action group can be re-used across multiple alerts.

10.  Fill in the first section of the **Add action group** blade as follows:
    - Action group name: **Mobile app push notifications action group**
    - Short name: **Mobile Push**
    - Subscription: **Choose your subscription**
    - Resource group: **HOLRG**

   ![Screenshot showing the first section of the 'Add action group' blade filled in as described in the preceeding text.](images/Lab-guide/image167i.png "Add action group screenshot")

11.  In the table under **Actions**, under **Action name** enter **Notify mobile app**. Open the drop down under **Action Type** and select **Email/SMS/Push/Voice**.

   ![Screenshot showing the first line of the 'Actions' table, filled in as described in the preceeding text.](images/Lab-guide/image167j.png "Actions table screenshot")

12.  The **Email/SMS/Push/Voice** blade should open automatically (if it does not, click on **Edit details**). Enable the checkbox for **Azure app Push Notifications**, and fill in your Azure user ID, then select **OK**.

   ![Screenshot showing the 'Email/SMS/Push/Voice' blade, filled in as described in the preceeding text.](images/Lab-guide/image167k.png "'Email/SMS/Push/Voice' blade screenshot")

13.  All alert settings are now complete. Select **OK** to close the 'Add action group' blade, then click on **Create alert rule** to close the **Create rule** blade. You will see a notification once the alert has been created.

![Screenshots of the notification of the successfully created alert rule.](images/Lab-guide/image171.png "Successfully created alert rule message")

    NOTE: It may take up to 5 minutes after the alert rule is created for the alert to become active.

### Task 4: Installing & using the Azure mobile application

In this section, you will take your monitoring solution mobile by installing and configuring the OMS Mobile Application.

1.  Open the **AppStore** or **Google Play** on your mobile device. Locate the search box and type **Microsoft Azure,** and press enter.

2.  When you locate the Microsoft Azure application, install it to your device

    ![Screenshot of the Microsoft Azure Application Open screen that displays after successfully downloading the application.](images/Lab-guide/image172.png "Azure Application Open screen")

3.  Once the application has been installed, you will need to allow the application to send you notifications

4.  Next, touch the Sign In Button

    ![Screenshot of the Sign in screen.](images/Lab-guide/image173.png "Sign in screen")

5.  Once the login page loads, enter your Azure Credentials

6.  Once you are logged into the app, navigate to the Notifications menu to see there are currently no notifications

    ![Notifications screen screenshot.](images/Lab-guide/image174.png "Notifications screen")

7.  Putting the phone aside for a moment, in your desktop web browser, navigate to the **Virtual Machines** list in the Azure portal

8.  Click on **checkbox next to WEBVM1**, then click **Stop**, followed by **Yes** at the confirmation prompt. This will stop (deallocate) this virtual machine, which should trigger our alert to notifiy the Azure mobile application.

    ![WEBVM1 is selected from the virtual machies list, and the 'stop' button is highlighted.](images/Lab-guide/image175.png "Stop WEBVM1")

9. In a few moments, you should receive an alert through the Azure mobile app that the virtual machine was stopped (deallocated)

### Task 5: Application Insights

Understanding what is happening within an application can be very challenging, but with the Application Insights configured for CloudShop, there is great telemetry being fed to the Azure Portal. Here, you will investigate that data regarding how the CloudShop is performing.

1.  Open the **Azure portal** and navigate to Azure Monitor by clicking on **All services**, searching for "*monitor*", and selecting **Monitor**

    ![The previously mentioned selections are made in the Azure portal.](images/Lab-guide/image66.png "Azure portal")

2.  Click on the **Application Insights** tile, then select **HOLCloudShop**

    ![Application Insights (1) is selected in the Overview blade, and under Name, HOLCloudShop (2) is selected.](images/Lab-guide/image177.png "Selecting Application Insights from Azure Monitor")

3.  From the Application Insights blade, click on **Pin to Dashboard**

    ![The Pin to dashboard icon is selected from the Overview blade.](images/Lab-guide/image178.png "Pin to dashboard icon")

4.  Under the **INVESTIGATE** section, click the **Performance** item

    ![Under Investigate, Performance (preview) is selected.](images/Lab-guide/image179.png "Investigate section")

5.  The **Performance** feature of Application Insights allows us to get rich performance monitoring and easy to consume dashboards


6.  This dashboard gives near real-time insight into the performance of your application. In the screenshot below, you can see this application appears to have a performance issue with the Home/Index page. It appears to be taking 20.5 seconds on average to load. 

NOTE: Your numbers may not match. By clicking on the GET Home/Index, you will see the other sections of the dashboard will filter to performance data focused on that page.

    ![Under Operation Name, Get Home / Index is selected.](images/Lab-guide/image181.png "Operation Name section")

7.  Feel free to experiment with this dashboard to understand the performance considerations of your application

8.  Pin the 'Operation times' chart to to My Dashboard by clicking on the pin in the top right of the chart

    ![Screenshot of the Pin icon.](images/Lab-guide/image183.png "Pin icon")

9.  At this point, navigate back to **My Dashboard** and click **Edit Dashboard,** and arrange the many tiles you have added to give yourself an all up view of the environment you have built. Feel free to open the various solutions, find interesting data and Pin it to the Dashboard.

    ![Screenshot of My deashboard.](images/Lab-guide/image184.png "My deashboard")

### Summary

In this exercise, you explored the information and data being provided by Azure Security and Operations Management and Application Insights to gain situational awareness of the application and infrastructure. You looked at the Security Posture of the infrastructure, the performance of applications, and you built a dashboard that can be used to manage it moving forward.

## After the hands-on lab 

Duration: 10 mins

### Overview

In this exercise, attendees will de-provision any Azure resources that were created in support of the lab.

1.  Delete the **HOLRG, HOLInsights**, and **OPSLABRG** resource groups

You should follow all steps provided *after* attending the Hands-on lab.
