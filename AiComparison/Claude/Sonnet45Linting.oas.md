__../AiCollections/AiComparison/Claude/Sonnet45CatalogManagement.oas.yaml__

17:9   warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.   security[0].openIdConnect[0]

18:9   warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  security[0].openIdConnect[1]

37:15  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.   paths./catalog-entries.get.security[0].openIdConnect[0]

60:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  paths./catalog-entries.post.security[0].openIdConnect[0]

85:15  warning  oas3-operation-security-defined  "catalog:read" must be listed among scopes.   paths./catalog-entries/{catalogEntryId}.get.security[0].openIdConnect[0]

110:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}.put.security[0].openIdConnect[0]
137:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}.delete.security[0].openIdConnect[0]

163:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}/abstracts.put.security[0].openIdConnect[0]

191:15  warning  oas3-operation-security-defined  "catalog:write" must be listed among scopes.  paths./catalog-entries/{catalogEntryId}/tags.put.security[0].openIdConnect[0]

✖ 9 problems (0 errors, 9 warnings, 0 infos, 0 hints)

__*Note*: for OpenIDConnect no security scopes are given in an OpenAPI because they need to be read from the IDM service during runtime__
