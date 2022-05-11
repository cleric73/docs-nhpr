## The National Health Products Registry (NHPR)
### API - Reference

The NHPR API is organized around REST. Our API has predictable resource-oriented URLs, accepts raw JSON request bodies, uses route parameters instead of query parameters for queries, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

### Contents

1. [Authorization](/authorization.md)
2. Errors
3. Product identifications
4. Core resources
  - Generic product
  - Trade item
  - Substance
  - Medication

### Authorization


### Errors

NHPR uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the `2xx` range indicate success. Codes in the `5xx` range indicate an error with NHPR's servers. All requests successfully processed by the server that do not case a server crash (error `5xx`) will return return with error codes in the `2xx` range with a `data` field in the body. 
Requests that result into logical errors but fail to return expected results will still return with a `2xx` code but will have a `false` success state with a `message` field decribing the error.
```markdown
{
    "success": false,
    "data": "One or more missing mandatory fields."
}
```
