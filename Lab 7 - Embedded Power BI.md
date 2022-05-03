![Microsoft Power Platform.](Images/powerbi-welcome-7.png 'Microsoft Power Platform')

# Lab 7 - Embedded Power BI App in Web App

**Contents** 

<!-- TOC -->

- [Introduction](#introduction)
- [Embedded Power BI App in Web App](#embedded-power-bi)
  - [Task 1 - Generate Client secret value in Azure AD App](#task1-generate-client-secret-value-in-azure-ad-app)
  - [Task 2 - Enable the Power BI service admin settings](#task2-enable-the-power-bi-service-admin-settings)
  - [Task 3 - Add the service principal and security group to your workspace](#task3-add-the-service-principal-and-security-group-to-your-workspace)
  - [Task 4 - Change your Azure AD app's permissions](#task4-change-your-azure-ad-app's-permissions)
  - [Task 5 - Get the embedding parameter values](#task5-get-the-embedding-parameter-values)
  - [Task 6 - Embed your content in Visual Studio and run locally](#task6-embed-your-content-in-visual-studio-and-run-locally)
  - [Task 7 - Publishing Embedded Power BI report to Web App](#task7-publishing-embedded-power-bi-report-to-web-app)
  - [Developing your application](#developing-your-application)
  - [References](#references)

## Introduction

**Embedded analytics** and **Power BI Embedded** (the Azure offer) allow you to embed Power BI content such as reports, dashboards and tiles, into your application.

In this lab, you'll learn how to:

 - Set up your embedded environment.
 - Configure an embed for your customers (also known as app owns data) sample application.

To use application, users won't need to sign in to Power BI or have a Power BI license.

We recommend using the embed for users method to embed your Power BI content, if you're an independent software vendor (ISV) or a developer, who wants to create applications for third parties.

### Code sample specifications

This lab includes instructions for configuring an embed for sample application in one of the following frameworks:

 - .NET Framework
 - .NET Core
 - Java
 - Node JS
 - Python

The code samples support the following browsers:

 - Microsoft Edge
 - Google Chrome
 - Mozilla Firefox

## To create an embed for users sample app, follow these steps:

### Task 1 - Generate Client secret value in Azure AD App

1. Log into [Microsoft Azure](https://portal.azure.com) portal.

2. Search for App registrations in Search box at the top and **App registrations** under Services.

   ![](Images/search-app-registration.png)
   
3. In the App registrations pane, select **All application**  and click on **DIADApp**.

   ![](Images/app-registration-diadapp.png)

4. From the **Overview** tab, copy and save the **Application (client) ID** in text editor for later use.

   ![](Images/overview-applicationid.png)
   
5. Click on **Certificates & secrets** under Manage from left and select **+ New client secret**.

   ![](Images/new-client-secret.png)
   
6. **Add a client secret** pane appears, enter **DIAD client secret** in Description and leave default for Expires and click on **Add**.

   ![](Images/add-client-secret.png)
   
7. Copy and save the **Client Secret Value** in text editor for later use.

   ![](Images/save-secret-value.png)

>**Note**: After you leave this window, the client secret value will be hidden, and you'll not be able to view or copy it again.

### Task 2 - Enable the Power BI service admin settings

1. Naviaget to [PowerBI App](https://app.powerbi.com/) in the browser. Use your admin account credentials for sign-in.

2. From the page header, select **...**(1)> **Settings**(2) > **Admin portal**(3).

   ![](Images/select-admin-portal.png)
   
3. In **Tenant settings** under Admin Portal, scroll down to **Developer settings**. Then click on the **Allow service principals to use Power BI APIs** drop-down, toggle the button to **Enabled**. For **Apply to**: Check for **The entire organization** and click on **Apply**.

   ![](Images/enabled-service-principle.png)
   
### Task 3 - Add the service principal and security group to your workspace

1. Navigate back to **Home** pane of Power BI App, select **Workspaces**(1) then click on **eclipse**(2) next to workspace name. Then select **Workspace access**(3).

   ![](Images/workspace-access.png)
 
2. In the **Access** pane, search for DIAD and add both the **DIADGroup** and **DIADApp**.

   ![](Images/add-diad-group-sp.png)

3. After selecting **DIADGroup** and **DIADApp** (1), select **Member** (2) access from the drop-down and click on **Add** (3) and **Close** (4).

   ![](Images/sp-sg-access-add.png)

   
### Task 4 - Change your Azure AD app's permissions

1. Navigate back to [Microsoft Azure](https://portal.azure.com) portal.

2. Search for App registrations in Search box at the top and click the **App registrations** under Services.

   ![](Images/search-app-registration.png)
   
3. In the App registrations pane, select **All application**  and click on **DIADApp**.

   ![](Images/app-registration-diadapp.png)

4. Select **API permissions** under Manage and select **+ Add a permission**.

   ![](Images/add-api-permission.png)
   
5. From the Microsoft APIs tab, scroll-down and select Power BI service.

6. Select **Delegated Permissions** and add or remove the specific permissions as you need. Now you can add some permissions for **App**, **Capacity**, **Dashboard**, **Report**, **Tenant**, and **Workspace** as shown in the below image. Then click on **Add permissions**.

   ![](Images/assign-api-permission.png)

7. Select the **Grant admin consent for Azure HOL ###**(1) and click **Yes** on the pop-up.

   ![](Images/grant-admin-consent.png)

>**Note**: To remove a permission, select the ellipsis (...) to the right of the permission. Select Remove permission. In the Remove permission pop-up window, select Yes, remove.

### Task 5 - Get the embedding parameter values

To embed your content, you need to obtain certain parameter values. The table below shows the required values, and indicates if they're applicable to the service principal authentication method, the master user authentication method, or both.

Before you embed your content, make sure you have all the values listed below. Some of the values will differ, depending on the authentication method you're using.

   | Parameter                | Service principal     | Master user         |
   | ------------------------ | --------------------- | ------------------- |
   | [Client ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#client-id)                | ![](Images/yes.png)   | ![](Images/yes.png) |  
   | [Workspace ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#workspace-id)             | ![](Images/yes.png)   | ![](Images/yes.png) |
   | [Report ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#report-id)                | ![](Images/yes.png)   | ![](Images/yes.png) |
   | [Client secret](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#client-secret)            | ![](Images/yes.png)   | ![](Images/no.png)  |
   | [Tenant ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#tenant-id)               | ![](Images/yes.png)   | ![](Images/no.png)  |
   | [Power BI username](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#power-bi-username-and-password)        | ![](Images/no.png)    | ![](Images/yes.png) |
   | [Power BI password](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#power-bi-username-and-password)        | ![](Images/no.png)    | ![](Images/yes.png) |

### Workspace ID

>**Tip**: Applies to: ![](Images/yes.png) Service principal ![](Images/yes.png) Master user

To get the workspace ID GUID, follow these steps:

1. Sign in to Power BI service.

2. Open the report you want to embed.

3. Copy the GUID from the URL. The GUID is the number between **/groups/** and **/reports/**.

   ![](Images/powerbi-07-02.png)

Alternatively, you can find the workspace ID in the **Admin portal** settings by selecting **Details** next to the workspace name.

![](Images/powerbi-07-03.png)

### Report ID

>**Tip**: Applies to: ![](Images/yes.png) Service principal ![](Images/yes.png) Master user

To get the report ID GUID, follow these steps:

1. Sign in to Power BI service.

2. Open the report you want to embed.

3. Copy the GUID from the URL. The GUID is the number between **/reports/** and **/ReportSection**.

   ![](Images/powerbi-07-04.png)

### Tenant ID

>**Tip**: Applies to: ![](Images/yes.png) Service principal ![](Images/no.png) Master user

To get the tenant ID GUID, follow these steps:

1. Log into [Microsoft Azure](https://ms.portal.azure.com/#allservices).

2. Search for **App registrations** and select the **App registrations** link.

3. Select the Azure AD app you're using for embedding your Power BI content.

4. From the **Overview** section, copy the **Directory (tenant) ID** GUID.

### Power BI username and password

>**Tip**: Applies to: ![](Images/no.png) Service principal ![](Images/yes.png) Master user

Obtain the username and password of the Power BI user you're using as your **master user**. This is the same user you used to create a workspace and upload a report to, in Power BI service.

### Task 6 - Embed your content in Visual Studio and run locally

The Power BI embedded sample application allows you to create an embed for your customers Power BI app.

Follow these steps to modify the embed for your customers sample application, to embed your Power BI report.

1. Open the [Power BI developer samples](https://github.com/microsoft/PowerBI-Developer-Samples) folder in the virtual machine.

2. Select **Code** and then select **Download zip**.

   ![](Images/powerbi-07-08.png)
   
3. Extract the downloaded ZIP and navigate to the **PowerBI-Developer-Samples-master** folder.

4. Depending on the language you want your app to use, open one of these folders:

   - .NET Core
   - .NET Framework
   - Java
   - Node JS
   - Python  

>**Note**: The embed for your customers sample applications only support the frameworks listed above. The React sample application only supports the **embed for your organization** solution.

5. Using **.NET Core**, open the **Embed for your customers** folder path `C:\Users\demouser\Downloads\PowerBI-Developer-Samples-master\PowerBI-Developer-Samples-master\.NET Core\Embed for your customers`

6. Double click on the **AppOwnsData.sln** file and select **Visual Studio 2019**.

   ![](Images/app-owns-data.png)

7. In the Sign in to Visual Studio pop-up, click on **Sign in** use your admin credentials to sign in.

   * Email/Username: <inject key="AzureAdUserEmail"></inject>
   
   * Password: <inject key="AzureAdUserPassword"></inject>

   ![](Images/vs-signin.png)
      
8. From the **Solution Explorer**, expand **AppOwnsData** and then double-click on **appsettings.json**.

   ![](Images/open-app-settings.png)

9. Update the **ClientID**, **TenantID**, and **ClientSecret** which you have copied earlier from the DIADApp.

10. Update the  **PbiUsername** and **PbiPassword** by using your admin credentials.

    * **PbiUsername**: <inject key="AzureAdUserEmail"></inject>

    * **PbiPassword**: <inject key="AzureAdUserPassword"></inject>

11. Update the **WorkspaceID** and **ReportID** with the values you have copied from the Power BI report URL.

12. After updating all the values your file should look like as shown in the below image.

    ![](Images/updated-appsettings.png)
    
13. **Save** the file **appsettings.json**.

14. Set the **IIS Express** play to **Web Browser (Microsoft Edge)**.

    ![](Images/run-with-edge.png)

15. Then run the project by selecting **IIS Express** (play). You will be able to see the website as shown in the below image.

    ![](Images/local-run-result.png)

>**Note**: You may receive error that your connection isn't private, select **Advanced** and click on **Continue to localhost (unsafe)**.
>
>   ![](Images/continue-localhost.png)
   
### Task 7 - Publishing Embedded Power BI report to Web App

1. In the Visual studio, right-click on the **AppOwnsData** in the Solution Explorer and select **Publish**.

   ![](Images/publish-appwonsdata.png)
   
2. For Target, select **Azure** and click on **Next**.

   ![](Images/select-azure-publish.png)  

3. For Specific target, select **Azure App Service (Windows)** and click on **Next**.

   ![](Images/azure-app-service-publish.png)

4. For App Service, click on the resource group **ODL-DIAD-<inject key="DeploymentID" enableCopy="false"/>** drop-down and select the **DIAD-webapp-<inject key="DeploymentID" enableCopy="false"/>** web app. Click on **Finish**.

   ![](Images/select-webapp-finish.png)

5. In the Ready to Publish, select **Publish** and wait for few minutes till the buid to gets succeed.

   ![](Images/ready-to-publish.png)
   
6. Navigate back to Azure portal, search for **DIAD-webapp-<inject key="DeploymentID" enableCopy="false"/>** and select it.

7. In the Overview pane of the Web App, click on **URL**.

   ![](Images/web-app-url.png)
   
8. You will be able to see the PowerBI App in the new tab as shown in the below image.

   ![](Images/web-app-result.png)  

## Developing your application

After configuring and running the embed for your customers sample application, you can start developing your own application.

When you're ready, review the [move to production](https://docs.microsoft.com/en-us/power-bi/developer/embedded/move-to-production) requirements. You'll also need a [capacity](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embedded-capacity), and should review the [capacity planning](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embedded-capacity-planning) article to establish which SKU best suits your needs.
     
 ## References

Dashboard in a Day introduces you to some of the key functions available in Power BI. In the ribbon of the Power BI Desktop, the Help section has links to some great resources.

   ![](Images/powerbi-01-53.png)

Here are a few more resources that will help you with your next steps with Power BI.

  - Getting started: http://powerbi.com
  - Power BI Desktop: https://powerbi.microsoft.com/desktop
  - Power BI Mobile: https://powerbi.microsoft.com/mobile
  - Community site https://community.powerbi.com/
  - Power BI Getting started support page: https://support.powerbi.com/knowledgebase/articles/430814-get-started-with-power-bi
  - Support site https://support.powerbi.com/
  - Feature requests https://ideas.powerbi.com/forums/265200-power-bi-ideas
  - New ideas for using Power BI https://aka.ms/PBI_Comm_Ideas
  - Power BI Courses http://aka.ms/pbi-create-reports
  - Power Platform https://powerplatform.microsoft.com/en-us/instructor-led-training/
  - Power Apps [Business Apps | Microsoft Power Apps](https://powerapps.microsoft.com/en-us/)
  - Power Automate [Power Automate | Microsoft Power Platform](https://powerapps.microsoft.com/en-us/)
  - Dataverse [What is Microsoft Dataverse? - Power Apps | Microsoft Docs](https://docs.microsoft.com/en-us/powerapps/maker/data-platform/data-platform-intro)
  - Embedded Power BI repotrt [Embed a Power BI report in an application for your customers | Microsoft Docs](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-customer-app)
  
