![Microsoft Power Platform.](Images/powerbi-welcome-7.png 'Microsoft Power Platform')

# Lab 7 - Embedded Power BI App in Web App

**Contents** 

<!-- TOC -->

- [Introduction](#introduction)
- [Embedded Power BI](#embedded-power-bi)

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

2. Search for App registrations in Search box at the top and click the App registrations link.

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
   
### Add the service principal and security group to your workspace

4. Navigate back to **Home** pane of Power BI App, select **Workspaces**(1) then click on **eclipse**(2) next to workspace name. Then select **Workspace access**(3).

   ![](Images/workspace-access.png)
 
5. In the **Access** pane, search for DIAD and add both the **DIADGroup** and **DIADApp**.

   ![](Images/add-diad-group-sp.png)

6. After selecting **DIADGroup** and **DIADApp** (1), select **Member** (2) access from the drop-down and click on **Add** (3) and **Close** (4).

   ![](Images/sp-sg-access-add.png)

### Task 3 - Register an Azure AD application to use with Power BI

1. Open the [Power BI App Registration Tool](https://app.powerbi.com/embedsetup) in the browser.

2. In the Choose an embedding solution section, select **Embed for your customers**.

   ![](Images/embed-for-customers.png)

3. In Step 1 - If you have not sign in to Power BI then sign in using your admin account credentials. The Azure AD app will be registered under this user. Click on **Next**.

   ![](Images/signin-next.png)
   
4. In Step 2 - Enter the Application Name as **DIAD Embedded Power BI App**. Select all the **Read only APIs** access and you can also select for other access according to your requirment. Click on **Register**.

   ![](Images/access-register.png)

5. Skip the Steps 3 & 4 as we have already created a workspace and imported content.

6. In Step 5 - Click on **Grant permissions**. You will receive an authentication pop-up of Permissions requested, **Check** the consent on behalf of your organization and click on **Accept**.

   ![](Images/grant-permission.png)
   
Task 4 - Change your Azure AD app's permissions




















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

### Client ID

>**Tip**: Applies to: ![](Images/yes.png) Service principal ![](Images/yes.png) Master user

To get the client ID GUID (also know as application ID), follow these steps:

1. Log into [Microsoft Azure](https://ms.portal.azure.com/#allservices).

2. Search for **App registrations** and select the **App registrations** link.

3. Select the Azure AD app you're using for embedding your Power BI content.

4. From the **Overview** section, copy the **Application (client) ID** GUID.

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

### Client secret

>**Tip**: Applies to: ![](Images/yes.png) Service principal ![](Images/no.png) Master user

To get the client secret, follow these steps:

1. Log into [Microsoft Azure](https://ms.portal.azure.com/#allservices).

2. Search for **App registrations** and select the **App registrations** link.

3. Select the Azure AD app you're using for embedding your Power BI content.

4. Under **Manage**, select **Certificates & secrets**.

5. Under **Client secrets**, select **New client secret**.

6. In the **Add a client secret** pop-up window, provide a description for your application secret, select when the application secret expires, and select **Add**.

7. From the **Client secrets** section, copy the string in the **Value** column of the newly created application secret. The client secret value is your client ID.

> **Note**: Make sure you copy the client secret value when it first appears. After navigating away from this page, the client secret will be hidden and you'll not be able to retrieve its value.

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

### Task 4 - Service principal API access

> **Top**: Applies to: ![](Images/yes.png) Service principal ![](Images/no.png) Master user

> **Note**: This step is only relevant if you're using the service principal authentication method. If you're using a master user, skip this step and continue with Step 7 - Enable workspace access.

For an Azure AD app to be able to access the Power BI content and APIs, a Power BI admin needs to enable service principal access in the Power BI admin portal. If you're not the admin of your tenant, get the tenant's admin to enable the Tenant settings for you.

1. In Power BI service, select **Settings** > **Settings** > **Admin portal**.

   ![](Images/powerbi-07-05.png)
   
2. Select **Tenant settings** and then scroll down to the **Developer settings** section.

3. Expand **Allow service principals to use Power BI APIs**, and enable this option.

   ![](Images/powerbi-07-06.png)

> **Note**: When using a service principal, it's recommended to limit its access to the tenant settings using a security group. To learn more about this feature, see these sections in the service principal article:
>- [Create an Azure AD security group](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-service-principal#step-2---create-an-azure-ad-security-group)
>- [Enable the Power BI service admin settings](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-service-principal#step-3---enable-the-power-bi-service-admin-settings)

### Task 5 - Embed your content

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

5. Open the **Embed for your customers** folder.

6. Using **.NET Core**, open the embed for your customers sample app using one of these methods:

   - If you're using [Visual Studio](https://visualstudio.microsoft.com/), open the **AppOwnsData.sln** file.

   - If you're using [Visual Studio Code](https://code.visualstudio.com/), open the **AppOwnsData** folder.

7. Open **appsettings.json**.

8. Depending on your authentication method, fill in the following parameter values:

   | Parameter                | Service principal     | Master user         |
   | ------------------------ | --------------------- | ------------------- |
   | AuthenticationMode       | ServicePrincipal      | MasterUser          |  
   | ClientId                 | Your Azure AD app [client ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#client-id)   | https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#client-id  |
   | TenantId                 | Your Azure AD [tenant ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#tenant-id)   | N/A |
   | PbiUsername              | N/A   | Your master user username, see [Power BI username and password](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#power-bi-username-and-password)  |
   | PbiPassword              | N/A   | Your master user password, see [Power BI username and password](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#power-bi-username-and-password)   |
   | ClientSecret             | Your Azure AD [client secret](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#client-secret)    | N/A   |
   | WorkspaceId              | The ID of the workspace with your embedded report, see [Workspace ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#workspace-id)    | The ID of the workspace with your embedded report, see [Workspace ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#workspace-id)  |
   | ReportId                 | The ID of the report you're embedding, see [Report ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#report-id)   | The ID of the report you're embedding, see [Report ID](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core#report-id)  |
   
9. Run the project by selecting the appropriate option:

   - If you're using **Visual Studio**, select **IIS Express** (play).

   - If you're using **Visual Studio Code**, select **Run** > **Start Debugging**.   
   
## Developing your application

After configuring and running the embed for your customers sample application, you can start developing your own application.

When you're ready, review the [move to production](https://docs.microsoft.com/en-us/power-bi/developer/embedded/move-to-production) requirements. You'll also need a [capacity](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embedded-capacity), and should review the [capacity planning](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embedded-capacity-planning) article to establish which SKU best suits your needs.
   
   
   
   
   
   
   
   
   
