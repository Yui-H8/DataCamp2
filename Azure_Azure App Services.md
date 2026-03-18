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
