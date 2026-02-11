# Develop Azure Event-based and Message-based Solutions
---
**Description**

This course teaches you how to build event-driven and asynchronous architectures in Azure and connect them to third-party services such as webhooks, messaging platforms, and external applications.


You’ll start by understanding the fundamentals of event-driven architecture and why it is essential for modern cloud systems. From there, you’ll work hands-on with Azure Event Grid, Event Hubs, Queue Storage, and Azure Service Bus to route, filter, and process events at scale.


Through practical exercises, you’ll learn how to publish and subscribe to events, trigger Azure Functions, integrate external systems, and design reliable messaging workflows. As the course progresses, you’ll explore advanced patterns like dead-lettering, retries, session-based processing, duplicate detection, and large-scale streaming pipelines.


By the end of the course, you’ll be able to design and implement scalable, resilient, and event-driven solutions that seamlessly connect Azure services with third-party systems—skills that are directly applicable to real-world cloud applications and Azure developer roles.

---
### Understanding event-driven architecture
A financial services company currently uses a polling-based system that checks for stock price changes every 30 seconds. Traders have complained that they are missing critical price movements because updates arrive too late. The development team is evaluating a move to event-driven architecture. What is the primary advantage of this approach over their current polling-based system?
```
It reduces the amount of data stored in the database.
It eliminates the need for network communication between services.
It notifies the application immediately when an event occurs, rather than waiting for the next scheduled check.
It guarantees that no events will ever be lost during transmission.
```
*Correct! Event-driven architecture pushes notifications to applications the moment something happens, eliminating the delay caused by polling intervals. This enables real-time responsiveness.*

### Setting up a basic event flow
The purpose of this exercise is to get familiar with how Event Grid resources are created. We will choose a multipurpose Event Grid resource, as it can be used by any application type, regardless of whether it's hosted in Azure or not. We will go into more details on event flows in the next video.   
1. **Choose an Event Grid Topic resource**   
* In Azure portal, search for Event Grid.
* Scroll down until you see the Topics section and create a resource under it.

2. **Create an Event Grid Topic**
* Choose an existing Resource group from the drop-down list, give the topic the name of "sale-events", and choose East US as the location.
* Create the resource, leaving all remaining options as their default values.

3. **Review the topic connection settings**
* Open the topic's Overview page and locate the Topic endpoint. You'll use this URL to publish events later.
* Expand the Settings section in the menu on the left and select the Access keys

Topic endpoint and one of the access keys allow applications to connect to an Event Grid topic securely and publish events to it. Typically, these values are securely stored in the settings or environment variables of the publisher app, so they aren't exposed via the code, which enhances application security.

4. **Review the topic connection settings**

Open the topic's Overview page and locate the Topic endpoint. You'll use this URL to publish events later.
Expand the Settings section in the menu on the left and select the Access keys
Topic endpoint and one of the access keys allow applications to connect to an Event Grid topic securely and publish events to it. Typically, these values are securely stored in the settings or environment variables of the publisher app, so they aren't exposed via the code, which enhances application security.

*The Access Keys page of an Event Grid topic has two access keys to choose from.*

### Setting up Event Grid
Place Event Grid in the most appropriate spot on this diagram. This diagram describes the following process:

1. User applies for a loan
2. The back-end web service asynchronously instructs an external Azure Function to generate the agreement, which may take a few minutes
3. The back-end web service synchronously creates a small entry in the database, recording the fact the user made a loan application
4. A document-generation process continues in the background and the document is saved into Blob storage once ready
```
1. User applies for a loan
〇 2. External Azure Function is instructed to generate the agreement
3. A record of the loan application is added to the database
4. Document is stored in Blob storage
```
*Event Grid is used here to outsource asynchronous and potentially long-running task to an external service, so it doesn't block the main thread.*

### Connecting a custom app to Event Grid
