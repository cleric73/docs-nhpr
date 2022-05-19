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
----
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

----
#### List all generic products
Returns a list of generic products.

```markdown
GET /nhpr/api/v1/medicines/genericproducts/all
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 1701,
        "products": [
            {
                "medicineGenericProductKey": "e985e1b5-74a3-4586-b13e-bb60e85da6c2",
                "medicineIndexKey": null,
                "nationalProductCode": null,
                "dataLifePhase": "0",
                "genericName": "WATER FOR INJECTION",
                "productForm": "AMPOULE",
                "productDescription": "WATER FOR INJECTION N/A AMPOULE",
                "strength": "N/A",
                "activeIngredient": "WATER FOR INJECTION",
                "dosageForm": "PARENTERAL ORDINARY AMPOULES     ",
                "routeOfAdministration": "PARENTERAL",
                "venClassification": "Essential",
                "unspscCode": null,
                "gpcCode": null,
                "atcCode": null,
                "productActivation": true,
                "productActivationDate": "1997-07-01T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-01-19T00:00:00.000Z",
                "updatedAt": "2022-01-19T00:00:00.000Z"
            },
            .
            .
            .
        ]
    }
}
```


----
#### List all generic products under drafting
Returns a list of generic products whose `currentApprovalSttus` is `drafting`. A product is considered to be under drafting if it has not yet been submitted to the approver for review and approval. 

```markdown
GET /nhpr/api/v1/medicines/genericproducts/drafting
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 7,
        "products": [
            {
                "medicineGenericProductKey": "e985e1b5-74a3-4586-b13e-bb60e85da6c2",
                "medicineIndexKey": null,
                "nationalProductCode": null,
                "dataLifePhase": "0",
                "genericName": "WATER FOR INJECTION",
                "productForm": "AMPOULE",
                "productDescription": "WATER FOR INJECTION N/A AMPOULE",
                "strength": "N/A",
                "activeIngredient": "WATER FOR INJECTION",
                "dosageForm": "PARENTERAL ORDINARY AMPOULES     ",
                "routeOfAdministration": "PARENTERAL",
                "venClassification": "Essential",
                "unspscCode": null,
                "gpcCode": null,
                "atcCode": null,
                "productActivation": true,
                "productActivationDate": "1997-07-01T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-01-19T00:00:00.000Z",
                "updatedAt": "2022-01-19T00:00:00.000Z"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all approved generic products
Returns a list of generic products whose `currentApprovalSttus` is `approved`. A product's data is considered to be approved if it has been submitted to the approver and is approved.

The approval status is an acknowledgement of the accuracy of the product data by an appropriate user (the approver)

```markdown
GET /nhpr/api/v1/medicines/genericproducts/approved
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 7,
        "products": [
            {
                "medicineGenericProductKey": "e985e1b5-74a3-4586-b13e-bb60e85da6c2",
                "medicineIndexKey": null,
                "nationalProductCode": null,
                "dataLifePhase": "0",
                "genericName": "WATER FOR INJECTION",
                "productForm": "AMPOULE",
                "productDescription": "WATER FOR INJECTION N/A AMPOULE",
                "strength": "N/A",
                "activeIngredient": "WATER FOR INJECTION",
                "dosageForm": "PARENTERAL ORDINARY AMPOULES     ",
                "routeOfAdministration": "PARENTERAL",
                "venClassification": "Essential",
                "unspscCode": null,
                "gpcCode": null,
                "atcCode": null,
                "productActivation": true,
                "productActivationDate": "1997-07-01T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-01-19T00:00:00.000Z",
                "updatedAt": "2022-01-19T00:00:00.000Z"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all active generic products
Returns a list of generic products whose `productActivation` is `active`. A product is considered to be active if it has been submitted to the approver and is approved and activated.

The active status is an indication that the product is in active use within the country.

```markdown
GET /nhpr/api/v1/medicines/genericproducts/active
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 7,
        "products": [
            {
                "medicineGenericProductKey": "e985e1b5-74a3-4586-b13e-bb60e85da6c2",
                "medicineIndexKey": null,
                "nationalProductCode": null,
                "dataLifePhase": "0",
                "genericName": "WATER FOR INJECTION",
                "productForm": "AMPOULE",
                "productDescription": "WATER FOR INJECTION N/A AMPOULE",
                "strength": "N/A",
                "activeIngredient": "WATER FOR INJECTION",
                "dosageForm": "PARENTERAL ORDINARY AMPOULES     ",
                "routeOfAdministration": "PARENTERAL",
                "venClassification": "Essential",
                "unspscCode": null,
                "gpcCode": null,
                "atcCode": null,
                "productActivation": true,
                "productActivationDate": "1997-07-01T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-01-19T00:00:00.000Z",
                "updatedAt": "2022-01-19T00:00:00.000Z"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all deactivated generic products
Returns a list of generic products whose `productActivation` is `false`. A product is considered to be deactivated if it has not yet be activated by an approver or was previously active then deactivated by the approver..

The product is then considered to not be in active use in the country.

```markdown
GET /nhpr/api/v1/medicines/genericproducts/deactivated
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 7,
        "products": [
            {
                "medicineGenericProductKey": "e985e1b5-74a3-4586-b13e-bb60e85da6c2",
                "medicineIndexKey": null,
                "nationalProductCode": null,
                "dataLifePhase": "0",
                "genericName": "WATER FOR INJECTION",
                "productForm": "AMPOULE",
                "productDescription": "WATER FOR INJECTION N/A AMPOULE",
                "strength": "N/A",
                "activeIngredient": "WATER FOR INJECTION",
                "dosageForm": "PARENTERAL ORDINARY AMPOULES     ",
                "routeOfAdministration": "PARENTERAL",
                "venClassification": "Essential",
                "unspscCode": null,
                "gpcCode": null,
                "atcCode": null,
                "productActivation": false,
                "productActivationDate": "1997-07-01T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-01-19T00:00:00.000Z",
                "updatedAt": "2022-01-19T00:00:00.000Z"
            },
            .
            .
            .
        ]
    }
}
```

----
#### Update generic product
Update generic product data.
The complete generic product data with the product id (medicineGenericProductKey)

```markdown
PUT /nhpr/api/v1/medicines/genericproducts/update
```

Request body
```markdown
{
    "product": {
            "medicineGenericProductKey": "e985e1b5-74a3-4586-b13e-bb60e85da6c2",
            "medicineIndexKey": null,
            "nationalProductCode": null,
            "dataLifePhase": "0",
            "genericName": "WATER FOR INJECTION",
            "productForm": "AMPOULE",
            "productDescription": "WATER FOR INJECTION N/A AMPOULE",
            "strength": "N/A",
            "activeIngredient": "WATER FOR INJECTION",
            "dosageForm": "PARENTERAL ORDINARY AMPOULES     ",
            "routeOfAdministration": "PARENTERAL",
            "venClassification": "Essential",
            "unspscCode": null,
            "gpcCode": null,
            "atcCode": null,
            "currentApprovalStatus": "Submitted"
    }
}
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Approve generic product
Approve generic product data. Generic product key supplied as a route parameter

