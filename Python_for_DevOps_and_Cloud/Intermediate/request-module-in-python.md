# 05 — Python Requests Module for DevOps & Cloud

## Prerequisites

* Basic Python: variables, functions, `if/else`, and exceptions
* Basic understanding of HTTP and APIs
* Python environment management: `venv`, `pip`, and `requirements.txt`
* Basic understanding of JSON

## 1. What Is the `requests` Module?

The `requests` module is a Python library used to send HTTP requests to servers and receive responses.

In simple words, it allows Python to communicate with APIs over HTTP or HTTPS.

Install it:

```bash
pip install requests
```

Example:

```python
import requests

response = requests.get("https://httpbin.org/get") # httpbin is a website used to test requests are working or not

print(response.status_code)
print(response.text)
```

### How it works

```text
Python Script
     |
     | HTTP Request
     v
API / Web Server
     |
     | HTTP Response
     v
Python Script
     |
     v
Read the response
```

### Where is it used in DevOps and Cloud?

* Calling AWS or other cloud APIs.
* Checking whether an application is healthy.
* Downloading configuration files and software resources.
* Triggering deployments through APIs.
* Fetching monitoring metrics.
* Communicating with CI/CD tools and other services.
* Automating routine API operations.

**Important:** `requests` is an HTTP client library. It does not automatically deploy applications or manage cloud infrastructure; your Python code must call the appropriate API.

---

## 2. HTTP Request Methods

HTTP methods tell a server what action the client wants to perform.

| Method    | Purpose                                        | DevOps/Cloud example           |
| --------- | ---------------------------------------------- | ------------------------------ |
| `GET`     | Retrieve information                           | Get deployment status          |
| `POST`    | Submit data or create a resource               | Trigger a deployment           |
| `PUT`     | Replace a resource or its representation       | Replace configuration          |
| `PATCH`   | Partially update a resource                    | Update one configuration field |
| `DELETE`  | Delete a resource                              | Delete a test resource         |
| `HEAD`    | Get response headers without the response body | Check resource metadata        |
| `OPTIONS` | Ask which communication options are supported  | Discover allowed HTTP methods  |

**Correction:** There is no standard HTTP method named `UPDATE`. Use `PUT` or `PATCH`, depending on the operation.

### Example: GET

```python
import requests

response = requests.get(
    "https://httpbin.org/get",
    timeout=10
)

print(response.status_code)
print(response.json())
```

`GET` retrieves information. Here, `timeout=10` limits how long the client waits for a response.

### Example: POST

```python
import requests

url = "https://httpbin.org/post"

payload = { # "payload" refers to the data sent in a request Body.
    "service": "backend",
    "environment": "production"
}

response = requests.post(
    url,
    json=payload,
    timeout=10
)

print(response.status_code)
print(response.json())
```

`POST` sends data to the server. In a real deployment system, an API might use this method to trigger a deployment.

### Example: PUT

```python
requests.put(
    "https://api.example.com/config/10",
    json={"environment": "production"},
    timeout=10
)
```

`PUT` commonly replaces a resource's representation. The exact behavior depends on the API.

### Example: PATCH

```python
requests.patch(
    "https://api.example.com/config/10",
    json={"environment": "staging"},
    timeout=10
)
```

`PATCH` is commonly used to change only selected fields.

### Example: DELETE

```python
requests.delete(
    "https://api.example.com/test-resources/10",
    timeout=10
)
```

This requests deletion of the specified resource. Whether deletion succeeds depends on the server and your permissions.

### Example: HEAD

```python
response = requests.head(
    "https://example.com",
    timeout=10
)

print(response.status_code)
print(response.headers)
```

`HEAD` retrieves response headers without the response body.

### Example: OPTIONS

```python
response = requests.options(
    "https://example.com",
    timeout=10
)

print(response.headers.get("Allow"))
```

The server may return an `Allow` header listing supported methods. Not every server implements `OPTIONS` the same way.

**DevOps rule:** Use the HTTP method intended by the API documentation. Do not assume that every API supports all methods.

---

## 3. Calling an API Using Python

An API lets one application communicate with another application.

Suppose a monitoring script needs to retrieve the health status of a service.

```python
import requests

url = "https://api.example.com/health"

response = requests.get(url, timeout=10)

response.raise_for_status()

data = response.json()

print(data)
```

### Explanation

