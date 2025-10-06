__../AiCollections/AiComparison/Gemini/Gemini25ProCatalogManagement.oas.yaml__

15:9  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.   security[0].openIdConnect[0]

16:9  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  security[0].openIdConnect[1]

__*Note*: for OpenIDConnect no security scopes are given in an OpenAPI because they need to be read from the IDM service during runtime__