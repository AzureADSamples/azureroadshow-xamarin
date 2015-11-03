---
services: active-directory
platforms: dotnet
author: dstrockis
---

# Demo - To-do list reimagined iOS app
This code sample is one of three referenced in the Azure AD sessions of the [Microsoft Cloud Roadshow](https://www.microsoftcloudroadshow.com/).  Recordings of these sessions will be available shortly [here]().  We recommned you watch one of these recordings to understand the purpose and goals of this code sample.

To-Do List Reimagined (tdlr;) is a new cloud service that allows users to store and manage a list of tasks.  It integrates with Azure AD in order to provide enterprise features to its customers that have an existing Azure AD tenant.  These features include:

- Discovery of accounts with an existing Azure AD tenant
- Signing up for the app with a work account
- One-click user authorization using consent & OAuth 2.0
- Signing into the app with a work account
- Sharing tasks between users in the same company using a "people picker".
- Outsourced user management to company admins

The full service consists of three different sample projects:

- [The tdlr; web application](https://github.com/azureadsamples/azureroadshow-web), written as .NET 4.5 MVC app.
- [This tdlr; iOS application](https://github.com/azureadsamples/azureroadshow-xamarin), written as a cross-platform Xamarin app.
- [The tdlr; admin web portal](https://github.com/azureadsamples/azureroadshow-web-admin), written as a .NET 4.5 MVC app.

## Running the tdlr; iOS app

### Get a mac, and get Xamarin

At this point in time, the tdlr; app will only run on iOS - which means that you will need a mac on which to deploy the code.  In addition, you will need to [download & install Xamarin](http://xamarin.com/download) on your machine.  This code sample was built using Xamarin Studio, which is installed by default when your install Xamarin on your machine.  You are free to use Visual Studio on a Windows machine and remote connect to the Xamarin.iOS build host via wifi - but setting up your development environment will be a bit more involved.

### Running the tdlr; web service

This sample Xamarin application is written to be used with the tdlr; web app as its backend data storage service.  Before proceeding, you will need to create your own instance of the service by following the instructions for the [tdlr web app](https://github.com/azureadsamples/azureroadshow-web) and host it somewhere where your iOS app can communicate with it - we recommend deploying it as an Azure web app.

If you don't want to spin up your own tdlr; service, you can instead use our instance as a workaround.  To sign up for our tdlr; service, open [this link](http://todolistreimagined.azurewebsites.net/account/signup/aad?sign_up_hint=) in a new tab.  Sign in with a user in your development tenant.  This process will add our tdlr; service to your tenant so you can configure your app to access it.  Note that **you will need to repeat this process for each tenant** that you use to sign into your iOS app (we told you it was a workaround).

### Register an app with Azure AD

You'll now need to register your Xamarin app in the Azure Management Portal so that your version of tdlr; can sign users in and get information from Azure AD.  In order to do so, you will need an Azure Subscription.  If you don't already have an Azure subscription, you can get a free subscription by signing up at [http://azure.com](http://azure.com).  All of the Azure AD features used by this sample are available free of charge.

You'll also need an Azure Activce Directory tenant in which to register your application.  Every Azure subscription has an associated tenant, which you are free to use.  You may also wish to create an additional tenant, since the tdlr; app is 'multi-tenant' - it allows users from any organization to sign up & sign in.  You'll want to create a few users in your tenant(s) for testing purposes - a guest user with a personal MSA account will not work for this sample.

1. Sign in to the [Azure management portal](https://manage.windowsazure.com).
2. Click on Active Directory in the left hand nav.
3. Click the directory tenant where you wish to register the sample application.
4. Click the Applications tab.
5. In the drawer, click Add.
6. Click "Add an application my organization is developing".
7. Enter a friendly name for the application, for example "TDLR;", select "Native Client Application", and click next.
8. For the Redirect URI, enter `http://tdlr`.  Click finish.
9. Click the Configure tab of the application.
10. Find the Client ID value and copy it aside, you will need this later when configuring your application.
13. In "Permissions to Other Applications", click the dropdown for "Delegated Permissions".  Check the `Access the directory as the signed-in user` permission.  Your app should now show two delegated permission.
11. Also in the "Permissions to Other Applications" section, click "Add Application."  Select "Other" in the "Show" dropdown, and click the upper check mark.  Locate & click on the tdlr; web app, and click the bottom check mark to add the application.  Select "Access TDLR;" from the "Delegated Permissions" dropdown, and save the configuration.

Note: if you can't find the TDLR; web app in this menu, read the above section.  You'll first need to spin up your own instance of the tdlr; web service, or sign up for ours.

### Download the code

Now you can [download this repo as a zip](https://github.com/AzureADSamples/azureroadshow-xamarin/archive/master.zip) or clone it to your local machine:

`git clone https://github.com/azureadsamples/azureroadshow-xamarin`

In your local repo, open the `tdlr.sln` file in Xamarin Studio.

### Edit the app's config

To run the app, you'll need to enter the information from your app registration.  In Xamarin Studio, open the `tdlr/tdlr.cs` file in the root of the project and locate the app config properties.  Replace the following values with your own:

```
		// App Config Values
		public static string clientId = "[Enter your client ID as registered in the Azure Management Portal, e.g. 3d8c4803-ffcd-4b2a-baec-05056abdc408]";
		public static string taskApiResourceId = "https://strockisdevtwo.onmicrosoft.com/tdlr";
```

Also open the `tdlr/utils/TaskHelper.cs` file, and edit the value of the `taskApi` property to the base address of your tdlr web app.  

Note: if you are using our instance of the tdlr; service, leave the default value of the `taskApiResourceId` and `taskApi` as they are.

### Run the app!

You can now run the tdlr; app and explore its functionality.  Try signing up and signing in with your Azure AD users, creating tasks, and sharing them with other users.  To understand the code behind the app, we recommend you watch on of the recorded Microsoft Cloud Roadshow sessions which will be available soon [here]().  If you're already familiar with Azure AD, you may find the code comments instructive as well.

