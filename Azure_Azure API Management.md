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
