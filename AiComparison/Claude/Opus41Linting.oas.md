__/Users/annegretjunkercodecentricag/IdeaProjects/AiCollections/AiComparison/Claude/Opus41CatalogManagement.yaml__

18:9   warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.      security[0].openIdConnect[0]

19:9   warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     security[0].openIdConnect[1]

40:15  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.      paths./catalog-entries.get.security[0].openIdConnect[0]

75:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries.post.security[0].openIdConnect[0]

111:15  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.      paths./catalog-entries/{catalogEntryId}.get.security[0].openIdConnect[0]

143:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}.put.security[0].openIdConnect[0]

181:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}.delete.security[0].openIdConnect[0]

203:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}/abstracts.put.security[0].openIdConnect[0]

245:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.     paths./catalog-entries/{catalogEntryId}/tags.put.security[0].openIdConnect[0]

462:10  warning  oas3-unused-component            Potentially unused component has been detected.  components.schemas.Book

__*Note*: for OpenIDConnect no security scopes are given in an OpenAPI because they need to be read from the IDM service during runtime__