1. `requests.get()` sends a GET request.
2. `timeout=10` sets a timeout.
3. `raise_for_status()` raises an HTTP error for unsuccessful HTTP status codes.
4. `response.json()` converts a JSON response into Python data.
5. `print(data)` displays the result.

The URL above is an example. A real script must use an actual API endpoint.

### Typical API flow

```text
Python Script
     |
     v
Send HTTP Request
     |
     v
API Server
     |
     v
Process Request
     |
     v
Return HTTP Status + Data
     |
     v
Python Handles Response
```

### DevOps use cases

* Check service health.
* Fetch deployment details.
* Trigger CI/CD jobs.
* Retrieve cloud resource information.
* Read metrics from monitoring APIs.

---

## 4. Resource Downloading

Python can download files from a URL, such as a configuration file, report, or public software resource.

### Download a file

```python
import requests

url = "https://example.com/file.zip"

with requests.get(url, stream=True, timeout=(5, 60)) as response:
    response.raise_for_status()

    with open("file.zip", "wb") as file:
        for chunk in response.iter_content(chunk_size=8192):
            if chunk:
                file.write(chunk)

print("Download completed")
```

### Important points

* `stream=True` allows the response body to be read in chunks rather than loaded into memory all at once.
* `timeout=(5, 60)` sets a 5-second connection timeout and a 60-second read timeout.
* `raise_for_status()` stops the download if the server returns an HTTP error.
* `"wb"` writes binary data, which is suitable for ZIP files and other non-text resources.
* `with` ensures the file and response are closed when their blocks finish.

### DevOps use cases

* Download build artifacts.
* Fetch reports from APIs.
* Download configuration or data files.
* Retrieve files from internal services.

**Security note:** For production downloads, verify the source and, where applicable, the file's checksum or signature. Never execute a downloaded file merely because the download succeeded.

---

## 5. Authentication Mechanisms

Authentication is how an API verifies who is making the request.

Common mechanisms include:

* API keys
* Bearer tokens
* Basic authentication
* OAuth 2.0 access tokens

The API documentation determines which mechanism is required.

### A. API key

Some APIs require a key in a request header.

```python
import os
import requests

api_key = os.environ["API_KEY"]

response = requests.get(
    "https://api.example.com/resources",
    headers={"X-API-Key": api_key},
    timeout=10
)

response.raise_for_status()
print(response.json())
```

The header name `X-API-Key` is an example. Use the header specified by the API.

### B. Bearer token

Many APIs use bearer tokens.

```python
import os
import requests

token = os.environ["API_TOKEN"]

response = requests.get(
    "https://api.example.com/resources",
    headers={"Authorization": f"Bearer {token}"},
    timeout=10
)

response.raise_for_status()
print(response.json())
```

The token should be supplied securely.

### C. Basic authentication

```python
import os
import requests

response = requests.get(
    "https://api.example.com/resources",
    auth=(
        os.environ["API_USERNAME"],
        os.environ["API_PASSWORD"]
    ),
    timeout=10
)

response.raise_for_status()
```

Basic authentication encodes credentials; it does not encrypt them by itself. Use HTTPS.

### D. OAuth 2.0

OAuth 2.0 is commonly used when an application needs an access token to call another service. Your script generally obtains a token through the provider's supported flow and then sends it as a bearer token.

### DevOps best practices

* Never hardcode secrets in Python files.
* Use environment variables or a secret manager.
* Do not print tokens or passwords in logs.
* Give automation accounts only the permissions they need.
* Rotate credentials when required.

---

## 6. Payload Management: `data` vs `json`

A payload is the data sent in an HTTP request body.

In `requests`, `data=` and `json=` are used for different purposes.

### Using `json=`

```python
import requests

payload = {
    "service": "backend",
    "replicas": 3
}

response = requests.post(
    "https://httpbin.org/post",
    json=payload,
    timeout=10
)

print(response.json())
```

`json=` serializes the Python dictionary into JSON and sets the JSON content type automatically.

### Using `data=`

```python
import requests

payload = {
    "username": "devops-user",
    "role": "admin"
}

response = requests.post(
    "https://httpbin.org/post",
    data=payload,
    timeout=10
)

print(response.status_code)
```

When `data=` receives a dictionary, `requests` normally sends form-encoded data rather than JSON.

### Difference

