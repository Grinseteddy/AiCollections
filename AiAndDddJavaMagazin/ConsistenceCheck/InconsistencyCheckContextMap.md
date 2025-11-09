# Context Map vs AsyncAPI Consistency Checker

You are an expert in Domain-Driven Design and API specification analysis. Your task is to identify inconsistencies between a Context Map and an AsyncAPI specification for a specific bounded context/service: [Bounded Context Name].

## Input Materials

I will provide:
1. **Context Map**: A visual or textual representation showing bounded contexts and their relationships
2. **Service/Bounded Context Name**: The specific context we're analyzing
3. **AsyncAPI Specification**: The async API spec for that service

## Analysis Framework

Perform a systematic consistency check across these dimensions:

### 1. Service/Bounded Context Identity
- **Context Map**: What is the bounded context called?
- **AsyncAPI**: What is the service identified as in `info.title` and throughout the spec?
- **Check**: Do the names match or clearly refer to the same concept?
- **Inconsistencies**: Name mismatches, unclear service identity

### 2. Relationship Patterns vs Message Flows
For each relationship shown in the Context Map involving this service:

#### 2.1 Upstream Relationships (Context consumes from others)
- **Context Map**: Which contexts does this context consume from? What relationship pattern (Customer-Supplier, Conformist, Anticorruption Layer, etc.)?
- **AsyncAPI**: Which channels/topics does the service subscribe to? (`channels.*.subscribe`)
- **Check**:
    - Are all upstream relationships represented by subscriptions?
    - Are there subscriptions without corresponding upstream relationships?
    - Do the subscription patterns match the relationship types (e.g., ACL suggests message transformation)?

#### 2.2 Downstream Relationships (Context publishes to others)
- **Context Map**: Which contexts consume from this context? What relationship pattern (Published Language, Open Host Service, etc.)?
- **AsyncAPI**: Which channels/topics does the service publish to? (`channels.*.publish`)
- **Check**:
    - Are all downstream relationships represented by publications?
    - Are there publications without corresponding downstream relationships?
    - If marked as Published Language or OHS, is there clear, stable schema documentation?

### 3. Event/Message Names and Semantics
- **Context Map**: What domain events or information is exchanged (may be shown in labels or descriptions)?
- **AsyncAPI**: What are the actual message names and operation IDs?
- **Check**:
    - Do message names reflect ubiquitous language from the domain?
    - Are event names consistent with domain event naming in the context?
    - Do message semantics match expected domain flows?

### 4. Channel/Topic Names vs Context Names
- **AsyncAPI**: Examine channel/topic naming patterns
- **Context Map**: Compare with bounded context names in relationships
- **Check**:
    - Do channel names reference the correct bounded contexts?
    - Is there a clear naming convention that reflects context ownership?
    - Are there orphaned channels not tied to any context relationship?

### 5. Shared Kernel and Published Language
If the Context Map shows Shared Kernel or Published Language patterns:
- **Check**: Are these reflected in schema sharing or stability?
- **Published Language**: Should have well-documented, versioned schemas
- **Shared Kernel**: May show identical schema definitions
- **Inconsistencies**: Unstable schemas in Published Language, divergent schemas in Shared Kernel

### 6. Conformist and Anticorruption Layer Patterns
- **Conformist**: Service should use upstream schemas directly without transformation
- **ACL**: Should have distinct internal models, transformation logic may be implied
- **Check**: Does the spec suggest the right level of coupling/decoupling?

### 7. Partnership Patterns
If Partnership is shown:
- **Check**: Is there bidirectional communication? Coordinated schema evolution?
- **Inconsistencies**: One-way communication only, no coordination evident

### 8. Context Boundaries and Message Payloads
- **Context Map**: Implied data ownership and boundaries
- **AsyncAPI**: Schema definitions in messages
- **Check**:
    - Do published messages contain data owned by this context?
    - Do subscribed messages respect context boundaries (not exposing internals unnecessarily)?
    - Are there data ownership violations (context publishing data from another context's domain)?

### 9. Missing Elements
- **Context Map shows but AsyncAPI doesn't**: Relationships that should have corresponding pub/sub but are missing
- **AsyncAPI shows but Context Map doesn't**: Messaging patterns not documented in strategic design

### 10. Versioning and Evolution
- **Context Map**: Partnership, Published Language patterns suggest versioning needs
- **AsyncAPI**: Check version information, schema evolution strategy
- **Check**: Is versioning appropriate for the relationship patterns shown?

## Output Format

Provide your analysis in this structure:

### ✅ Confirmed Consistencies
List what aligns correctly between the Context Map and AsyncAPI

### ⚠️ Inconsistencies Found

For each inconsistency:
```
**Category**: [e.g., Missing Relationship, Naming Mismatch, Pattern Violation]
**Severity**: [Critical/High/Medium/Low]
**Context Map Shows**: [what the strategic design indicates]
**AsyncAPI Shows**: [what the technical spec shows]
**Impact**: [why this matters]
**Recommendation**: [how to resolve]
```

### ❓ Ambiguities or Unclear Aspects
Areas where either artifact is unclear or incomplete

### 📋 Summary Statistics
- Total relationships in Context Map involving this service: X
- Total channels in AsyncAPI: Y
- Matched relationships: Z
- Unmatched relationships: A
- Undocumented channels: B

## Special Considerations

- **Context Map variations**: Handle both formal DDD context maps and informal architectural diagrams
- **AsyncAPI versions**: Be aware that AsyncAPI 2.x and 3.x have structural differences
- **Language considerations**: Be sensitive to multilingual contexts (German/English domain terminology)
- **Implicit vs Explicit**: Some relationships might be implicit in the Context Map but explicit in AsyncAPI

## Example Usage

```
Please analyze the consistency between:

1. Context Map: [attach or paste image/description]
2. Service: "Catalog Management"
3. AsyncAPI spec: [attach or paste YAML]
```

---

**Note**: This analysis helps ensure that strategic DDD design decisions are properly reflected in technical implementations, and vice versa. Inconsistencies often reveal either missing documentation or implementation gaps that need attention.