# Azure App Services
---
**Description**

In today's cloud-first world, deploying and managing scalable web applications is essential for modern developers, and Azure App Service offers the tools to make it happen. In this course, you'll explore App Service Plans, serverless functions, deployment strategies, and production-ready configurations, empowering you to build robust cloud applications with confidence. We will delve into the core elements of Azure compute solutions, mastering techniques to deploy web apps, orchestrate serverless workflows, and secure your applications seamlessly. Learn to transform complex deployment challenges into streamlined cloud successes!

---
### Azure compute services
Which of the following best describes Azure compute services?
```
They provide storage and database solutions for applications.
〇 They include services such as Virtual Machines, App Services, and Containers for running applications.
They are used only for managing networking and security.
They focus solely on analytics and data visualization.
```
*Correct! Azure compute services power application execution through resources like VMs, App Services, and containers.*

### Create an App Service plan
Your team is preparing to launch their first web application on Azure. Before the app can go live, you need to set up the environment where it will run. This means start by creating an App Service Plan in the Azure Portal, the foundation that defines the app's region, pricing tier, and computing resources.
1. Step1
   * Start the creation of a new App Service Plan in the existing resource group.
   * Give the App Service Plan a name of your choice and configure the plan to use Linux as the operating system.
   * Use East US for the region and choose Free for the pricing plan.
   * Skip to Review and Create, wait for the final validation and click Create.
   * Wait for the App Service Plan to deploy successfully.
2. When designing an App Service Plan, which factor most directly affects both performance and cost?

Depending on Plan

### Why use App Service?
Why use Azure App Service for hosting your applications?

Choose the two correct answers.
```
〇 It allows you to deploy applications quickly without managing servers.
〇 It provides automatic scaling based on application traffic.
It requires you to manually manage virtual machines and operating systems.
It only supports Windows-based applications.
```
*Well done! You've identified the key benefits of Azure App Service - speed, simplicity, and scalability without server management.*

### Create a simple Web App
You've been asked to quickly set up a demo website for your marketing team to showcase a new campaign. You'll deploy a Web App using the Azure Portal, and configure the necessary settings.
1. Step1
   * Go to App Services and create a new Web App in the existing resource group.
   * Give the Web App a name of your choice.
   * Use Python 3.10 as the runtime stack and East US for the region.
   * Configure the Web App to a use a new App Service Plan or use an existing plan if available.
   * Go to Review and Create, wait for the final validation and click Create.
   * Wait for the Web App to deploy successfully.
2. Step2
   * Open the newly created Web App.
   * Identify the default domain of the Web App on the Overview blade.
   * Open the URL listed in the default domain and wait for it to load.
   * Since this is the first time someone is accessing the newly created Web App, it might take a few moments.

The next exercises build on the project you've already started. For the best experience, try to complete this chapter's exercises in one session.
3. What heading text is displayed on the Web App page?

*Correct! This is the default heading shown when the web app has been created successfully but not yet customized.*

---
### Run a Web App from a ZIP package
You've just finished preparing your web app for deployment and want a smooth, reliable way to get it running in Azure. Instead of sending files one by one, imagine packaging everything neatly into a single bundle - ready to go in one move. This method provides faster startup, atomic deployments, and easier rollbacks, making it a recommended approach for managing application deployments.
1. These steps continue from the previous exercise. If you've lost your progress, please repeat the earlier instructions before moving on.
   * Create a new Storage Account with a unique name in East US region.
   * Leave most settings to their default and wait for the storage account to deploy successfully.
   * Once the Storage Account has been created, go ahead and add a new storage container called "web-packages".
   * Open web-packages container and upload the local file found at Desktop / Resources / webapp
   * This is the source code of the Web App project which will we will be using for this exercise.
2. Step2
   * Once the file was uploaded, right click on the file (webapp) and generate a new SAS (you do not need to change any settings) and copy the Blob SAS URL.
   * Navigate to your previously created Web App, go to Environment variables blade (under Settings group) and add variable WEBSITE_RUN_FROM_PACKAGE with the value of the Blob SAS URL.
   * This will configure the Web App to use the previously uploaded ZIP package as the source of the website.
3. Setp3
   * In order to make possible for the web app content to run, we also need to introduce a startup command.
   * To to that, navigate to the Configuration blade (under Settings) and copy this text in the Startup command field: gunicorn --bind=0.0.0.0:$PORT app:app
   * Don't forget to save before leaving this screen.
   * Restart the Web App and wait for the action to complete (it might take a few minutes).
     
Once restarted, you can also browse the Web App URL and see the new look of the website.
4. What is the main advantage of using the Run From Package feature in Azure Web Apps?
> Hint
> Think about how this feature reduces the complexity of copying and managing many files during deployment.

*Correct! Running directly from a single package file makes deployments much simpler and more predictable. Keep it up!*

### Configure Web App settings
One of the powerful aspects of Azure Web Apps is that you can adjust behavior without touching the application code. By using environment variables, you can control settings like messages, connection strings, or feature flags directly from the Azure portal. This means you can make quick changes, test different configurations, or roll out updates without needing to redeploy the app.
1. Srep1
   * These steps continue from the previous exercise. If you've lost your progress, please repeat the earlier instructions before moving on.
   * Navigate to your Web App, go to Environment variables blade (under Settings group) and add variable WELCOME_TEXT with a value of your choosing.
   * Restart the Web App and wait for the action to complete.
   * Open the website; pay attention to the text displayed on the home page.
2. Step2
   * Change the `WELCOME_TEXT` environment variable, restart the Web App and refresh the home page once again.
   * The variable is responsible for the greeting message displayed on the home page and it can be changed without deploying new code.
3. Step3
   What are the benefits of storing configuration settings in environment variables?
> Hint   
> Think about why environment variables exist - what problem do they solve when managing applications in production?

*Correct! Environment variables make configuration changes simple and flexible, without redeployment.*

---
### Scaling a Web App
When running apps in the cloud, one size doesn't fit all. Imagine a marketing campaign going live - suddenly, your website experiences a surge in traffic. Sometimes you need more power for a single instance, and other times you need more instances to handle demand. Azure Web Apps makes it easy to adjust resources through scaling - whether by giving your app stronger machines (scale up) or by adding more instances (scale out).
1. Step1
   * Create a new App Service plan using the Free tier or use an existing one.
   * Go to the Scale up blade (under Settings) of the App Service plan and choose a plan higher than the current one (for example, Basic B2).
   * This will allocate more computing power for your Web Apps (the details are described when choosing the plan).
Wait for the scaling up to be implemented.
