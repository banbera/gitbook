# Function Calling

Aesoperator allows LLMs (large language models) to dynamically spin up and utilize Operators through a serverless function calling architecture, similar to platforms like AWS Lambda and Cloudflare Workers.

## Serverless Functions

In a serverless model, code is packaged into discrete functions that can be invoked on-demand by external systems. The serverless platform dynamically provisions resources to execute the function, abstracts away infrastructure management, and scales automatically based on incoming requests.

This allows developers to build event-driven, composable applications without worrying about server operations. Functions can be triggered in response to HTTP requests, database changes, queue messages, scheduled jobs, and other external stimuli. 

AWS Lambda is a popular serverless compute service that runs functions in response to events and automatically manages the underlying compute resources. Cloudflare Workers provides a serverless execution environment for deploying functions to Cloudflare's edge network.

## Operators as Functions

Aesoperator applies this serverless model to AI agents called Operators. An Operator is packaged as a function that encapsulates a specific capability or skill. It can be dynamically invoked with input parameters to perform a task and return a result.

For example, you might define Operator functions for:

- Summarizing articles on a given topic 
- Extracting structured data from web pages
- Interacting with UI elements to complete a workflow
- Answering questions based on a knowledge base
- Generating images or text based on a prompt

Each Operator function is a self-contained unit of functionality that can be composed and orchestrated to build complex automations.

## Invoking Operators

LLMs can discover and invoke Operator functions through Aesoperator's API or SDK. The basic flow is:

1. The LLM queries Aesoperator to get a list of available Operator functions and their input/output schemas

2. The LLM selects an Operator function to invoke and prepares the necessary input parameters

3. The LLM sends an invocation request to Aesoperator, specifying the function name and input payload

4. Aesoperator dynamically provisions a new instance of the Operator to execute the function

5. The Operator performs its task, which may involve interacting with web pages, external APIs, databases, etc.

6. The Operator returns its result payload to Aesoperator 

7. Aesoperator sends the result back to the invoking LLM

Here's a hypothetical example of an LLM invoking an Operator function to summarize a Wikipedia article:

```python
# LLM queries Aesoperator for available functions
functions = aesop.list_functions()

# LLM selects the `summarize_article` function 
summarize_func = functions["summarize_article"]

# LLM prepares the input parameters
input_params = {
  "url": "https://en.wikipedia.org/wiki/Artificial_intelligence",
  "max_length": 500
}

# LLM invokes the function
result = aesop.invoke(summarize_func, input_params)

# LLM receives the summarized article text
summary = result["summary"]
```

Behind the scenes, Aesoperator spins up a new Operator instance, which fetches the specified Wikipedia page, extracts the relevant content, generates a summary, and returns it to the LLM.

## Composing Operators

LLMs can compose multiple Operator function calls to build more sophisticated workflows. For example, an LLM could:

1. Invoke an Operator to search for relevant articles on a topic
2. Invoke a summarization Operator on each of the top results 
3. Invoke a question-answering Operator to extract key insights from the summaries
4. Invoke a text-to-image Operator to generate visuals for the insights
5. Return the combined results to the end user

By breaking down complex tasks into smaller, reusable Operator functions, LLMs can dynamically assemble and orchestrate agent behaviors based on the specific needs of each user interaction.

## Use Cases

Some potential use cases for LLMs dynamically invoking Operator functions include:

- Chatbots that can retrieve and summarize information from external sources in response to user queries
- AI writing assistants that can gather research, generate drafts, and provide feedback and editing suggestions
- Intelligent task automation flows that can interact with web pages, APIs, and databases to complete multi-step processes
- Autonomous agents that can continuously learn and adapt their behaviors based on interactions with users and the environment

The serverless function calling model provides a flexible, scalable way for LLMs to access and compose AI capabilities to build more intelligent, open-ended applications.

## Learn More

- [Core Concepts](core-concepts.md) for an overview of key Aesoperator abstractions like Tasks, Pages, and Memory
- [Creating Operator Functions](creating-functions.md) for a guide to defining your own Operator functions
- [Aesoperator API Reference](api-reference.md) for details on the function calling API and SDK
- [Sample Operator Functions](sample-functions.md) for example implementations of common Operator capabilities