```markdown
PUT /nhpr/api/v1/medicines/genericproducts/approve/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Decline generic product
Decline approval request of generic product data. Generic product key supplied as a route parameter.

```markdown
PUT /nhpr/api/v1/medicines/genericproducts/decline/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Activate generic product
Activate a generic product data. Generic product key supplied as a route parameter.

```markdown
PUT /nhpr/api/v1/medicines/genericproducts/activate/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Deactivate generic product
Deactivate a generic product data. Generic product key supplied as a route parameter.

```markdown
PUT /nhpr/api/v1/medicines/genericproducts/deactivate/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```
---------
### Trade Items

The medication resource represents an actual medication that can be given to a patient and is traded on the market.

Trade items are products that contain a given generic product. Hence may contain a reference to a generic product.

#### The trade item object

All results of the requests conform to the minimum Data Specifications for the Health Products Registry.

```markdown
{
    "medicineTradeItemKey": "0002da35-7137-4e19-b86b-312d3c85959c",
    "dataLifePhase": null,
    "medicineIndexKey": null,
    "matchedToGenericProduct": true,
    "ndaRegNumber": "NDA/MAL/HDP/6992",
    "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
    "ndaExpirationDate": null,
    "tradeItemDescription": null,
    "packaging": "1.0X15.0 ML BOTTLE",
    "content": null,
    "packMeasure": null,
    "packCount": null,
    "unitMeasure": null,
    "unitCount": null,
    "unitSize": null,
    "brandName": "AZIRIV-200",
    "brandOwner": "ROYAL PHARMA 2011 LIMITED",
    "brandOwnerLocation": "UGANDA",
    "brandOwnerGLN": null,
    "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
    "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
    "manufacturerLocation": "INDIA",
    "manufacturerGLN": null,
    "manufacturerPartNUM": null,
    "countryOfOrigin": "INDIA",
    "shelfLifeDays": null,
    "storageInstructions": null,
    "maxTemperature": null,
    "maxTemperatureUOM": null,
    "minTemperature": null,
    "minTemperatureUOM": null,
    "productActivation": true,
    "productActivationDate": "2019-05-29T00:00:00.000Z",
    "productPublication": true,
    "productPublicationDate": "2019-05-29T00:00:00.000Z",
    "currentApprovalStatus": "Approved",
    "nextApprovalStatus": "Drafting",
    "createdAt": "2022-04-23T00:00:00.000Z",
    "updatedAt": "2022-04-23T00:00:00.000Z",
    "MedicineGenericProductMedicineGenericProductKey": "a255b346-0502-426e-ad3f-3553a729bb3a"
}
```

#### Endpoints
```markdown
GET /nhpr/api/v1/medicines/tradeitems/all
GET /nhpr/api/v1/medicines/tradeitems/drafts
GET /nhpr/api/v1/medicines/tradeitems/approved
GET /nhpr/api/v1/medicines/tradeitems/active
GET /nhpr/api/v1/medicines/tradeitems/deactivated
GET /nhpr/api/v1/medicines/tradeitems/id/:id
PUT /nhpr/api/v1/medicines/tradeitems/update
PUT /nhpr/api/v1/medicines/tradeitems/approve/:id
PUT /nhpr/api/v1/medicines/tradeitems/decline/:id
PUT /nhpr/api/v1/medicines/tradeitems/deactivate/:id
PUT /nhpr/api/v1/medicines/tradeitems/activate/:id
PUT /nhpr/api/v1/medicines/tradeitems/publish/:id
PUT /nhpr/api/v1/medicines/tradeitems/unpublish/:id
PUT /nhpr/api/v1/medicines/tradeitems/match/:tradeitemid/:genericproductid
POST /nhpr/api/v1/medicines/tradeitems/create
```
----
#### Create a trade item
Creates a new trade item object

```markdown
POST /nhpr/api/v1/medicines/tradeitems/create
```

###### Mandatory fields
```markdown
brandName
brandOwner
brandOwnerLocation
manufacturerName
manufacturerLocation
countryOfOrigin
```

Request body
```markdown
{
    "product": {
        "ndaRegNumber": "NDA/MAL/HDP/699KLM3",
        "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
        "ndaExpirationDate": null,
        "tradeItemDescription": null,
        "packaging": "1.0X15.0 ML BOTTLE",
        "content": null,
        "packMeasure": null,
        "packCount": null,
        "unitMeasure": null,
        "unitCount": null,
        "unitSize": null,
        "brandName": "AZIRIV-200",
        "brandOwner": "ROYAL PHARMA 2011 LIMITED",
        "brandOwnerLocation": "UGANDA",
        "brandOwnerGLN": null,
        "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
        "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
        "manufacturerLocation": "INDIA",
        "manufacturerGLN": null,
        "manufacturerPartNUM": null,
        "countryOfOrigin": "INDIA",
        "shelfLifeDays": null,
        "storageInstructions": null,
        "maxTemperature": null,
        "maxTemperatureUOM": null,
        "minTemperature": null,
        "minTemperatureUOM": null
    }
}
```
Response body
```markdown
{
    "success": true,
    "data": {
        "product": {
            "medicineTradeItemKey": "15d97f62-b545-41eb-bfab-a27372d2ce9f",
            "dataLifePhase": "draft",
            "matchedToGenericProduct": false,
            "productActivation": false,
            "productPublication": false,
            "currentApprovalStatus": "Drafting",
            "nextApprovalStatus": "",
            "ndaRegNumber": "NDA/MAL/HDP/699KLM3",
            "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
            "ndaExpirationDate": null,
            "tradeItemDescription": null,
            "packaging": "1.0X15.0 ML BOTTLE",
            "content": null,
            "packMeasure": null,
            "packCount": null,
            "unitMeasure": null,
            "unitCount": null,
            "unitSize": null,
            "brandName": "AZIRIV-200",
            "brandOwner": "ROYAL PHARMA 2011 LIMITED",
            "brandOwnerLocation": "UGANDA",
            "brandOwnerGLN": null,
            "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
            "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
            "manufacturerLocation": "INDIA",
            "manufacturerGLN": null,
            "manufacturerPartNUM": null,
            "countryOfOrigin": "INDIA",
            "shelfLifeDays": null,
            "storageInstructions": null,
            "maxTemperature": null,
            "maxTemperatureUOM": null,
            "minTemperature": null,
            "minTemperatureUOM": null,
            "updatedAt": "2022-05-19T10:26:10.258Z",
            "createdAt": "2022-05-19T10:26:10.258Z",
            "medicineIndexKey": null,
            "productActivationDate": null,
            "productPublicationDate": null,
            "MedicineGenericProductMedicineGenericProductKey": null
        }
    }
}
```

----
#### Retrieve trade item
Retrieves trade item object based on product key supplied as route parameter

```markdown
GET /nhpr/api/v1/medicines/tradeitems/id/:id
```

Response body
```markdown
{
    "success": true,
    "data": {
        "product": {
            "medicineTradeItemKey": "349c7e76-7e20-4ada-8aad-d511631c3284",
            "dataLifePhase": "draft",
            "medicineIndexKey": null,
            "matchedToGenericProduct": true,
            "ndaRegNumber": "NDA/MAL/HDP/699KLM77",
            "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
            "ndaExpirationDate": null,
            "tradeItemDescription": null,
            "packaging": "1.0X15.0 ML BOTTLE",
            "content": null,
            "packMeasure": null,
            "packCount": null,
            "unitMeasure": null,
            "unitCount": null,
            "unitSize": null,
            "brandName": "AZIRIV-200",
            "brandOwner": "ROYAL PHARMA 2011 LIMITED",
            "brandOwnerLocation": "UGANDA",
            "brandOwnerGLN": null,
            "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
            "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
            "manufacturerLocation": "INDIA",
            "manufacturerGLN": null,
            "manufacturerPartNUM": null,
            "countryOfOrigin": "INDIA",
            "shelfLifeDays": null,
            "storageInstructions": null,
            "maxTemperature": null,
            "maxTemperatureUOM": null,
            "minTemperature": null,
            "minTemperatureUOM": null,
            "productActivation": false,
            "productActivationDate": null,
            "productPublication": false,
            "productPublicationDate": null,
            "currentApprovalStatus": "Approved",
            "nextApprovalStatus": "Drafting",
            "createdAt": "2022-04-27T10:05:51.619Z",
            "updatedAt": "2022-05-04T06:28:50.308Z",
            "MedicineGenericProductMedicineGenericProductKey": "7b20c321-cdf9-4576-857d-b574798559b7",
            "TradeItemPackagings": [],
            "ChangeLogs": []
        }
    }
}
```

----
#### List all trade items
Returns a list of trade items.

```markdown
GET /nhpr/api/v1/medicines/tradeitems/all
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 3763,
        "products": [
            {
                "medicineTradeItemKey": "0002da35-7137-4e19-b86b-312d3c85959c",
                "dataLifePhase": null,
                "medicineIndexKey": null,
                "matchedToGenericProduct": true,
                "ndaRegNumber": "NDA/MAL/HDP/6992",
                "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
                "ndaExpirationDate": null,
                "tradeItemDescription": null,
                "packaging": "1.0X15.0 ML BOTTLE",
                "content": null,
                "packMeasure": null,
                "packCount": null,
                "unitMeasure": null,
                "unitCount": null,
                "unitSize": null,
                "brandName": "AZIRIV-200",
                "brandOwner": "ROYAL PHARMA 2011 LIMITED",
                "brandOwnerLocation": "UGANDA",
                "brandOwnerGLN": null,
                "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
                "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
                "manufacturerLocation": "INDIA",
                "manufacturerGLN": null,
                "manufacturerPartNUM": null,
                "countryOfOrigin": "INDIA",
                "shelfLifeDays": null,
                "storageInstructions": null,
                "maxTemperature": null,
                "maxTemperatureUOM": null,
                "minTemperature": null,
                "minTemperatureUOM": null,
                "productActivation": true,
                "productActivationDate": "2019-05-29T00:00:00.000Z",
                "productPublication": true,
                "productPublicationDate": "2019-05-29T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-04-23T00:00:00.000Z",
                "updatedAt": "2022-04-23T00:00:00.000Z",
                "MedicineGenericProductMedicineGenericProductKey": "a255b346-0502-426e-ad3f-3553a729bb3a"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all trade items under drafting
Returns a list of trade items whose `currentApprovalStatus` is `drafting`. A product is considered to be under drafting if it has not yet been submitted to the approver for review and approval. 

```markdown
GET /nhpr/api/v1/medicines/tradeitems/drafting
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 7,
        "products": [
            {
                "medicineTradeItemKey": "0002da35-7137-4e19-b86b-312d3c85959c",
                "dataLifePhase": null,
                "medicineIndexKey": null,
                "matchedToGenericProduct": true,
                "ndaRegNumber": "NDA/MAL/HDP/6992",
                "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
                "ndaExpirationDate": null,
                "tradeItemDescription": null,
                "packaging": "1.0X15.0 ML BOTTLE",
                "content": null,
                "packMeasure": null,
                "packCount": null,
                "unitMeasure": null,
                "unitCount": null,
                "unitSize": null,
                "brandName": "AZIRIV-200",
                "brandOwner": "ROYAL PHARMA 2011 LIMITED",
                "brandOwnerLocation": "UGANDA",
                "brandOwnerGLN": null,
                "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
                "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
                "manufacturerLocation": "INDIA",
                "manufacturerGLN": null,
                "manufacturerPartNUM": null,
                "countryOfOrigin": "INDIA",
                "shelfLifeDays": null,
                "storageInstructions": null,
                "maxTemperature": null,
                "maxTemperatureUOM": null,
                "minTemperature": null,
                "minTemperatureUOM": null,
                "productActivation": true,
                "productActivationDate": "2019-05-29T00:00:00.000Z",
                "productPublication": true,
                "productPublicationDate": "2019-05-29T00:00:00.000Z",
                "currentApprovalStatus": "Drafting",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-04-23T00:00:00.000Z",
                "updatedAt": "2022-04-23T00:00:00.000Z",
                "MedicineGenericProductMedicineGenericProductKey": "a255b346-0502-426e-ad3f-3553a729bb3a"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all approved trade items
Returns a list of trade items whose `currentApprovalSttus` is `approved`. A product's data is considered to be approved if it has been submitted to the approver and is approved.

The approval status is an acknowledgement of the accuracy of the product data by an appropriate user (the approver)

```markdown
GET /nhpr/api/v1/medicines/tradeitems/approved
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 3760,
        "products": [
            {
                "medicineTradeItemKey": "0002da35-7137-4e19-b86b-312d3c85959c",
                "dataLifePhase": null,
                "medicineIndexKey": null,
                "matchedToGenericProduct": true,
                "ndaRegNumber": "NDA/MAL/HDP/6992",
                "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
                "ndaExpirationDate": null,
                "tradeItemDescription": null,
                "packaging": "1.0X15.0 ML BOTTLE",
                "content": null,
                "packMeasure": null,
                "packCount": null,
                "unitMeasure": null,
                "unitCount": null,
                "unitSize": null,
                "brandName": "AZIRIV-200",
                "brandOwner": "ROYAL PHARMA 2011 LIMITED",
                "brandOwnerLocation": "UGANDA",
                "brandOwnerGLN": null,
                "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
                "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
                "manufacturerLocation": "INDIA",
                "manufacturerGLN": null,
                "manufacturerPartNUM": null,
                "countryOfOrigin": "INDIA",
                "shelfLifeDays": null,
                "storageInstructions": null,
                "maxTemperature": null,
                "maxTemperatureUOM": null,
                "minTemperature": null,
                "minTemperatureUOM": null,
                "productActivation": true,
                "productActivationDate": "2019-05-29T00:00:00.000Z",
                "productPublication": true,
                "productPublicationDate": "2019-05-29T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-04-23T00:00:00.000Z",
                "updatedAt": "2022-04-23T00:00:00.000Z",
                "MedicineGenericProductMedicineGenericProductKey": "a255b346-0502-426e-ad3f-3553a729bb3a"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all active trade items
Returns a list of trade items whose `productActivation` is `true` _(for active)_ . A product is considered to be active if it has been submitted to the approver and is approved and activated.

The active status is an indication that the product is in active use and traded within the country.

```markdown
GET /nhpr/api/v1/medicines/tradeitems/active
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 3759,
        "products": [
            {
                "medicineTradeItemKey": "0002da35-7137-4e19-b86b-312d3c85959c",
                "dataLifePhase": null,
                "medicineIndexKey": null,
                "matchedToGenericProduct": true,
                "ndaRegNumber": "NDA/MAL/HDP/6992",
                "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
                "ndaExpirationDate": null,
                "tradeItemDescription": null,
                "packaging": "1.0X15.0 ML BOTTLE",
                "content": null,
                "packMeasure": null,
                "packCount": null,
                "unitMeasure": null,
                "unitCount": null,
                "unitSize": null,
                "brandName": "AZIRIV-200",
                "brandOwner": "ROYAL PHARMA 2011 LIMITED",
                "brandOwnerLocation": "UGANDA",
                "brandOwnerGLN": null,
                "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
                "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
                "manufacturerLocation": "INDIA",
                "manufacturerGLN": null,
                "manufacturerPartNUM": null,
                "countryOfOrigin": "INDIA",
                "shelfLifeDays": null,
                "storageInstructions": null,
                "maxTemperature": null,
                "maxTemperatureUOM": null,
                "minTemperature": null,
                "minTemperatureUOM": null,
                "productActivation": true,
                "productActivationDate": "2019-05-29T00:00:00.000Z",
                "productPublication": true,
                "productPublicationDate": "2019-05-29T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-04-23T00:00:00.000Z",
                "updatedAt": "2022-04-23T00:00:00.000Z",
                "MedicineGenericProductMedicineGenericProductKey": "a255b346-0502-426e-ad3f-3553a729bb3a"
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all deactivated trade items
Returns a list of trade items whose `productActivation` is `false` _(for deactivated)_. A product is considered to be deactivated if it has not yet be activated by an approver or was previously active then deactivated by the approver.

The product is then considered to not be in active use or not traded in the country.

```markdown
GET /nhpr/api/v1/medicines/tradeitems/deactivated
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 4,
        "products": [
            {
                "medicineTradeItemKey": "21b16ce4-86fe-4d8d-a1f9-7de68ae045f0",
                "dataLifePhase": "draft",
                "medicineIndexKey": null,
                "matchedToGenericProduct": false,
                "ndaRegNumber": "NDA/MAL/HDP/699KLM4",
                "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
                "ndaExpirationDate": null,
                "tradeItemDescription": null,
                "packaging": "1.0X15.0 ML BOTTLE",
                "content": null,
                "packMeasure": null,
                "packCount": null,
                "unitMeasure": null,
                "unitCount": null,
                "unitSize": null,
                "brandName": "AZIRIV-200",
                "brandOwner": "ROYAL PHARMA 2011 LIMITED",
                "brandOwnerLocation": "UGANDA",
                "brandOwnerGLN": null,
                "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
                "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
                "manufacturerLocation": "INDIA",
                "manufacturerGLN": null,
                "manufacturerPartNUM": null,
                "countryOfOrigin": "INDIA",
                "shelfLifeDays": null,
                "storageInstructions": null,
                "maxTemperature": null,
                "maxTemperatureUOM": null,
                "minTemperature": null,
                "minTemperatureUOM": null,
                "productActivation": false,
                "productActivationDate": null,
                "productPublication": false,
                "productPublicationDate": null,
                "currentApprovalStatus": "Declined",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-04-30T06:10:09.713Z",
                "updatedAt": "2022-04-30T06:19:01.510Z",
                "MedicineGenericProductMedicineGenericProductKey": null
            },
            .
            .
            .
        ]
    }
}
```

----
#### List all matched trade items
Returns a list of trade items whose that are currently associated to a generic product.

Matched trade items have the field `matchedToGenericProduct` _(Boolean)_ set to `true` and have a valid `uuidv4` string in the field `MedicineGenericProductMedicineGenericProductKey` which is the `MedicineGenericProductKey` of the generic product.

```markdown
GET /nhpr/api/v1/medicines/tradeitems/matched
```

Response body
```markdown
{
    "success": true,
    "data": {
        "count": 3761,
        "products": [
            {
                "medicineTradeItemKey": "0002da35-7137-4e19-b86b-312d3c85959c",
                "dataLifePhase": null,
                "medicineIndexKey": null,
                "matchedToGenericProduct": true,
                "ndaRegNumber": "NDA/MAL/HDP/6992",
                "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
                "ndaExpirationDate": null,
                "tradeItemDescription": null,
                "packaging": "1.0X15.0 ML BOTTLE",
                "content": null,
                "packMeasure": null,
                "packCount": null,
                "unitMeasure": null,
                "unitCount": null,
                "unitSize": null,
                "brandName": "AZIRIV-200",
                "brandOwner": "ROYAL PHARMA 2011 LIMITED",
                "brandOwnerLocation": "UGANDA",
                "brandOwnerGLN": null,
                "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
                "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
                "manufacturerLocation": "INDIA",
                "manufacturerGLN": null,
                "manufacturerPartNUM": null,
                "countryOfOrigin": "INDIA",
                "shelfLifeDays": null,
                "storageInstructions": null,
                "maxTemperature": null,
                "maxTemperatureUOM": null,
                "minTemperature": null,
                "minTemperatureUOM": null,
                "productActivation": true,
                "productActivationDate": "2019-05-29T00:00:00.000Z",
                "productPublication": true,
                "productPublicationDate": "2019-05-29T00:00:00.000Z",
                "currentApprovalStatus": "Approved",
                "nextApprovalStatus": "Drafting",
                "createdAt": "2022-04-23T00:00:00.000Z",
                "updatedAt": "2022-04-23T00:00:00.000Z",
                "MedicineGenericProductMedicineGenericProductKey": "a255b346-0502-426e-ad3f-3553a729bb3a"
            },
            .
            .
            .
        ]
    }
}
```

----
#### Update trade item
Update trade itme data.
The complete trade itme data with the product id (medicineTradeItemKey)

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/update
```

Request body
```markdown
{
    "product": {
        "medicineTradeItemKey": "349c7e76-7e20-4ada-8aad-d511631c3284",
        "dataLifePhase": "draft",
        "matchedToGenericProduct": false,
        "productActivation": false,
        "productPublication": false,
        "currentApprovalStatus": "Drafting",
        "nextApprovalStatus": "",
        "ndaRegNumber": "NDA/MAL/HDP/699KLM77",
        "ndaRegistrationDate": "2019-05-29T00:00:00.000Z",
        "ndaExpirationDate": null,
        "tradeItemDescription": null,
        "packaging": "1.0X15.0 ML BOTTLE",
        "content": null,
        "packMeasure": null,
        "packCount": null,
        "unitMeasure": null,
        "unitCount": null,
        "unitSize": null,
        "brandName": "AZIRIV-200",
        "brandOwner": "ROYAL PHARMA 2011 LIMITED",
        "brandOwnerLocation": "UGANDA",
        "brandOwnerGLN": null,
        "manufacturerName": "EAST AFRICAN (INDIA) OVERSEAS-PLOT NO.1, PHARMACITY SELAQUI  DEHRADUN – 248011 (U.K), INDIA",
        "manufacturerShortName": "ROYAL PHARMA 2011 LIMITED",
        "manufacturerLocation": "INDIA",
        "manufacturerGLN": null,
        "manufacturerPartNUM": null,
        "countryOfOrigin": "INDIA",
        "shelfLifeDays": null,
        "storageInstructions": null,
        "maxTemperature": null,
        "maxTemperatureUOM": null,
        "minTemperature": null,
        "minTemperatureUOM": null,
        "updatedAt": "2022-04-27T10:05:51.619Z",
        "createdAt": "2022-04-27T10:05:51.619Z",
        "medicineIndexKey": null,
        "productActivationDate": null,
        "productPublicationDate": null,
        "MedicineGenericProductMedicineGenericProductKey": null
    }
}
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Approve trade items
Approve trade item data. trade item product key supplied as a route parameter

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/approve/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Decline trade item
Decline approval request of trade item data. Trade item product key supplied as a route parameter.

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/decline/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Activate trade item
Activate a trade item data. Trade item product key supplied as a route parameter.

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/activate/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Deactivate trade item
Deactivate a trade item data. Trade item key supplied as a route parameter.

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/deactivate/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Publish trade item
Deactivate a trade item data. Trade item key supplied as a route parameter.

Publishing a trade item make the trade item data available on the public portal.

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/publish/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Unublish trade item
Deactivate a trade item data. Trade item key supplied as a route parameter.

Unpublishing a trade item make the trade item data unavailable on the public portal.

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/unpublish/:id
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```

----
#### Match trade item
Match a trade item to generic product. Trade item and generic product keys are supplied as route parameters.

```markdown
PUT /nhpr/api/v1/medicines/tradeitems/match/:tradeitemid/:genericproductid
```

Response body
The `data` field returns the number of records updated wich is `1`
```markdown
{
    "success": true,
    "data": {
        "product": [
            1
        ]
    }
}
```
