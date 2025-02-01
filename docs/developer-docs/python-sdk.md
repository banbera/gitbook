---
description: Note as of 0.1.1 Aesoperator SDK is still experimental and in development
---

# Python SDK

The Aesoperator Python SDK provides a convenient way to discover, invoke, and compose [Operator functions](function-calling.md) from your Python code.

## Installation

Install the SDK using pip:

```bash
pip install computer-agent
```

The SDK requires Python 3.7 or later. It depends on the `requests` library for making HTTP requests to the Aesoperator API.

## Authentication

To use the SDK, you first need to obtain an API key from your Aesoperator account dashboard. Then initialize the SDK client with your key:

```python
from aesoperator import Aesoperator

aesop = Aesoperator(api_key="your_api_key_here")
```

This client instance will automatically authenticate with the Aesoperator service and handle token refresh. You can also specify a custom API endpoint if needed:

```python
aesop = Aesoperator(api_key="your_key", api_base="https://api.aesoperator.com/v2")
```

## Discovering Functions

List the Operator functions available to your account:

```python
functions = aesop.list_functions()

for func in functions:
    print(func.name, func.description)
```

This returns a list of `AesopFunction` objects, with metadata like the function name, description, input and output schemas, and usage limits.

You can also search for functions by keyword:

```python
search_results = aesop.search_functions(query="website screenshot")
```

And retrieve an individual function by name:

```python
screenshot_func = aesop.get_function("take_screenshot")
```

## Invoking Functions

To invoke an Operator function, call the `invoke` method with the function name and input parameters:

```python
output = aesop.invoke("take_screenshot", url="https://example.com", format="png")
```

The input parameters are validated against the function's schema. The invocation happens synchronously and the method returns when the function completes.

For long-running functions, you can invoke them asynchronously:

```python
from aesoperator import InvocationMode

invocation = aesop.invoke("process_large_file", file_url="https://example.com/big_file.csv", 
                          _mode=InvocationMode.ASYNC)

invocation_id = invocation.id
print(f"Triggered async invocation: {invocation_id}")
```

This immediately returns an `AesopInvocation` object with metadata about the in-progress invocation. You can then poll for completion:

```python
import time

while True:
    status = aesop.get_invocation(invocation_id).status
    print(f"Invocation status: {status}")

    if status == "Completed":
        break
    
    time.sleep(1)

output = aesop.get_invocation(invocation_id).output
```

## Handling Results

Operator function results are returned as Python objects deserialized from JSON. The exact structure depends on the function's output schema, but often includes the main `output` payload along with metadata like processing `latency` and `logs`.

```python
output = aesop.invoke("summarize_text", text="Some long input text goes here")

summary = output.output
print(f"Summary: {summary}")

latency_ms = output.latency
print(f"Summarization took {latency_ms} milliseconds")
```

If the function returns an error, an `AesopError` exception is raised with details:

```python
try:
    output = aesop.invoke("flaky_function")
except aesoperator.AesopError as e:
    print(f"Function invocation failed: {e.message}")
    print(f"Error details: {e.details}")
```

## Composing Functions

You can use the SDK to compose multiple Operator function calls into a workflow. For example, to generate questions and answers from a web page:

```python
# Scrape the text from a web page
page_text = aesop.invoke("scrape_text", url="https://example.com")

# Generate questions based on the scraped text
questions = aesop.invoke("generate_questions", text=page_text)

# Generate answers for each question 
answers = []
for question in questions:
    answer = aesop.invoke("answer_question", question=question, context=page_text)
    answers.append(answer)
```

The SDK also provides a `compose` method to define a sequence of function calls as a reusable pipeline:

```python
def generate_qa_from_url(url):
    return aesop.compose(
        ("scrape_text", {"url": url}),
        ("generate_questions", {"text": "scrape_text.output"}),
        ("answer_questions", {"questions": "generate_questions.output", 
                              "context": "scrape_text.output"})
    )
```