| Feature             | `json=`                | `data=`                                   |
| ------------------- | ---------------------- | ----------------------------------------- |
| Typical use         | JSON API payload       | Form data or raw request body             |
| Dictionary behavior | JSON serialization     | Form encoding                             |
| Content type        | Sets JSON content type | Usually form URL-encoded for a dictionary |
| Common example      | REST API               | HTML form submission                      |

**Remember:** If an API expects JSON, prefer `json=payload`. If it expects form data or a specific raw body, use `data=` as documented.

---

## 7. Timeouts

A timeout prevents your script from waiting indefinitely for a server.

Without a timeout, a slow or unresponsive service can delay automation.

### Example

```python
import requests

try:
    response = requests.get(
        "https://httpbin.org/delay/5",
        timeout=3
    )
    response.raise_for_status()
    print(response.status_code)

except requests.exceptions.Timeout:
    print("The request timed out")
```

The endpoint intentionally delays its response. Because the timeout is shorter than the delay, a timeout is expected.

### Connection timeout vs read timeout

```python
requests.get(
    "https://api.example.com/health",
    timeout=(3, 10)
)
```

* `3`: maximum time to establish a connection.
* `10`: maximum wait between incoming data while reading the response.

These are not necessarily a strict total wall-clock deadline for the entire operation.

### DevOps use cases

* Health checks.
* Cloud API calls.
* Deployment automation.
* Monitoring scripts.
* External service checks.

**Best practice:** Set explicit timeouts for network requests.

---

## 8. Error Handling and HTTP Status Codes

A server returns an HTTP status code to describe the result of a request.

### Important status codes

| Code          | Meaning               | Typical interpretation                             |
| ------------- | --------------------- | -------------------------------------------------- |
| `200`         | OK                    | Request succeeded                                  |
| `201`         | Created               | Resource created                                   |
| `204`         | No Content            | Request succeeded without a response body          |
| `301` / `302` | Redirect              | Resource moved or request redirected               |
| `400`         | Bad Request           | Invalid request                                    |
| `401`         | Unauthorized          | Missing or invalid authentication                  |
| `403`         | Forbidden             | Not permitted                                      |
| `404`         | Not Found             | Resource does not exist                            |
| `429`         | Too Many Requests     | Rate limit exceeded                                |
| `500`         | Internal Server Error | Server-side failure                                |
| `502`         | Bad Gateway           | Gateway received an invalid upstream response      |
| `503`         | Service Unavailable   | Service temporarily unavailable                    |
| `504`         | Gateway Timeout       | Gateway did not receive a timely upstream response |

### Example

```python
import requests

try:
    response = requests.get(
        "https://httpbin.org/status/404",
        timeout=10
    )

    response.raise_for_status()
    print(response.json())

except requests.exceptions.HTTPError as error:
    print(f"HTTP error: {error}")

except requests.exceptions.Timeout:
    print("Request timed out")

except requests.exceptions.ConnectionError:
    print("Could not connect to the server")

except requests.exceptions.RequestException as error:
    print(f"Request failed: {error}")
```

This example deliberately requests a 404 response, so the `HTTPError` handler is expected to run.

### Important distinction

`requests.get()` does not automatically raise an exception for a response such as `404` or `500`.

Use:

```python
response.raise_for_status()
```

to raise an HTTP error for unsuccessful status codes.

A successful HTTP response also does not guarantee that your business operation succeeded. Read the API response and follow its documentation.

---

## 9. Sessions and Connection Pooling

A `Session` lets multiple requests share settings such as headers, cookies, and connection pools.

Connection pooling can reuse existing connections instead of establishing a new connection for every request.

### Without a session

```python
import requests

response1 = requests.get(
    "https://httpbin.org/get",
    timeout=10
)

response2 = requests.get(
    "https://httpbin.org/get",
    timeout=10
)
```

### With a session

```python
import requests

with requests.Session() as session:
    session.headers.update({
        "User-Agent": "DevOps-Automation"
    })

    response1 = session.get(
        "https://httpbin.org/get",
        timeout=10
    )

    response2 = session.get(
        "https://httpbin.org/get",
        timeout=10
    )

    response1.raise_for_status()
    response2.raise_for_status()

    print(response1.status_code)
    print(response2.status_code)
```

### Why use a session?

* Reuse connections where possible.
* Share common headers.
* Reuse cookies.
* Reduce repeated connection setup for multiple requests.

### DevOps use cases

