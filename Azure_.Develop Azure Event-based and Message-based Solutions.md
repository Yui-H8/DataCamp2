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
Imagine you are building a message board. Once a new message appears on it, all users and applications that monitor the message board are notified of it immediately. In this exercise, you will build a webhook endpoint that will facilitate this. An Azure Function will be triggered via an HTTP request. When it's triggered, it will send an event to the Even Grid.

You will create this function, connect it to a real Event Grid instance, and test it by sending an HTTP request to it.

1. **Find Azure Function App**
Find Azure Function App

* Search for the Function App with the name of function-app.. and open it.   
> Note: you may need to refresh the screen if you can't find the function app on the first attempt.

2. **Obtain Event Grid connection settings**
* Open a new tab in the learner environment and navigate to portal.azure.com.
* Find an Event Grid Topic with the name of egtopic-... open it, and find both the Topic endpoint URL and access key.
* In the original Function App, expand Settings, choose Environment Variables, add the following environment variables, then save and confirm the settings:
  - Name - "EVENT_GRID_TOPIC_ENDPOINT"; Value - the topic endpoint value.
  - Name - "EVENT_GRID_TOPIC_KEY"; Value - one of the keys from Access keys.
* Apply these changes.

> Note: deployment slot checkbox can be left unchecked while adding environment variables.

3. **Create HTTP Function trigger**    
Since the connection settings are stored in environment variables, they are stored securely and we can update them easily without having to rewrite the code. We will now add the code that will consume them.
* Go back to the Overview page of the Function App and create a new Function from there.
* Create an HTTP trigger with the name of publish_message with the default authorization setting

4. **Add the JavaScript trigger function**
* Replace all existing code with the following, which you can copy by using Ctrl + C on Windows or Linux or Command + C on Mac, and paste by using Ctr + V on any machine:
```JavaScript
module.exports = async function (context, req) {
    const endpoint = process.env.EVENT_GRID_TOPIC_ENDPOINT;
    const key = process.env.EVENT_GRID_TOPIC_KEY;
    if (!endpoint || !key) throw new Error("Missing endpoint or key");

    const event = {
      id: (globalThis.crypto?.randomUUID?.() ?? `${Date.now()}-${Math.random()}`),
      subject: "/demo/app",
      eventType: "MessageCreated",
      dataVersion: "1.0",
      data: { message: "Hello from Event Grid!" },
      eventTime: new Date()
    };

    const res = await fetch(endpoint, {
      method: "POST",
      headers: {
        "aeg-sas-key": key,
        "Content-Type": "application/json"
      },
      body: JSON.stringify([event])
    });

    const text = await res.text();
    context.log("Publish response:", res.status, text);
    context.res = { status: res.ok ? 200 : 500, body: res.ok ? "Event published" : text };
};
```
* Save the change.
> Once an HTTP request is sent to the Function endpoint, the JavaScript code inside the function sends the request to the Event Grid Topic. The endpoint URL is extracted from environment variables via process.env.EVENT_GRID_TOPIC_ENDPOIN. The secret key is extracted from process.env.EVENT_GRID_TOPIC_KEY. The event variable holds the event payload. The await fetch invocation submits this payload to the endpoint via a POST HTTP request, placing the secret key into the aeg-sas-key header.

5. **Test the function**
* Open the Get function URL dialog from the top of the screen
* You will see three function URLs. Copy any of then into a new tab.
* You should see a message indicating that an event has been sent successfully.

6. **How did you extract Event Grid Topic endpoint and its key from environment variables?**
*process.env contains the full collection of environment variables available to the process that runs the code.*

### Building an event subscriber
In this exercise, we will add Function App that can subscribe to an Event Grid topic. Such an app, which can be anything and not necessarily Azure Function App, receives the events from other services. This is how, for example, a mobile app receives notifications.

To demonstrate how Event Grid can share events between completely different applications, you will create a separate instance of Azure Function to act as the event subscriber.

