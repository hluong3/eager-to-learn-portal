# Eager To Learn Portal

Please see the following references which provide the requirements for the landing page:
https://learn.microsoft.com/en-us/partner-center/marketplace/azure-ad-transactable-saaslanding-page
https://learn.microsoft.com/en-us/partner-center/marketplace/partner-center-portal/pc-saasfulfillment-life-cycle
https://learn.microsoft.com/en-us/partner-center/marketplace/partner-center-portal/pc-saasfulfillment-webhook

# App Registrations
You will create 2 app registrationa in the Azure portal. It will be single-tenant and multi-tenant.
### Create a single-tenant App Registration
You will create an app registration in the Azure portal. It will be single-tenant and used to connect to the SaaS API.

1. Log in to the Azure portal.
2. In the top-of-center command window in the portal, type "app reg" and select the item "App registrations" that appears. You will see a list of current application registrations.
3. At the top of the page, click the **+ New registration** link. You are taken to the **Register an application page**.
4. For example, use  `eager-to-learn-single-tenant` as the name of this app registration.
5. At the bottom of the page, click the **Register** button.

### Add a secret

Now you will add a secret to the app registration. Make sure to copy and paste the secret value somewhere you can access it later.

1. Click into the `eager-to-learn-single-tenant` application registration.
2. In the left-hand menu, find the **Manage** menu and click the **Certificates and secrets** link.
3. Create a new client secret.
    1. Give it a description.
    2. Choose an expiration date. The 6-month default should be fine.
4. Copy the **Value** of the secret and paste it somewhere you can easily access it later.

## Creating a multi-tenant App Registration

Following steps are how to create a multi-tenant app registration in the Azure portal to authenticate users coming to your landing page.

1. Create an app registration. For example, use `eager-to-learn-multi-tenant` as the name of this app registration.
2. Under **Supported account types**, select the option: **Accounts in any organizational directory (Any Azure AD directory - Multitenant)**.
3. At the bottom of the page, click the **Register** button.

### Create a client secret

1. Create a client secret for this app registration.
2. Copy the **Value** of the secret and paste it somewhere you can easily access it later.

## Configure LandingPage SourceCode - Prepare for deploying to Azure Web App: appsettings.json

1. Download the SourceCode from this Github repository
2. Unzip and open the File Explorer on Windows, go to "LandingPage" > appsettings.json
3. The `appsettings.json` file in the LandingPage project must be filled out before publishing the application. That's what you'll do in this section.

### Values from the eager-to-learn-multi-tenant app registration

All changes in this section occur under the `AzureAD` section of the `appsettings.json` file.

1. In the Landing Page project, open the `appsettings.json` file.
2. In the Azure portal, click the **Azure Active Directory** button in the left-hand menu.
3. On the overview page in the **Basic information** section, find the the **Primary domain** and use it to replace `DOMAIN_NAME` in `appsettings.json`.
4. In the left-hand menu under the **Manage** menu, click the **App registrations** link.
5. Click the `eager-to-learn-multi-tenant` app registration.
6. In the `appsetting.json` file, replace `CLIENT_ID` with the **Application (client) ID** from this screen.
7. Replace `CLIENT_SECRET` with the client secret you noted earlier for the `eager-to-learn-multi-tenant` app registration.

### Values from the eager-to-learn-single-tenant app registration

All changes in this section occur under the `MarketplaceAPI` section of the `appsettings.json` file.

1. Go back to the list of App registrations.
2. Click the `eager-to-learn-single-tenant` app registration.
3. In the `appsetting.json` file, replace `CLIENT_ID` with the **Application (client) ID** from this screen.
4. In the `appsetting.json` file, replace `TENANT_ID` with the **Directory (tenant) ID** from this screen.
5. In the `appsetting.json` file, replace `CLIENT_SECRET` with the client secret value you stored earlier.

Congratulations, your LandingPage project is ready to publish!

## Publishing your landing page

First you must create a publish profile file that will hold the information about your project to publish. Also, you must create an App Service Plan to host your website.

### Creating your publish profile

1. In the Solution Explorer of Visual Studio, right click the `LandingPage` project and select **Publish**.
1. The **Publish** dialog appears.
1. Select **Azure** as your publish target and click the **Next** button.
1. Select **Azure App Service (Windows)** and click the **Next** button.

    > You are now on the **App Service** tab. You may now need to sign in to Visual Studio with your Azure credentials.

1. Choose the correct subscription from the list of subscriptions.
1. To the far right of the words **App Service Instances**, click the green **+** (plus) button to create an App Service. The App Service dialog opens.
1. Name the App Service `SaaSEagerToLearnLandingPage` with a number on the end to make it unique.
1. Choose `eager-to-learn` for the resource group.
1. Create a new **Hosting Plan** and name it `SaaSEagerToLearnAppServicePlan`.
1. Click the **Create** button at the bottom of the screen.

    > After clicking the **Create** button at the bottom of the page it may take a moment for the Hosting Plan and App Service to be created.

1. After the App Service is created, you will be back at the **Publish** dialog. Select your new App Service instance and click the **Next** button.
1. For Publish type, select **Publish (generate pubxmlfile)**.
1. Click the **Finish** button at the bottom of the dialog.

You now have a publish profile that can be used to publish your application.

### Publishing the landing page

Near the top-right of the Publish dialog click the **Publish** button. This deploys your application. You can watch the publish process in the Output window at the bottom of the screen.

> The page will launch, but shows an error message. You aren't quite done configuring your landing page application.

### Registering your landing page

1. In the command bar at the top of the Azure portal, search for and select **App Services**.
1. Click on the App Service you just created, **SaaSEagerToLearnLandingPage**.
1. On the overview screen in Visual Studio, the **URL** for the site should be in the **Hosting** section. Copy it into your paste buffer.
1. In the command bar at the top of the Azure portal, search for and select **App registrations**.
1. Click on the `eager-to-learn-multi-tenant` app registration.
1. In the left-hand menu click the **Authentication** link.
1. In the **Web > Redirect URIs** section, click **Add URI** and paste in your Landing Page URL.
1. Paste it in again on another line, this time adding a suffix of `/signin-oidc` so that your URI looks something like this.

    > https://*.azurewebsites.net/signin-oidc

Click the **Save** button at the top of the screen.

## Browse to your landing page

1. Using the App Service URL to you created, `https://<prefix>.azurewebsites.net`, browse to that web page.
1. Authenticate when prompted.
1. You should see a message that reads, "**Token URL parameter cannot be empty**."

> **This is good!** You are seeing this message because you aren't coming to the landing page through the Azure portal and no marketplace purchase token is being passed. To finish your configuration, we need to configure Partner Center to be aware of the landing page.


