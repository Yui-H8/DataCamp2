# Implement Azure Security for Developers
---
**Description**

In this course, you’ll learn how to manage and secure users, apps, and resources in Azure using Microsoft Entra ID. You’ll explore tools like Azure Key Vault to protect secrets, and Azure App Configuration to organize app settings. Then, you’ll see how services can talk to each other securely using Managed Identities, and how Microsoft Graph helps automate identity tasks—all with hands-on practice in the Azure portal.

---
### Microsoft Entra ID overview
At Peoplesphere, managing secure access is essential for employees and customers. Before we dive into how to manage identities and permissions, let’s first explore the platform you’ll be working with: Microsoft Entra ID.
1. Open Microsoft Entra ID
   * Navigate to Microsoft Entra ID from the Azure portal.
   * Open the Entra admin center by selecting Go to Microsoft Entra in the Try Microsoft Entra admin center card.
2. Explore Entra ID core sections
   * Expand the Entra ID section from the left-hand menu and glance at the various options available. They are crucial when it comes to security.
3. Microsoft Entra ID helps you explore identities and manage access control. Which of the following can you not manage in Entra ID?

Answer: Databace

*Correct! Databases are not managed in Entra ID. They’re handled by services like Azure SQL or Cosmos DB. For helpful background on identity fundamentals, see: What is Microsoft Entra on Microsoft Learn*

### Selecting the right security tool
At Peoplesphere, security is a top priority. You’ve already seen a few Azure tools that help keep applications safe. Now, let’s match each tool to the key role it plays in protecting apps and data.

*Nice work! You matched the tools correctly. Keep it up!*

---
### Creating users in Entra ID
Let’s step into the world of Peoplesphere and see how new user creation works in Microsoft Entra ID. We’ll explore what details are important to provide when setting up a user and why they matter for managing identity and access securely.
1. Creating a new user
   * Navigate to the Overview page in the Entra ID admin centre.
   * Create a new User by the + Add button at the top choosing Create new user.
2. Exploring user creation

Due to security permissions you will not be able to create a new user, however lets take a moment to understand the process and fields:
* Basics   
Includes required fields like User principal name and Display name.
* Properties   
Covers optional fields such as Identity, Job, and Contact Information.
* Assignments   
Used for assigning roles to the user.  
* Review + create   
Final step to review and validate all entered details.


3. You need to add a contractor to your Microsoft Entra directory who does not belong to your organization but requires temporary access to certain resources. What type of user should you create?
   
   Answer: Guest user     
*Correct! Guest users work best for temporary access.*

---
### Secure access
At Peoplesphere, an employee signs in to the Azure portal with their username and password. After logging in, only HR applications are visible; financial and IT management apps are not accessible.

Which statement best explains what is happening?
```
Authentication gave the employee access to all applications, but authorization removed some later.
Authorization verified the employee’s identity, and authentication decided which apps they could open.
〇 Authentication confirmed the employee’s identity, and authorization limited their access to HR applications.
Authentication and authorization are the same, so the employee only needed to log in once.
```
*Correct. Authentication verifies identity at sign-in; authorization uses roles/permissions to restrict access to specific apps and data.*

### Authentication vs. authorization
At Peoplesphere, keeping employee and customer information secure is essential. Authentication and authorization may sound similar, but understanding the difference between them is key to strong identity management.

*Great job! You've correctly classified the authentication and authorization scenarios.*

---
### Application object vs service principal
In Microsoft Entra ID, two objects define how apps are secured and used across tenants: the Application Object and the Service Principal.

*Well done! You've correctly identified the difference between the app blueprint and its tenant-level identity.*

### App-only access scenarios
The PeopleSphere app sometimes acts on behalf of a signed-in user, while in other cases it runs independently in the background.

In this exercise, you’ll decide which scenario represents App-only access.
```
PeopleSphere’s app prompts the user to sign in before accessing their OneDrive files via Microsoft Graph.
〇 Azure Logic App uses its managed identity to retrieve secrets from Azure Key Vault without user interaction.
An employee signs into the PeopleSphere web portal to view their calendar through Microsoft Graph.
A script runs interactively in PowerShell using a user account to manage Azure resources.
```

*Well done! You correctly identified the app-only access scenario.*

---
### Choosing the right SAS
Your team is building a web app that lets employees view training videos stored in Azure Blob Storage. The app signs in users with Microsoft Entra ID, and you want to give them temporary, read-only access to just the video blob.

Which type of SAS should you use?
```
〇 User Delegation SAS
Account SAS
Service SAS
Connection String with Account Key
```
*Perfect!User Delegation SAS supports Entra ID based authentication and is ideal for secure, scoped access to blob storage without using account keys.*

### Creating a SAS token
PeopleSphere, is launching a limited-time promotion for email subscribers. They’ve asked for a way to securely store promotional content in Blob storage and grant access only during specific hours. To achieve this, you can generate a Shared Access Signature (SAS) that restricts access to the content within the defined time window. This ensures that only the intended audience can view the promotion at the right time.
1. Let’s create a SAS token inside a Storage account:
   * Navigate to Storage accounts and open the pre-made storage account.
   * Under Properties navigate into Blob service.
   * Create a container and name it blob-container
2. Let’s upload an image to your Blob Container:
   * Open blob-container and select Upload.
   * In the pop up Browse for files.
   * Navigate to the Desktop/Resouces folder and select the image Promo and Upload it.
