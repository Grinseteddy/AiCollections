__../AiCollections/AiComparison/Gemini/Gemini25FlashCatalogManagement.oas.yaml__

17:9   warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  security[0].openIdConnect[0]

32:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries.get.security[0].openIdConnect[0]

60:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries.post.security[0].openIdConnect[0]

93:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}.get.security[0].openIdConnect[0]

122:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}.put.security[0].openIdConnect[0]

153:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}.delete.security[0].openIdConnect[0]

177:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}/abstracts.put.security[0].openIdConnect[0]

209:15  warning  oas3-operation-security-defined  "catalog:manage" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}/tags.put.security[0].openIdConnect[0]

__*Note*: for OpenIDConnect no security scopes are given in an OpenAPI because they need to be read from the IDM service during runtime__