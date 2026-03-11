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
```
import azure.functions as func
```
Add the following to line 2 to reference a library that can deal with data in the JSON format:
```
import json
```
Copy using Ctrl + C or Command + C depending if you're using a Windows or a Mac. You need to paste in the Azure interface using Ctrl + V given it's a Windows machine.

2. Configure the Function App
   Add the following line to reference an Azure Function:
```
app = func.FunctionApp(http_auth_level=func.AuthLevel.FUNCTION)
```
3. Define the static data that will represent our restaurant menu

Next add:
```python
MENU = {
    "starters": ["Tomato Soup", "Garlic Bread"],
    "mains": ["Margherita Pizza", "Pasta Carbonara"],
    "desserts": ["Tiramisu", "Gelato"]
}
```
Click here for further explanation
> This Python code defines a dictionary named MENU, which organizes a set of menu items into categories. The variable name representing the dictionary. It holds the entire menu data in a structured way. The outer {} braces indicate that MENU is a dictionary. A dictionary in Python stores key-value pairs. The keys in the dictionary are "starters", "mains", and "desserts". These represent the categories of menu items. The values are lists of strings (denoted by []), with each list containing the items for that category.

4. Define the HTTP trigger
   * Lastly, add the following code below:
```python
@app.route(route="restaurant_menu_items", methods=["GET"])
def get_menu(req: func.HttpRequest) -> func.HttpResponse:
    return func.HttpResponse(
        body=json.dumps(MENU),
        mimetype="application/json",
        status_code=200
    )
```
Save your code.
```
Hint
The complete Python code should be as follows:
```
```python
import azure.functions as func
import json

# Create the Function App
app = func.FunctionApp()

# Sample menu
MENU = {
    "starters": ["Tomato Soup", "Garlic Bread"],
    "mains": ["Margherita Pizza", "Pasta Carbonara"],
    "desserts": ["Tiramisu", "Gelato"]
}

# Define the route with auth level
@app.route(route="restaurant_menu_items", methods=["GET"], auth_level=func.AuthLevel.FUNCTION)
def get_menu(req: func.HttpRequest) -> func.HttpResponse:
    return func.HttpResponse(
        body=json.dumps(MENU),
        mimetype="application/json",
        status_code=200
    )
```
5. This is the endpoint code that will return the menu data when invoked. Let's test our function by getting its URL and pasting it into a browser. It should return restaurant menu data.

**Note: If there is an error when you paste the URL, navigate back to the Function app and click back into the function and get new URL**

Click here for further explanation
> The @app.route(...) is a decorator that registers the function as an HTTP-triggered Azure Function.
> route="restaurant_menu_items" specifies the URL route where the function is accessible. In this case, the function will be invoked via a URL ending in /restaurant_items_menu. methods=["GET"] defines that the endpoint is only accessible via a GET HTTP verb.
> ```
> def get_menu(req: func.HttpRequest) -> func.HttpResponse
> ```
> defines the function called whenever a request is made to the /restaurant_menu_items route. func.HttpResponse(...) constructs the HTTP response to send back to the client.

6. What is the purpose of the `route` parameter in the `@app.route()` invocation?

---
### Creating an Azure API management service
Now that we have built the API, the restaurant wants to securely share this API with delivery partners, such as food delivery apps. Using API Management, the restaurant can expose the API to external partners while controlling access, applying security policies, and monitoring usage. This ensures the menu data is consistently available, reliable, and protected, even as more partners and customers begin using it.

1. Setting up an API Management instance
　* Create an API Management service instance with a unique Resource name in the East US Region
Organization name and and Administrator email are mandatory, but we can provide any company name and any valid email
Select the Consumption pricing tier
> Click here to learn more about pricing tier SLA
> Consumption tier mentions SLA, which stands for Service Level Agreement. This is the level of availability that Azure guarantees. It's measured in percentage uptime, such as the SLA of 99.99% means that the service will be reachable 99.99% of the time. The higher is the availability - the more expensive is the pricing tier.
2. Finalize API Management