* Querying many cloud resources.
* Polling monitoring APIs.
* Calling multiple endpoints in an automation job.

Use a session when making multiple related requests to the same service. Close it when finished.

---

## 10. Automatic Retries

Sometimes a request fails temporarily because of network problems, rate limits, or server errors.

A retry attempts the request again.

However, retrying every failure is not a good idea.

For example, retrying an invalid password will not normally fix a `401` response.

### Example using `HTTPAdapter` and `Retry`

```python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

retry_strategy = Retry(
    total=3,
    backoff_factor=1,
    status_forcelist=[429, 500, 502, 503, 504],
    allowed_methods=["GET", "HEAD", "OPTIONS"],
    respect_retry_after_header=True
)

session = requests.Session()

adapter = HTTPAdapter(max_retries=retry_strategy)

session.mount("https://", adapter)
session.mount("http://", adapter)

try:
    response = session.get(
        "https://httpbin.org/status/503",
        timeout=10
    )

    response.raise_for_status()
    print(response.status_code)

except requests.exceptions.RequestException as error:
    print(f"Request failed after retries: {error}")

finally:
    session.close()
```

This example deliberately requests a 503 response. It retries eligible responses and eventually reports failure.

### What do the settings mean?

* `total=3`: allows up to three retries under the configured retry policy.
* `backoff_factor=1`: introduces increasing delays between retries.
* `status_forcelist`: status codes that can trigger retries.
* `allowed_methods`: methods considered safe to retry by this policy.
* `respect_retry_after_header=True`: respects a server's `Retry-After` instruction when applicable.

### Important rules

* Retry temporary failures, not every failure.
* Do not blindly retry `POST` or other non-idempotent operations; they may create duplicate resources.
* Use an API-supported idempotency key when available for operations that must not be duplicated.
* Set timeouts even when retries are enabled.
* Account for retry delays when designing deployment deadlines.

### DevOps use cases

* Temporary cloud API failures.
* Rate-limited monitoring requests.
* Intermittent network issues.
* Health checks against services that may briefly be unavailable.

---

## 11. Handling Pagination

Some APIs return only a limited number of records per response.

For example, an API may return 100 resources at a time even if 1,000 resources exist.

This is called pagination.

### Example API response

```json
{
  "items": ["server-1", "server-2"],
  "next_page": 2
}
```

The next request must ask for page 2.

### Simple pagination example

```python
import requests

url = "https://api.example.com/resources"
page = 1
all_items = []

while page is not None:
    response = requests.get(
        url,
        params={"page": page},
        timeout=10
    )

    response.raise_for_status()
    data = response.json()

    all_items.extend(data["items"])
    page = data.get("next_page")

print(all_items)
```

This is an illustrative example. The API must return `items` and `next_page` in this format.

### Important points

* `params=` sends URL query parameters.
* `data.get("next_page")` returns `None` if the key is missing.
* The loop stops when the API indicates that no page remains.

Different APIs use different pagination styles:

* Page numbers.
* Offsets and limits.
* Next-page URLs.
* Cursor tokens.

Follow the API's documented format.

### DevOps use cases

* Fetch all cloud resources.
* Retrieve all monitoring events.
* List deployments across a project.
* Collect paginated logs or inventory records.

---

## 12. SSL Certificate Verification

HTTPS uses TLS to protect communication and authenticate the server.

By default, `requests` verifies the server's TLS certificate.

### Normal secure request

```python
import requests

response = requests.get(
    "https://api.example.com/health",
    timeout=10
)

response.raise_for_status()
print(response.status_code)
```

If the certificate cannot be verified, `requests` normally raises an `SSLError`.

### Using a custom CA certificate

In a company environment, an internal API may use a certificate issued by an internal Certificate Authority (CA).

You can provide the trusted CA certificate:

```python
import requests

response = requests.get(
    "https://internal-api.example.com/health",
    verify="/path/to/company-ca.pem",
    timeout=10
)

response.raise_for_status()
print(response.status_code)
```

Replace the path with the actual trusted CA certificate file.

### What if you set `verify=False`?

```python
requests.get(
    "https://api.example.com",
    verify=False
)
```

This disables certificate verification and exposes the request to man-in-the-middle risks.

**Do not use `verify=False` in production.** Fix the certificate or CA trust configuration instead.

### DevOps use cases

