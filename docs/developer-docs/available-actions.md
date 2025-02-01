# Available Actions

Core actions that will be available in Aesoperator in version 2.0.0:

## Browser Actions

```python
# Navigate to URL
navigate(url: str) -> None

# Click element
click(selector: str, wait_for_navigation: bool = False) -> None

# Type text
type(selector: str, text: str) -> None

# Get text content
get_text(selector: str) -> str

# Get attribute value
get_attribute(selector: str, attribute: str) -> str

# Take screenshot
screenshot(selector: str = None, full_page: bool = False) -> bytes

# Get page HTML
get_html() -> str

# Wait for element
wait_for_element(selector: str, timeout_ms: int = 5000) -> bool

# Check if element exists
element_exists(selector: str) -> bool

# Get all elements matching selector
get_elements(selector: str) -> List[Element]

# Scroll page
scroll(x: int = 0, y: int = 0) -> None
scroll_to_bottom() -> None
```

## Data Extraction

```python
# Extract structured data from page
extract_data(schema: Dict) -> Dict

# Extract all links
extract_links() -> List[str]

# Extract table data
extract_table(selector: str) -> List[List[str]]

# Extract text matching pattern
extract_text(pattern: str) -> List[str]

# Extract images
extract_images() -> List[Dict[str, str]]
```

## LLM Actions

```python
# Summarize text
summarize(text: str, max_length: int = 500) -> str

# Answer question from context
answer_question(question: str, context: str) -> str

# Generate text
generate_text(prompt: str, max_tokens: int = 100) -> str

# Classify text
classify_text(text: str, labels: List[str]) -> str

# Extract entities
extract_entities(text: str) -> List[Dict[str, str]]

# Sentiment analysis
analyze_sentiment(text: str) -> Dict[str, float]

# Vision model
see_image(image: bytes) -> str
```

## Memory Actions
### Simple CRUD operations to change what context is available

```python
# Set value in memory
memory_set(key: str, value: Any) -> None

# Get value from memory
memory_get(key: str) -> Any

# Delete from memory
memory_delete(key: str) -> None

# Clear all memory
memory_clear() -> None

# Get all memory keys
memory_keys() -> List[str]
```

## File Actions
### Writes to local file system
```python
# Read file
read_file(path: str) -> bytes

# Write file
write_file(path: str, content: bytes) -> None

# Download file
download_file(url: str, path: str) -> None

# Upload file
upload_file(path: str, url: str) -> None
```

## System Actions
### Directly interacts with the system and its environment it's running on. For example:
### On a Debian system, you can install packages or run commands.


```python
# Sleep/wait
sleep(seconds: float) -> None

# Log message
log(message: str, level: str = "info") -> None

# Get current timestamp
timestamp() -> int

# Generate random string
random_string(length: int = 16) -> str
```

## Error Handling

All actions can throw these exceptions:
- `ActionError`: Base error class
- `TimeoutError`: Action timed out
- `ValidationError`: Invalid parameters
- `AuthError`: Authentication failed
- `RateLimitError`: Rate limit exceeded
- `CaptchaError`: CAPTCHA challenge detected

## Usage Limits

- Max concurrent actions: 5
- Max action duration: 30 seconds
- Rate limits vary by action type
- Memory storage: 100MB per task
- File size: 50MB max
- API rate limits: Varies by endpoint