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
    "message": "One or more missing mandatory fields."
}
```

#### Error handling

Errors can be programatically tracked by the `success` field of the response body.

### Product identifications

NHPR uses `uuidv4` for all generated product unique identifiers.

### Core Resources

#### Generic Products

Generic Products represents a raw/main material used to represent a packaged or measured amount of a known substance, an ingredient of an actively traded medication on the market.

#### The generic product object

All results of the requests conform to the minimum Data Specifications for the Health Products Registry.

```markdown
{
    "medicineGenericProductKey": "7b20c321-cdf9-4576-857d-b574798559b7",
    "medicineIndexKey": null,
    "nationalProductCode": null,
    "genericName": "MULTI-INGREDIENT",
    "productForm": "AEROSOLS",
    "productDescription": "MULTI-INGREDIENT MULTI-INGREDIENT AEROSOLS",
    "strength": "MULTI-INGREDIENT",
    "activeIngredient": "MULTI-INGREDIENT",
    "dosageForm": "TOPICAL EXTERNAL PRESSURISED AEROSOLS",
    "routeOfAdministration": "EXTERNAL",
    "venClassification": "Non-Essential",
    "unspscCode": null,
    "gpcCode": null,
    "atcCode": null,
    "productActivation": true,
    "productActivationDate": "1997-07-01T00:00:00.000Z",
    "currentApprovalStatus": "Drafting",
    "nextApprovalStatus": "Submitted",
    "createdAt": "2022-01-19T00:00:00.000Z",
    "updatedAt": "2022-05-06T05:59:14.423Z",
    "MedicineTradeItems": []
}
```

#### Endpoints
```markdown
GET /nhpr/api/v1/medicines/genericproducts/all
GET /nhpr/api/v1/medicines/genericproducts/drafts
GET /nhpr/api/v1/medicines/genericproducts/approved
GET /nhpr/api/v1/medicines/genericproducts/active
GET /nhpr/api/v1/medicines/genericproducts/deactivated
GET /nhpr/api/v1/medicines/genericproducts/id/:id
PUT /nhpr/api/v1/medicines/genericproducts/update
PUT /nhpr/api/v1/medicines/genericproducts/approve/:id
PUT /nhpr/api/v1/medicines/genericproducts/decline/:id
PUT /nhpr/api/v1/medicines/genericproducts/deactivate/:id
PUT /nhpr/api/v1/medicines/genericproducts/activate/:id
POST /nhpr/api/v1/medicines/genericproducts/create
```

#### Create a generic product
Creates a new generic product object

```markdown
POST /nhpr/api/v1/medicines/genericproducts/create
```

Request body
```markdown
{
    "product": {
        "genericName": "test generic name",
        "productForm": "test product form",
        "productDescription": "test product description",
        "strength": "test strength",
        "activeIngredient": "test active ingredient",
        "dosageForm": "test dosage form",
        "routeOfAdministration": "test route of administration",
        "venClassification": "Essential",
        "unspscCode": 12345,
        "gpcCode": 123456,
        "atcCode": 1234567
    }
}
```
Response body
```markdown
{
    "success": true,
    "data": {
        "product": {
            "medicineGenericProductKey": "16b5c6a6-9a4c-48ff-bf84-238dc6ddcd8a",
            "medicineIndexKey": "M_Avs4ABDJCaFHJgH6p3",
            "nationalProductCode": "",
            "dataLifePhase": "draft",
            "productActivation": false,
            "currentApprovalStatus": "Drafting",
            "nextApprovalStatus": "",
            "genericName": "test generic name",
            "productForm": "test product form",
            "productDescription": "test product description",
            "strength": "test strength",
            "activeIngredient": "test active ingredient",
            "dosageForm": "test dosage form",
            "routeOfAdministration": "test route of administration",
            "venClassification": "Essential",
            "unspscCode": "12345",
            "gpcCode": "123456",
            "atcCode": "1234567",
            "productActivationDate": null
        }
    }
}
```

----
#### Retrieve generic product
Retrieves generic product object based on generic product key supplied as route parameter

```markdown
GET /nhpr/api/v1/medicines/genericproducts/id/:id
```

Response body
```markdown
{
    "success": true,
    "data": {
        "product": {
            "medicineGenericProductKey": "7b20c321-cdf9-4576-857d-b574798559b7",
            "medicineIndexKey": null,
            "nationalProductCode": null,
            "dataLifePhase": "0",
            "genericName": "MULTI-INGREDIENT",
            "productForm": "AEROSOLS",
            "productDescription": "MULTI-INGREDIENT MULTI-INGREDIENT AEROSOLS",
            "strength": "MULTI-INGREDIENT",
            "activeIngredient": "MULTI-INGREDIENT",
            "dosageForm": "TOPICAL EXTERNAL PRESSURISED AEROSOLS    ",
            "routeOfAdministration": "EXTERNAL",
            "venClassification": "Non-Essential",
            "unspscCode": null,
            "gpcCode": null,
            "atcCode": null,
            "productActivation": true,
            "productActivationDate": "1997-07-01T00:00:00.000Z",
            "currentApprovalStatus": "Drafting",
            "nextApprovalStatus": "Submitted",
            "createdAt": "2022-01-19T00:00:00.000Z",
            "updatedAt": "2022-05-06T05:59:14.423Z",
            "MedicineTradeItems": []
        }
    }
}
```
