# urlink

🔗 **Python SDK for Urlink URL Shortener** - Shorten URLs with ease!

[![PyPI version](https://badge.fury.io/py/urlink.svg)](https://badge.fury.io/py/urlink)
[![Python Support](https://img.shields.io/pypi/pyversions/urlink.svg)](https://pypi.org/project/urlink/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Installation

```bash
pip install urlink
```

## Get an API Key

All requests require an API key. Use the built-in developer portal:

1. Go to [url.ashin.me](https://url.ashin.me) to access the portal.
2. Create an account or log in.
3. Click "API Key" button. this will take you to the [developer page](https://url.ashin.me/developers).

## Quick Start

### Configure API Access

```python
import urlink

urlink.configure(api_key="YOUR-API-KEY")
```

### Simple API (Recommended for beginners)

```python
import urlink

# Configure once per process
urlink.configure(api_key="YOUR-API-KEY")

# Set your long URL
urlink.input = "https://example.com/very/long/url/that/needs/shortening"

# Get the shortened URL
print(urlink.output)
# Output: https://639.qzz.io/abc123
```

### With Custom Options

```python
import urlink

# Set the URL to shorten
urlink.input = "https://example.com/my-awesome-page"

# Optional: Set a custom domain
urlink.domain = "mysite.co"

# Optional: Set a custom ending/slug
urlink.ending = "awesome"

# Get the result
print(urlink.output)
# Output: https://mysite.co/awesome
```

### One-liner Method

```python
import urlink

# Configure if you haven't already
urlink.configure(api_key="YOUR-API-KEY")

# Shorten in one call
short_url = urlink.shorten("https://example.com/long-url")
print(short_url)

# With custom ending
short_url = urlink.shorten(
    "https://example.com/long-url",
    ending="my-custom-link"
)
print(short_url)
```

## Advanced Usage

### Using the Client Class

```python
from urlink import UrlinkClient

# Create a client instance
client = UrlinkClient(api_key="YOUR-API-KEY")

# Shorten a URL
result = client.shorten("https://example.com/long-url")
print(result['short'])  # https://639.qzz.io/abc123
print(result['code'])   # abc123

# With custom code
result = client.shorten(
    url="https://example.com/product",
    code="my-product"
)

# With custom domain
result = client.shorten(
    url="https://example.com/page",
    domain="mybrand.co",
    code="landing"
)
```

### Check Code Availability

```python
from urlink import UrlinkClient

client = UrlinkClient(api_key="YOUR-API-KEY")

# Check if a code is available
if client.check_availability("my-link"):
    print("Code is available!")
    result = client.shorten(
        "https://example.com/page",
        code="my-link"
    )
else:
    print("Code is taken, try another one")
```

### Bulk URL Shortening

```python
from urlink import UrlinkClient

client = UrlinkClient(api_key="YOUR-API-KEY")

urls = [
    "https://example.com/page1",
    "https://example.com/page2",
    "https://example.com/page3",
]

results = client.bulk_shorten(urls)

for result in results:
    if result['success']:
        print(f"{result['original']} -> {result['short']}")
    else:
        print(f"Failed: {result['original']} - {result['error']}")
```

### Configuration

```python
import urlink

# Configure with API key (required)
urlink.configure(
    api_key="your-api-key",
    base_url="https://639.qzz.io"  # optional, defaults to https://639.qzz.io
)

# Self-hosted example
urlink.configure(
    api_key="your-self-hosted-key",
    base_url="https://your-domain.com"
)
```

## Error Handling

```python
from urlink import UrlinkClient
from urlink.client import (
    UrlinkError,
    UrlinkAPIError,
    UrlinkValidationError,
    UrlinkConnectionError
)

client = UrlinkClient(api_key="YOUR-API-KEY")

try:
    result = client.shorten("https://example.com", code="taken-code")
except UrlinkValidationError as e:
    print(f"Invalid input: {e}")
except UrlinkAPIError as e:
    print(f"API error: {e}")
    print(f"Status code: {e.status_code}")
except UrlinkConnectionError as e:
    print(f"Connection failed: {e}")
except UrlinkError as e:
    print(f"General error: {e}")
```

## API Reference

### Module-Level API

| Property/Method | Description |
|----------------|-------------|
| `urlink.input` | Set the URL to shorten |
| `urlink.domain` | Set custom domain (optional) |
| `urlink.ending` | Set custom slug/code (optional) |
| `urlink.output` | Get the shortened URL |
| `urlink.shorten(url, domain, ending)` | Shortcut method |
| `urlink.check(code, domain)` | Check code availability |
| `urlink.reset()` | Reset all settings |
| `urlink.configure(api_key, base_url)` | Configure the client |

### UrlinkClient Methods

| Method | Description |
|--------|-------------|
| `shorten(url, domain, code, user_id)` | Shorten a URL |
| `check_availability(code, domain)` | Check if code is available |
| `bulk_shorten(urls, domain)` | Shorten multiple URLs |

### Response Format

```python
{
    "code": "abc123",           # The short code
    "original": "https://...",  # Original URL
    "short": "https://...",     # Full shortened URL
    "domain": "639.qzz.io",      # Domain used
    "timestamp": 1700000000000  # Creation timestamp
}
```

## Requirements

- Python 3.7+
- No external dependencies (uses only Python standard library)

## License

MIT License - see [LICENSE](LICENSE) for details.

## Links

- 📖 [Documentation](https://github.com/Ashin-Jiyo/urlink/blob/main/README.md)
- 🐛 [Issue Tracker](https://github.com/Ashin-Jiyo/urlink/issues)
- 📦 [PyPI Package](https://pypi.org/project/urlink/)
- 🌐 [Urlink Website](https://url.ashin.me)
