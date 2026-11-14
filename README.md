# Brave Search Goggles

Custom search filters and rules for the [Brave Search API](https://brave.com/search/api/), tailored for Abi Global Health services.

## Overview

This repository hosts goggle files for use with the Brave Search API. Goggles allow you to customize search result ranking and filtering programmatically by passing goggle URLs to API requests.

**Note:** Goggle files must be hosted at publicly accessible URLs for the Brave API to fetch and apply them.

## Goggles

| Goggle | Description |
|--------|-------------|
| `all_web.goggle` | Searches the entire web while filtering out results from generic community platforms (Quora, Reddit). Use for cleaner, more focused results when searching for reliable information. |
| `axa_mx.goggle` | Restricts search results to AXA MX blog pages. |
| `cq_curated_global.goggle` | Curated results optimized for community quality and global relevance. Filters results based on source quality and relevance metrics. |
| `hk_medical.goggle` | Restricts search results to validated medical and public health domains in Hong Kong. |
| `ilh.goggle` | Restricts search results to ILH public pages. |
| `nafdac_medical.goggle` | Restricts search results to validated medical and public health domains, including Nigerian health resources. |

## What are Goggles?

[Brave Search Goggles](https://safe.search.brave.com/help/goggles) are custom filters and ranking rules that let you:

- **Discard** results from specific sites or domains
- **Boost** results from trusted sources
- **Downrank** less relevant results
- **Create** personalized search experiences programmatically

Goggles are:
- **Declarative** — defined with simple text rules
- **Composable** — combine multiple goggles
- **Transparent** — rules are auditable and version-controlled

## Goggle File Format

Goggle files use a simple declarative format:

```
! Comments start with exclamation marks
! name: Display name
! description: What this goggle does
! public: true or false
! author: Your name

! Rules
$discard,site=example.com          # Discard all results from example.com
$boost,site=trusted.com            # Boost results from trusted.com
$downrank,keywords=low-quality      # Downrank results with certain keywords
```

For complete syntax documentation, see the [Goggles Quickstart Guide](https://github.com/brave/goggles-quickstart).

## Using with Brave Search API

### 1. **Host the goggle file publicly**

This repository can be hosted publicly, or goggle files can be deployed to:
- Public GitHub repository (raw.githubusercontent.com URLs)
- Public S3 bucket or CDN
- Public web server

### 2. **Pass the goggle URL to the Brave API**

In your service, include the `goggles_id` parameter when calling the Brave Search API:

```
GET https://api.search.brave.com/res/v1/web/search?q=query&goggles_id=https://raw.githubusercontent.com/abiglobalhealth/brave-api-goggles/main/goggles/all_web.goggle
```

Or with the LLM Context endpoint:

```
GET https://api.search.brave.com/res/v1/llms?q=query&goggles_id=<goggle-url>
```

### 3. **Use in your code**

Example with Python:

```python
import requests

GOGGLE_URL = "https://raw.githubusercontent.com/abiglobalhealth/brave-api-goggles/main/goggles/all_web.goggle"
API_KEY = "your-brave-api-key"

response = requests.get(
    "https://api.search.brave.com/res/v1/web/search",
    params={
        "q": "your search query",
        "goggles_id": GOGGLE_URL,
    },
    headers={"Accept": "application/json"},
    auth=("", API_KEY)
)

results = response.json()
```

## Repository Structure

```
brave-api-goggles/
├── README.md                    # This file
├── goggles/
│   ├── all_web.goggle          # Filter out community platforms
│   └── cq_curated_global.goggle # Quality-focused curation
```

## Contributing

To add or update goggles:

1. Create a new `.goggle` file in the `goggles/` directory
2. Follow the goggle file format with proper metadata headers
3. Test your rules with the Brave Search API
4. Commit with a clear description of the filter's purpose
5. Deploy the public URL

### Goggle Guidelines

- Use descriptive, clear names and descriptions
- Document your reasoning for each rule
- Test thoroughly before deployment
- Keep goggles focused on a single use case
- Minimize performance impact (avoid overly broad rules)

## Deployment

1. Merge changes to `main`
2. Goggle files are automatically available at:
   - `https://raw.githubusercontent.com/abiglobalhealth/brave-api-goggles/main/goggles/<filename>.goggle`
3. Update service configurations to point to new goggle URLs if needed

## License

These goggles are maintained by [Abi Global Health](https://www.abiglobalhealth.org/).

## Resources

- [Brave Search API Documentation](https://brave.com/search/api/)
- [Goggles - Brave Search API](https://api.search.brave.com/)
- [Goggles Quickstart Guide](https://github.com/brave/goggles-quickstart)
- [Brave Search Goggles Overview](https://safe.search.brave.com/help/goggles)
- [Brave Support - Using Goggles](https://support.brave.app/hc/en-us/articles/6959189556237-How-do-I-use-Goggles)
