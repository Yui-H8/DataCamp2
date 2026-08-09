# Introduction to Model Context Protocol (MCP)
---
**Description**

Gone is the headache of integrating AI applications with disparate external systems! Now, every software application out there is talking the MCP language. Find out how MCP makes it easier than ever to connect AI applications to APIs, databases, and other external functionality using its protocol. Learn how to build your own MCP servers from the ground-up, including API and database connections, and integrate them with LLMs so it can make smart data lookups and trigger actions. Get started with the Model Context Protocol today!

---

### Getting to Know MCP
You're onboarding a new developer at a fintech startup that's building an AI-powered financial advisor that utilizes the Model Context Protocol (MCP). This new colleague is unfamiliar with MCP, so you're going to hold a knowledge sharing session so they can hit the ground running.

*Excellent work! You've correctly identified the roles of each MCP architecture component. Understanding these distinctions is crucial for building and debugging MCP-based AI applications.*

### Your First MCP Server
Time to get hands-on with your first MCP server! We've provided all the code for you here, which we'll teach in the next video, but take a look at the flow of the code:
1. An MCP server instance is defined with `FastMCP()`
2. A tool function `(convert_currency())` is written to perform some action; in this case, retrieving currency information from the Frankfurter API.
3. This function is converted into an MCP tool using the @mcp.tool() decorator.

* Take a look at the code provided to see how a function is converted into a tool for an MCP server.
* On line 43, test the MCP tool with an amount and your choice of currencies to convert to and from.
  
Note: You'll need to use the official currency codes, like GBP for the British pound sterling.

```python
# Create an MCP server instance
mcp = FastMCP("Currency Converter")

@mcp.tool()
def convert_currency(amount: float, from_currency: str, to_currency: str) -> str:
    """
    Convert an amount from one currency to another using current exchange rates.

    Args:
        amount: The amount to convert
        from_currency: Source currency code (e.g., 'USD', 'EUR', 'GBP')
        to_currency: Target currency code (e.g., 'USD', 'EUR', 'GBP')

    Returns:
        A string with the conversion result and exchange rate
    """
    # API endpoint for Frankfurter
    url = f"https://api.frankfurter.dev/v1/latest?base={from_currency}&symbols={to_currency}"

    try:
        # Make the API request
        response = requests.get(url)
        response.raise_for_status()

        # Parse the response
        data = response.json()

        # Get the exchange rate
        rate = data['rates'].get(to_currency)

        if rate is None:
            return f"Could not find exchange rate for {from_currency} to {to_currency}"

        # Calculate the converted amount
        converted_amount = amount * rate

        return f"{amount} {from_currency} = {converted_amount:.2f} {to_currency} (Rate: {rate})"

    except requests.exceptions.RequestException as e:
        return f"Error converting currency: {str(e)}"

print("Testing Currency Converter:")
result = convert_currency(amount=100, from_currency="USD", to_currency="EUR")
print(result)
```

*I hope you're excited as I am to get started! MCP servers like these make it much easier to integrate and exchange external data with AI systems. Let's now take a look at how you can begin writing your own MCP servers!*

---

### Building a Currency Converter MCP Server
Time to begin building your first MCP server! Here, you'll define an MCP server called 'Currency Converter' that consists of a single tool called `convert_currency()` to convert monetary values between different currencies.

This tool should take amount, from_currency, and to_currency parameters, and return a string containing the amount converted into the desired currency, along with the exchange rate used.

For example, `100 EUR = 88.04 GBP (Rate: 0.8804)`.
1. Instantiate an MCP server called "Currency Converter".
```
# Create an MCP server instance
mcp = FastMCP("Corrency Converter")
```
2. Create an MCP tool called convert_currency() that takes amount, from_currency, and to_currency arguments.
```python
# Create an MCP server instance
mcp = FastMCP("Currency Converter")

# Define the MCP tool
@mcp.tool()
def convert_currency(amount, from_currency, to_currency):
    pass
```
Hint:    
Use the `@mcp.tool()` decorator before the function definition to convert it into an MCP tool.
Use the standard Python function syntax to define `convert_currency():` `def function_name(arg1, arg2, ...):`.