* Calling internal company APIs.
* Connecting to services with private CA certificates.
* Secure communication with cloud services.
* Debugging certificate configuration.

---

## 13. Query Parameters and Headers

These two concepts are important for API automation.

### Query parameters

Query parameters pass additional information through the URL.

```python
import requests

response = requests.get(
    "https://httpbin.org/get",
    params={
        "environment": "production",
        "page": 1
    },
    timeout=10
)

print(response.url)
```

`requests` builds the query string for you.

Example URL:

```text
https://httpbin.org/get?environment=production&page=1
```

### Headers

Headers provide additional information about a request.

```python
import requests

response = requests.get(
    "https://httpbin.org/headers",
    headers={
        "User-Agent": "DevOps-Script",
        "Accept": "application/json"
    },
    timeout=10
)

print(response.json())
```

Common uses:

* Authentication.
* Content type.
* Response format.
* Request identification.

Use the exact headers required by the API.

---

## 14. Logging API Failures

For DevOps automation, printing an error is useful, but logging is better for long-running scripts and troubleshooting.

```python
import logging
import requests

logging.basicConfig(level=logging.INFO)

try:
    response = requests.get(
        "https://httpbin.org/status/500",
        timeout=10
    )

    response.raise_for_status()
    logging.info("API request succeeded")

except requests.exceptions.RequestException as error:
    logging.error("API request failed: %s", error)
```

This example deliberately requests a 500 response.

### Why logging matters

Logs help engineers understand:

* Which operation failed.
* What error occurred.
* When a failure happened.
* Whether an automation task completed successfully.

Never log access tokens, passwords, or other secrets.

---

## 15. Important Best Practices

1. **Set timeouts:** Never rely on an API responding immediately.
2. **Check status codes:** Use `raise_for_status()` where appropriate.
3. **Handle exceptions:** Network requests can fail for many reasons.
4. **Protect secrets:** Use environment variables or a secret manager.
5. **Keep TLS verification enabled:** Do not disable certificate checks to bypass errors.
6. **Use sessions:** Reuse connections for repeated requests.
7. **Retry carefully:** Avoid duplicate operations and infinite retry loops.
8. **Follow pagination:** Do not assume one response contains every record.
9. **Use the correct payload:** Choose `json=` or `data=` according to the API.
10. **Read API documentation:** Authentication, response formats, rate limits, and supported methods vary by service.

---

## 16. Quick Revision Cheat Sheet

| Feature                | Python Requests                  |
| ---------------------- | -------------------------------- |
| GET request            | `requests.get(url)`              |
| POST request           | `requests.post(url, json=data)`  |
| PUT request            | `requests.put(url, json=data)`   |
| PATCH request          | `requests.patch(url, json=data)` |
| DELETE request         | `requests.delete(url)`           |
| HEAD request           | `requests.head(url)`             |
| OPTIONS request        | `requests.options(url)`          |
| Query parameters       | `params={...}`                   |
| Request headers        | `headers={...}`                  |
| JSON payload           | `json={...}`                     |
| Form/raw payload       | `data=...`                       |
| Timeout                | `timeout=10`                     |
| Check HTTP failure     | `response.raise_for_status()`    |
| Parse JSON response    | `response.json()`                |
| Response body as text  | `response.text`                  |
| Response status        | `response.status_code`           |
| Response headers       | `response.headers`               |
| Session                | `requests.Session()`             |
| Download large file    | `stream=True`                    |
| Verify TLS certificate | `verify=True` by default         |
| Custom CA certificate  | `verify="/path/to/ca.pem"`       |
| Install library        | `pip install requests`           |

---

## 17. Final Mental Model

```text
Python Script
     |
     v
Prepare Request
     |
     +-- URL
     +-- HTTP Method
     +-- Headers / Authentication
     +-- Query Parameters / Payload
     +-- Timeout
     |
     v
Send Request using requests
     |
     v
API Server
     |
     v
HTTP Response
     |
     +-- Status Code
     +-- Headers
     +-- Response Data
     |
     v
Handle Errors
     |
     +-- Retry if appropriate
     +-- Log failures
     +-- Process pagination
     +-- Verify response data
     |
     v
Continue Automation
```

**One-line interview answer:**

The Python `requests` module is an HTTP client library used to communicate with APIs. In DevOps and Cloud, it helps automate API calls, health checks, resource retrieval, deployment triggers, authentication, file downloads, and monitoring integrations.
