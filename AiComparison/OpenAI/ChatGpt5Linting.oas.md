__../AiCollections/AiComparison/OpenAI/ChatGpt5CatalogManagement.oas.yaml__

46:9   warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.      security[0].openIdConnect[0]

47:9   warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     security[0].openIdConnect[1]

64:27  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.      paths./catalog-entries.get.security[0].openIdConnect[0]

91:27  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries.post.security[0].openIdConnect[0]

115:27  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.      paths./catalog-entries/{catalogEntryId}.get.security[0].openIdConnect[0]

126:27  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}.put.security[0].openIdConnect[0]

145:27  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}.delete.security[0].openIdConnect[0]

163:27  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}/abstracts.put.security[0].openIdConnect[0]

185:27  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}/tags.put.security[0].openIdConnect[0]

428:10  warning  oas3-unused-component            Potentially unused component has been detected.  components.schemas.Book