3. Complete the API endpoint URL by setting the `base` and symbols query parameters to `from_currency` and `to_currency`, respectively.
```python
# Create an MCP server instance
mcp = FastMCP("Currency Converter")

# Define the MCP tool
@mcp.tool()
def convert_currency(amount, from_currency, to_currency):
    # API endpoint for Frankfurter
    url = f"https://api.frankfurter.dev/v1/latest?base={from_currency}&symbols={to_currency}"
```
4. Send a GET request to the url, extract the JSON from the response object, and calculate `converted_amount` by multiplying `amount` by `rate`.
```python
# Create an MCP server instance
mcp = FastMCP("Currency Converter")

# Define the MCP tool
@mcp.tool()
def convert_currency(amount, from_currency, to_currency):
    # API endpoint for Frankfurter
    url = f"https://api.frankfurter.dev/v1/latest?base={from_currency}&symbols={to_currency}"

    # 1. Make the API request
    response = requests.get(url)

    # 2. Extract the currency exchange rate from the response
    data = response.json()
    rate = data['rates'].get(to_currency)

    if rate is None:
        return f"Could not find exchange rate for {from_currency} to {to_currency}"

    # 3. Calculate the converted amount
    converted_amount = amount * rate
    return f"{amount} {from_currency} = {converted_amount:.2f} {to_currency} (Rate: {rate})"
      
print(convert_currency(amount=100, from_currency="EUR", to_currency="USD"))
```
```
100 EUR = 114.35 USD (Rate: 1.1435)
```
*Very nice work! Your currency converter tool is now functional for a human, but LLMs need a bit more help. Let's add a clear and comprehensive docstring and typing to the function arguments and return objects. This will greatly improve the reliability of these tool calls!*

### Adding Docstrings and Type Hints
Time to make your convert_currency() tool easier for LLMs to use through docstrings and type hints. Without this, the LLM may not be able to effectively choose which tool to call, or may pass values to the arguments incorrectly—both of which result in unreliable application performance!

An MCP server has already been instantiated using FastMCP and assigned to mcp.

* Add appropriate types to the function arguments and return object.
* Complete the docstring to match the three function arguments with their definitions.
```python
# Adding typing to the function arguments and return object
@mcp.tool()
def convert_currency(amount: float, from_currency: str, to_currency: str) -> str:
    # Complete the docstring with the function arguments
    """
    Convert an amount from one currency to another using current exchange rates.

    Args:
        amount: The amount to convert
        from_currency: Source currency code (e.g., 'USD', 'EUR', 'GBP')
        to_currency: Target currency code (e.g., 'USD', 'EUR', 'GBP')

    Returns:
        A string with the conversion result and exchange rate
    """

    url = f"https://api.frankfurter.dev/v1/latest?base={from_currency}&symbols={to_currency}"

    response = requests.get(url)
    data = response.json()
    rate = data['rates'].get(to_currency)

    if rate is None:
        return f"Could not find exchange rate for {from_currency} to {to_currency}"

    converted_amount = amount * rate
    return f"{amount} {from_currency} = {converted_amount:.2f} {to_currency} (Rate: {rate})"

print(convert_currency(amount=100, from_currency="EUR", to_currency="USD"))
```
```
100 EUR = 113.77 USD (Rate: 1.1377)
```
*Beautifully documented! With this docstring and type hints, an LLM should be able to use this tool extremely effectively! Let's actually put that final piece into place: building an MCP client that can call your server.*

---
### Building Bridges (Between the Client and Server)
You're onboarding a new developer to your team who will be building MCP applications. They've read the documentation but are still confused about some core concepts around how clients and servers communicate in the MCP protocol.

To help them understand, you've prepared a quick quiz to test their knowledge of MCP client-server communication fundamentals.

* Classify each statement as either True or False based on what you learned about MCP client-server communications.

*Excellent work! You have a solid understanding of MCP client-server communication fundamentals. These concepts form the foundation for building effective MCP applications. Time to start talking with your currency converter server by building a corresponding client!*


### Dynamic Tool Discoverability
MCP servers can house lots of functionality as separate tools, so a common first step is to list out all of the tools available to understand what they are and how they can be used.
```
Hint
Use the StdioServerParameters class to define the stdio server parameters.
```
`asyncio` has already been imported for you.

1. Define a function called get_tools_from_mcp(), and start by defining the stdio server parameters so that the "currency_server.py" file is executed.
```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def get_tools_from_mcp():
    # Define the server parameters
    params = StdioServerParameters(
        command=sys.executable,
        args=["currency_server.py"],
    )
```
2. Connect to the MCP server and open a session; allow concurrent operations and automatic session closing.
   * Initialize the session.
```
Hint
Use the async with pattern to enable concurrent operations and to automatically close sessions once the operations have completed.
Use the session's .initialize() method to perform the initialization.
```
```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def get_tools_from_mcp():
    # Define the server parameters
    params = StdioServerParameters(
        command=sys.executable,
        args=["currency_server.py"],
    )

    # Connect to the MCP server and open a session
    async with stdio_client(params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            # Initialize the session
            await session.initialize()
```

3. Return the tools available in the server.
```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def get_tools_from_mcp():
    # Define the server parameters
    params = StdioServerParameters(
        command=sys.executable,
        args=["currency_server.py"],
    )

    # Connect to the MCP server and open a session
    async with stdio_client(params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            # Initialize the session
            await session.initialize()

            # Ask the server what tools it provides
            response = await session.list_tools()
```
4. Return each tool's name and description (in that order) from response.