1. Add an Event Grid subscriber function
* Find the Azure Function with the name function-app... if you aren't on it already.
* Create a new function. Choose the Event Grid trigger and give it a name of "MessageSubscriber".
* Once created, replace the code with the following and save the changes:
```
module.exports = async function (context, azeventgrid) {
  context.log("Event received:", JSON.stringify(azeventgrid));

  const data = (azeventgrid && azeventgrid.data) || {};
  context.log("Event Data:", JSON.stringify(data));

  const message = (data && data.message) ? data.message : "No message provided";
  context.log(`Processed Message: ${message}`);
};
```
> Click here for further explanation    
This code is executed when a message is placed in an Event Grid Topic that the function is bound to. Inside this endpoint, we first log the event data, i.e. the whole JSON the event consists of. Then, we separately log the message extracted from the event data JSON.

2. Connect the function to the Event Grid subscription
* Open the Integration blade on top
* Navigate to the Event Grid Trigger (eventGridEvent) link in the trigger card.
* In the dialog that opens, choose the Create Event Grid subscription.
  
You should be taken to a new page where a subscription is configured.

3. Complete the subscription integration
* Give your subscription the name of "message-subscription". In the Event Schema drop-down list select Event Grid Schema.
* From the Topic Types drop-down list, select Event Grid Topic. Select the available value in the Subscription field. Select the available value in the Resource group drop-down. Select the egtopic-... Event Grid Topic in the Resource drop-down. Once all the options have been selected, create the subscription.
> Click here for further explanation   
Please note that the subscription can also be created from Event Grid Topic. The process is similar, but works in the opposite direction. Instead of selecting the Event Grid Topic, you will need to select the Azure Function with an Event Grid trigger. The end result will be identical.

4. Test the subscription
If you still have the HTTP trigger function from the previous exercise, you can test the event flow end to end.
* Paste the URL of the original HTTP trigger function into a new browser tab.
* Go to the egtopic-... Event Grid instance.
* Navigate to the message-subscription subscription.
* See that the graph is showing a processed event.

If the event is shown in the graph, it means it has been received and processed by the Event Grid Trigger function.

5. How would you configure a subscription to a specific Event Grit topic inside an Azure Function with an Event Grid trigger?

*How would you configure a subscription to a specific Event Grit topic inside an Azure Function with an Event Grid trigger?*

---
### Setting up an MQTT(Message Queuing Telemetry Transport) broker
Event routing is an important feature of Event Grid. It enables Event Grid to translate MQTT messages, such as IoT device telemetry, into standard Event Grid messages that can be handled by any applications, such as Azure Functions. In this exercise, we will set up Event Routing, which would be the first step to connect IoT devices, such as wearables and sensors, to our system.

1. Create an Event Grid Namespace
In Event Grid, event routing functionality is enabled in a multipurpose Event Grid Namespace resource. Therefore, let's create an instance of this resource.
* Navigate to Event Grid and create a Namespaces Event Grid.

Hint   
1. Select the available resource group from the Resource group drop-down list
2. Populate the Name field
3. Select East US from the Location drop-down list
4. Click Review + Create
5. Click Create

2. Configure the Event Grid Namespace
* Choose an existing resource group from the drop-down list give the namespace any valid unique name, and choose East US as the location.
* All remaining options can be left as their default values.

3. Enable the MQTT broker
* Once at the Overview page navigate to Configuration under the Settings blade.
* Tick the Enable MQTT broker box. Apply the changes and wait for the changes to be applied.

It may take a few minutes for the broker to get provisioned. Once created, make sure you complete the next exercise in the same sitting to make sure the resource is still available.

4. Why do you need to enable MQTT broker in order to enable event routing in an Event Grid Namespace?
```
Hint
Event routing is specifically designed to route MQTT messages to an Event Grid topic that other applications can subscribe to. Therefore, to enable event routing, we would need to enable MQTT broker.
```

### Configuring event routing
In this exercise, we will finish setting up event routing functionality. Once done, we will have an MQTT broker fully set up with event routing, ready for an IoT Hub to connect to it.

Once set up, all MQTT messages that go into this instance of Event Grid will be routed to this topic. Different clients, such as different types of IoT devices, can start start sending their telemetry to this topic via the MQTT protocol. Any subscribers to the topic can then receive and handle this data.

1. Create a namespace topic
