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

*I hope you're excited as I am to get started! MCP servers like these make it much easier to integrate and exchange external data with AI systems. Let's now take a look at how you can begin writing your own MCP servers!*

---

### Building a Currency Converter MCP Server
Time to begin building your first MCP server! Here, you'll define an MCP server called 'Currency Converter' that consists of a single tool called `convert_currency()` to convert monetary values between different currencies.

This tool should take amount, from_currency, and to_currency parameters, and return a string containing the amount converted into the desired currency, along with the exchange rate used.

For example, `100 EUR = 88.04 GBP (Rate: 0.8804)`.