Each step of the composition specifies the function to invoke and its parameters. You can reference the outputs of previous steps using the dot notation `step_name.output`.

Invoking the composed pipeline executes the steps in order and returns the final result:

```python
qa_pairs = generate_qa_from_url("https://example.com/some_article")
print(qa_pairs)
```

## Working with Pages and Memory

The SDK provides convenience methods for working with [Page](core-concepts.md#page) and [Memory](core-concepts.md#memory) objects in your function invocations.

To load a Page object from a URL and pass it to a function:

```python
page = aesop.get_page("https://example.com")

output = aesop.invoke("extract_links", page=page)
```

And to read and write key-value pairs from Aesoperator's persistent Memory:

```python
aesop.memory.set("user_id", 42)

user_id = aesop.memory.get("user_id")
print(f"Retrieved user ID: {user_id}")
```

You can also subscribe to changes in specific Memory keys and trigger function invocations:

```python
def handle_new_user(user_id):
    user_profile = aesop.invoke("fetch_user_profile", user_id=user_id)
    aesop.memory.set(f"user_profile:{user_id}", user_profile)

aesop.memory.subscribe("user_id", handle_new_user)
```

Now whenever the `user_id` key is updated in Memory, the `handle_new_user` function will be called with the new value.

## Examples

Here are a few examples demonstrating common patterns with the Aesoperator Python SDK.

### Web Scraping

Scrape a list of links from a web page and store them in Memory:

```python
page = aesop.get_page("https://news.ycombinator.com")

links = aesop.invoke("extract_links", page=page)

aesop.memory.set("hn_front_page_links", links)
```

### Question Answering

Generate an answer to a question by searching a set of documents and composing the most relevant passages:

```python
documents = [
    "https://example.com/textbook1", 
    "https://example.com/textbook2",
    "https://example.com/textbook3" 
]

def answer_question(question):
    search_results = aesop.invoke("semantic_search", query=question, documents=documents)
    
    relevant_passages = [
        passage 
        for doc in search_results[:3]
        for passage in aesop.invoke("extract_passages", document=doc)
    ]

    return aesop.invoke("summarize", 
                        passages=relevant_passages, 
                        question=question)

question = "What are the key differences between supervised and unsupervised learning?"
answer = answer_question(question)
print(f"Question: {question}")
print(f"Answer: {answer}")
```

### Automated Data Processing

Watch a database for new records and automatically process them using a sequence of Operator functions:

```python
import mydb 

def process_new_record(record_id):
    record = mydb.fetch_record(record_id)

    aesop.compose(
        ("validate_record", {"record": record}),
        ("extract_entities", {"text": "validate_record.output.description"}),
        ("enrich_entities", {"entities": "extract_entities.output"}),
        ("update_search_index", {"record_id": record_id, "entities": "enrich_entities.output"})
    )

mydb.subscribe("new_records", process_new_record)
```

This sets up a listener that triggers the `process_new_record` pipeline whenever a record is inserted into the `new_records` table. The pipeline validates the record, extracts key entities from its description field, looks up additional metadata for each entity, and updates a search index with the enriched record data.

## Learn More

* [Aesoperator API Reference](api-reference.md) for low-level details on the HTTP API
* [Operator Function Catalog](function-catalog.md) for a complete list of available functions and their schemas
* [Guides and Tutorials](guides-and-tutorials.md) for in-depth walkthroughs on building Aesoperator apps in Python

The Aesoperator Python SDK makes it easy to leverage powerful AI capabilities in your applications. You can compose Operator functions to automate a wide variety of tasks, from web scraping and data processing to question answering and content generation.

This guide covered the key features of the SDK, including function discovery and invocation, handling results, composing multiple functions, working with Pages and Memory, and common usage patterns.

For more details on the Aesoperator API and platform, check out the rest of the [developer documentation](./). If you have any questions or feedback, join our [community forums](https://community.aesoperator.com) or [contact support](mailto:support@aesoperator.com).
