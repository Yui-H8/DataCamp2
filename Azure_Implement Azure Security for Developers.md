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
Authentication confirmed the employee’s identity, and authorization limited their access to HR applications.
Authentication and authorization are the same, so the employee only needed to log in once.
```
