# Azure API Management
---

**Description**

In this course, you’ll learn how to create, publish, and protect APIs using Azure API Management. You’ll start by setting up an API Management instance and documenting APIs for developers. Next, you’ll explore how to control access with subscriptions and keys, and apply policies to secure and optimize traffic. Finally, you’ll see how API gateways make services easier to manage and scale, with hands-on practice in the Azure portal.

---

### Weather API: invoking and formatting a response
Modern apps often fetch data from a web API and then shape the response before showing it to users. In this exercise, you'll simulate calling a weather API for two cities and produce a single JSON payload with a parent key. This mirrors sending a GET request and rendering the resulting JSON.

* Use the provided API to fetch current weather for London and Paris.
* Combine the two results into one response object grouped under a single parent key.
* Print the JSON response.
```python
# Fetch weather for two cities
london_weather = get_weather('London')
paris_weather = get_weather('Paris')

# Combine under a parent key
payload = {'weather': [london_weather, paris_weather]}

print("Current weather report:")
respond(payload)
```
```
Current weather report:
{
  "weather": [
    {
      "city": "London",
      "temp_c": 18,
      "condition": "Cloudy",
      "humidity": 72
    },
    {
      "city": "Paris",
      "temp_c": 21,
      "condition": "Sunny",
      "humidity": 55
    }
  ]
}
```
*Nicely done! You invoked the API for two cities, wrapped the results under 'weather', and emitted a clean JSON payload.*

---
### HTTP verbs and RESTful architecture
Let's test our understanding of HTTP verbs and how they are used in a RESTful API architecture. We will do so by assigning a specific action type to the most appropriate HTTP verb according to the RESTful architecture standards.

*Great job! You now understand how different HTTP verbs are used.*

### Setting up a Function App
Imagine you’re building a weather application that shows users the forecast for their city. To make this work, you need a lightweight backend API that can process requests—such as retrieving weather data for a given location—and return it quickly to the app. Azure Function Apps are ideal here because they allow you to deploy APIs with minimal setup, automatically scale to handle traffic spikes (like during a storm), and keep costs low when demand is quiet.
1. Create an Azure Function App instance and enable it to run Python code
   * Create a Function App with the Consumption pricing tier
   * Assign the available Resource group
   * Give the app a Name of your choice
   * Select Python as the Runtime stack with East US as the Region
   * Select 3.12 as the version
   * Review + create the resource
     
The resource may take a few moments to provision

2. Create an HTTP trigger
   * Once created, Go to resource and navigate to Create function
   * Add a HTTP Trigger function to it
   * Enter restaurant_menu_items exactly as the Name of the function and save it
```
Hint
Either click on the Go to resource link immediately after the Function App has been created or navigate to the resource via the Portal home page
Click the Create function button
Choose HTTP Trigger and click Next
Type "restaurantmenuitems" in the Provide a function name field and click Create
Navigate to the function to see the code
```
3. Test the Function
   * Navigate to it and examine its Python code
   * Examine the url by clicking the Get function URL button
   * Copy any of the URLs from the dialog that opens into the address bar of any browser
   * If you want to use key shortcuts for copying and pasting, use Ctrl + C and Ctrl + V, as the virtual machine runs Windows

If the function works correctly, the message displayed in the browser upon pasting the URL should be as follows:

**This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response**

4. What is the purpose of a HTTP trigger?

*Nice work! The purpose of the HTTP triggers is to associate executable logic with a specific HTTP endpoint address. 
For a broader look at serverless execution and use cases, see: Azure Functions overview on Microsoft Learn*

### Building a REST API in Python
Imagine you are developing an online ordering system for a restaurant. The mobile app needs a backend service that can provide customers with the current menu whenever they open the app.

To support this, you’ll create a simple API endpoint named restaurantmenuitems that returns a static JSON list of menu options. Later, this API could be expanded to fetch live data from a database or adjust menu items based on availability, but for now, this exercise will get you started with the basics.
1. Import required dependencies into the Azure function

Open the function you created in the previous exercise and remove all the code except for the following line:
