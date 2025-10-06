__../AiCollections/AiComparison/Copilot/CopilotCatalogManagement.aas.yaml__

2:6   warning  asyncapi-3-tags                    AsyncAPI document must have non-empty "tags" array.                 info

19:6     error  asyncapi-3-document-resolved       Property "tags" is not expected to be here                          tags

24:24    error  asyncapi-3-document-resolved       "event-broker-library" property must have required property "host"  servers.event-broker-library

25:10    error  asyncapi-3-document-resolved       Property "url" is not expected to be here                           servers.event-broker-library.url

34:9     error  asyncapi-3-document-resolved       "0" property must have required property "type"                     servers.event-broker-library.security[0]

34:9     error  asyncapi-3-document-resolved       "0" property must not be valid                                      servers.event-broker-library.security[0]

34:23    error  asyncapi-3-document-resolved       Property "user-password" is not expected to be here                 servers.event-broker-library.security[0].user-password

39:13    error  asyncapi-3-document-resolved       Property "publish" is not expected to be here                       channels.catalog.entry.created.publish

58:13    error  asyncapi-3-document-resolved       Property "publish" is not expected to be here                       channels.catalog.entry.changed.publish

287:18  warning  asyncapi-unused-components-schema  Potentially unused components schema has been detected.             components.schemas.CatalogShort

303:14    error  asyncapi-3-document-resolved       Property "schema" is not expected to be here                        components.parameters.catalogEntryId.schema

307:20    error  asyncapi-3-document-resolved       Property "schemas_examples" is not expected to be here              components.schemas_examples