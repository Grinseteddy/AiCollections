# OpenAPI Skill

## When to Use

When creating, editing, or validating OpenAPI specification files.

## Supported Input Modes

### Mode 1: API Product Canvas + Visual Glossary
Use when: user provides a canvas describing the API's purpose, consumers,
and endpoints, plus a glossary defining domain terms.

1. Carefully analyze the images to extract API endpoint information and data models
2. Use visual recognition to identify endpoints, HTTP methods, parameters, and responses
3. Extract entity relationships, property names, types, and required fields from the Visual Glossary

Extraction rules:
- From the Canvas → populate `info.title`, `info.description`, `tags`,
  top-level `paths` 
- From the Glossary → populate `components/schemas` (each term = a schema,
  properties = attributes listed for that term)

### Mode 2: Context Map + Visual Glossary

#### Reading the Context Map — Visual Conventions

**Bounded Contexts** are rounded rectangles with a title.
- Each context = one logical API grouping (use as `tags`), when not differently specified
- Context classification (Generic / Supportive / Core) from the legend
  → include in the tag `description` to indicate strategic importance

**HTTP Methods** appear as colored sticky notes on the LEFT or RIGHT EDGE of a context:
- Blue sticky = GET
- Green sticky = POST
- Yellow/Orange sticky = PUT
- Pink/Magenta sticky = DELETE
  → Each method visible on a context's edge = one operation to generate under that context's path

**Domain Events** are ORANGE sticky notes INSIDE a context (person/flag icon on top-right)
- Each orange sticky = one operation within the context
- The sticky label = the `operationId` and `summary` (convert to verb form:
  "Bicycle distributed" → `distributeBicycle`)
- The HTTP method is determined by the colored sticky on the context edge
  nearest to this event

**Data Objects / Entities** are YELLOW sticky notes optional with a 📄 document icon
- Each yellow sticky = one entry in `components/schemas`
- If the Visual Glossary is provided, use its definition for the schema properties
- If no glossary, infer properties from context and domain event names

**Read Models / Projections** are GREEN sticky notes optional with a 👁 eye icon
- Generate as GET-only schemas with `readOnly: true` on all properties
- Do not generate POST/PUT operations for these

**Integration Patterns** are labels on connecting lines between contexts:
- `ACL` (Anti-Corruption Layer): The consuming context translates the model.
  → Add a note in the operation `description` that the data is translated
  from an upstream context
- `ACL + Conformist`: Downstream adopts upstream model.
  → Reuse the upstream context's schema via `$ref` rather than duplicating it
- Dashed arrows show direction of dependency (arrow points to provider)

#### Extraction Steps (Mode 2)
1. List all bounded contexts → become `tags` when not differently specified
2. For each context, identify HTTP methods on its edge → determine which
   operations exist
3. Map each orange sticky (event) to an operation under the matching HTTP method
4. Map each yellow sticky (entity) to a `components/schemas` entry
5. Map each green sticky (read model) to a GET schema with `readOnly: true`
6. For ACL+Conformist relationships → use `$ref` to the upstream schema
   instead of a new definition
7. Enrich all schemas with Visual Glossary definitions if provided

## API Guidelines

Before generating any spec, read and apply the full API guidelines at:
`/mnt/skills/user/openapi/api-guidelines.md`

The guidelines cover naming conventions, versioning, security patterns, HTTP methods,
schema design, error handling, pagination, sorting, and metadata. Rules in the
guidelines take precedence over inferred conventions.

## General Rules (all modes)

- Always use OpenAPI 3.1.0 unless the user specifies otherwise
- Include 'info', 'servers', 'paths', and 'components' section
- Use 'requestBodies' section to define request bodies
- Use 'parameters' section to define parameters
- Define reusable schemas in 'components/schemas'
- Every schema property must include `type` and `description`
- Use glossary definitions when available verbatim as `description` values — preserve domain language
- Always include examples
- Always include description
- Always include summary for endpoints and methods
- If a term in the glossary has relationships to other terms, model these as `$ref`
- Prefer YAML output unless user requests JSON
- Use the YAML example as a template for formatting and organization
- Use `read` scopes for GET operations and `write` scopes for POST, PUT, PATCH, and DELETE operations
- Use camelCase for all `operationId` values (e.g. `getTaskById`, `createTask`)
- Use camelCase for all properties
- Use kebab-case for all paths
- Use PascalCase for all schemas
- Use camelCase as names for parameters

## Pagination Rules

Apply pagination to every GET operation that returns a list (i.e. any operation whose response schema is an array or list wrapper).

**Style:** Cursor-based pagination via query parameters.

### Pagination Query Parameters
Every paginated GET operation must include these two reusable parameters:

```yaml
components:
  parameters:
    CursorParameter:
      name: cursor
      in: query
      description: Opaque cursor token for fetching the next or previous page of results, obtained from the navigation links in a previous response
      required: false
      schema:
        type: string
        examples:
          - eyJpZCI6IjEyMyJ9

    PageSizeParameter:
      name: pageSize
      in: query
      description: Number of items to return per page
      required: false
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20
        examples:
          - 20
```

### Paginated List Response Envelope
Every list response schema must use this envelope pattern. Replace `Item` with the actual domain type:

```yaml
ItemList:
  description: Paginated list of items
  type: object
  required:
    - items
    - currentPage
    - links
  properties:
    items:
      description: The items on the current page
      type: array
      items:
        $ref: '#/components/schemas/Item'
    currentPage:
      description: Metadata about the current page of results
      type: object
      required:
        - pageSize
        - itemCount
      properties:
        pageSize:
          description: Maximum number of items per page as requested
          type: integer
          examples:
            - 20
        itemCount:
          description: Actual number of items returned on this page
          type: integer
          examples:
            - 15
    links:
      description: Navigation links for cursor-based pagination
      type: object
      required:
        - self
      properties:
        self:
          description: URI of the current page
          type: string
          format: uri
          examples:
            - https://api.example.com/v1/items?pageSize=20
        next:
          description: URI of the next page; absent if this is the last page
          type: string
          format: uri
          examples:
            - https://api.example.com/v1/items?cursor=eyJpZCI6IjEyMyJ9&pageSize=20
        prev:
          description: URI of the previous page; absent if this is the first page
          type: string
          format: uri
          examples:
            - https://api.example.com/v1/items?cursor=eyJpZCI6Ijc4In0&pageSize=20
        first:
          description: URI of the first page
          type: string
          format: uri
          examples:
            - https://api.example.com/v1/items?pageSize=20
        last:
          description: URI of the last page; may be absent for cursor-based backends that do not support it
          type: string
          format: uri
          examples:
            - https://api.example.com/v1/items?cursor=eyJpZCI6Ijk5OSJ9&pageSize=20
```

### Sorting
Every paginated GET operation must also include the `SortByParameter`. Sorting and pagination always go together.

**Format:** `sortBy={field}:{direction}` — field and direction combined in a single parameter value, comma-separated for multiple sort criteria.

```yaml
components:
  parameters:
    SortByParameter:
      name: sortBy
      in: query
      description: >
        Sorting criteria in the format `{field}:{direction}`, where direction is
        either `asc` or `desc`. Multiple criteria can be combined comma-separated
        (e.g. `createdAt:desc,name:asc`). Allowed fields depend on the resource.
      required: false
      schema:
        type: string
        examples:
          - createdAt:desc
          - createdAt:desc,name:asc
```

**Sortable fields** are resource-specific. For each list endpoint, include a comment in the parameter description listing which fields are sortable, typically:
- `createdAt` — always include if the resource has a creation timestamp
- `updatedAt` — include if the resource has an update timestamp
- Other indexed, stable fields relevant to the domain (e.g. `name`, `status`)

Do NOT include fields that are not indexed or that would be expensive to sort on.

### Pagination Rules Summary
- ALWAYS use `CursorParameter`, `PageSizeParameter`, and `SortByParameter` on any GET that returns a list
- NEVER return a plain array as a response — always wrap in the envelope schema
- Name list schemas as `{Resource}List` (e.g. `BicycleList`, `TaskList`)
- The `next` and `prev` links are OPTIONAL (omit when on the last or first page)
- The `last` link is OPTIONAL — omit if the backend cannot efficiently compute it
- Cursor tokens are opaque strings — do not document their internal format
- Sorting format is always `{field}:{direction}` — never split into two parameters

## Clarification Protocol
If the visual input is ambiguous, ask the user:
1. Which mode are they using? (Canvas+Glossary or ContextMap+Glossary)
2. Who are the API consumers? (affects path naming and operation design)
3. REST or event-driven? (affects whether to use `paths` or `webhooks`)
4. If sticky note labels are partially illegible, list what was read and
     ask the user to confirm before generating the spec

## Output Checklist

- [ ] All glossary terms are represented as schemas
- [ ] All capabilities/use cases from canvas are represented as operations
- [ ] Every operation has at least one success and one error response
- [ ] `info.description` reflects the API's stated purpose from the canvas
- [ ] Every GET returning a list uses `CursorParameter`, `PageSizeParameter`, `SortByParameter`, and the list envelope schema
- [ ] `SortByParameter` description lists the sortable fields for that specific resource

## Verification

Before finalizing the spec:
- If information in the images is unclear, make reasonable assumptions based on API best practices and explain your decisions
- Validate the final specification against OpenAPI 3.1.0 standards before presenting it

## Example

```yaml
openapi: 3.1.0
info:
  title: Task Management API
  description: API for managing tasks between freelancers and assignees
  version: 1.0.0
  contact:
    name: Annegret Junker
    email: annegret.junker@gmx.de
  x-api-id: 8f3d9b2e-7c5f-4e21-aa41-826f8eaf4092
  x-audience: external-public

servers:
  - url: 'https://api.taskmanagement.com/v1'

security:
  - openIdConnect:
      - tasks:read
      - tasks:write

tags:
  - name: Tasks
    description: Task management operations
  - name: Invoices
    description: Invoice management operations
  - name: Users
    description: User management operations

paths:
  /tasks:
    get:
      summary: Get tasks by filters
      description: Responses with a list of tasks fulfilling the search parameter
      operationId: getTasksByFilter
      tags:
        - Tasks
      security:
        - openIdConnect:
            - tasks:read
      parameters:
        - $ref: '#/components/parameters/StatusParameter'
        - $ref: '#/components/parameters/AssigneeParameter'
        - $ref: '#/components/parameters/CreatorParameter'
        - $ref: '#/components/parameters/VersionParameter'
      responses:
        '200':
          $ref: '#/components/responses/TaskListResponse'
        '400':
          $ref: '#/components/responses/BadRequestResponse'
        '403':
          $ref: '#/components/responses/ForbiddenResponse'
        '500':
          $ref: '#/components/responses/ServiceNotAvailableResponse'
        default:
          $ref: '#/components/responses/DefaultResponse'

    post:
      summary: Creates a new task
      description: Creates a new task
      operationId: createTask
      tags:
        - Tasks
      security:
        - openIdConnect:
            - tasks:write
      parameters:
        - $ref: '#/components/parameters/VersionParameter'
      requestBody:
        $ref: '#/components/requestBodies/TaskCreateRequest'
      responses:
        '201':
          $ref: '#/components/responses/TaskCreatedResponse'
        '400':
          $ref: '#/components/responses/BadRequestResponse'
        '403':
          $ref: '#/components/responses/ForbiddenResponse'
        '500':
          $ref: '#/components/responses/ServiceNotAvailableResponse'
        default:
          $ref: '#/components/responses/DefaultResponse'

  /tasks/{taskId}:
    get:
      summary: Get a single task by its ID
      description: Responses with a task identified by its ID
      operationId: getTaskById
      security:
        - openIdConnect:
            - tasks:read
      tags:
        - Tasks
      parameters:
        - $ref: '#/components/parameters/VersionParameter'
        - $ref: '#/components/parameters/TaskIdParameter'
      responses:
        '200':
          $ref: '#/components/responses/TaskResponse'
        '400':
          $ref: '#/components/responses/BadRequestResponse'
        '403':
          $ref: '#/components/responses/ForbiddenResponse'
        '404':
          $ref: '#/components/responses/NotFoundResponse'
        default:
          $ref: '#/components/responses/DefaultResponse'

    put:
      summary: Changes a single task identified by its ID
      description: Updates a task with the given task in the body
      operationId: updateTask
      security:
        - openIdConnect:
            - tasks:write
      tags:
        - Tasks
      parameters:
        - $ref: '#/components/parameters/VersionParameter'
        - $ref: '#/components/parameters/TaskIdParameter'
      requestBody:
        $ref: '#/components/requestBodies/TaskUpdateRequest'
      responses:
        '200':
          $ref: '#/components/responses/TaskResponse'
        '400':
          $ref: '#/components/responses/BadRequestResponse'
        '403':
          $ref: '#/components/responses/ForbiddenResponse'
        '404':
          $ref: '#/components/responses/NotFoundResponse'
        default:
          $ref: '#/components/responses/DefaultResponse'

components:
  parameters:
    TaskIdParameter:
      description: Path parameter to identify a task uniquely
      name: taskId
      in: path
      required: true
      schema:
        type: string
        format: uuid
        examples:
          - 3114e925-6fa4-4805-ae11-4e7b810066e3

    StatusParameter:
      description: Parameter to filter a task by its status
      name: status
      in: query
      schema:
        $ref: '#/components/schemas/TaskStatus'

    AssigneeParameter:
      description: Parameter to find a task by its assignee
      name: assigneeId
      in: query
      schema:
        type: string
        format: uuid
        examples:
          - aeda7f29-2a29-4d54-9e30-54e71bec3a2b

    CreatorParameter:
      description: Parameter to filter a task by its creator
      name: creatorId
      in: query
      schema:
        type: string
        format: uuid
        examples:
          - 8257ae2c-7ef3-45a5-87cb-30a2820ad232

    VersionParameter:
      description: Version of the expected API specification version by the client
      name: x-api-version
      in: header
      required: true
      schema:
        type: string
        default: "1.0.0"
        x-extensible-enum: ["1.0.0"]

  requestBodies:
    TaskCreateRequest:
      description: Task creation request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/TaskToBeCreated'

    TaskUpdateRequest:
      description: Task update request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/TaskToBeUpdated'

  responses:
    TaskResponse:
      description: Single task response
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Task'

    TaskListResponse:
      description: List of tasks
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/TaskList'

    TaskCreatedResponse:
      description: Task created successfully
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/TaskLink'

    BadRequestResponse:
      description: Invalid request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    NotFoundResponse:
      description: Resource not found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    ForbiddenResponse:
      description: Access forbidden
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    ServiceNotAvailableResponse:
      description: Service unavailable
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    DefaultResponse:
      description: General error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

  schemas:
    Task:
      description: A task to be done by the assigned
      type: object
      allOf:
        - $ref: '#/components/schemas/TaskToBeCreated'
        - required:
            - id
            - status
        - properties:
            id:
              description: Identifier of the task
              type: string
              format: uuid
              examples:
                - 3114e925-6fa4-4805-ae11-4e7b810066e3
            status:
              $ref: '#/components/schemas/TaskStatus'
            assigneeId:
              description: Identifier of the person who is assigned to the task
              type: string
              format: uuid
              examples:
                - aeda7f29-2a29-4d54-9e30-54e71bec3a2b
            createdAt:
              description: Date and time when the task was created
              type: string
              format: date-time
              examples:
                - 2025-10-06T13:24Z
            updatedAt:
              description: Date and time when task was updated
              type: string
              format: date-time
              examples:
                - 2025-10-06T13:24Z

    TaskStatus:
      description: Status of a task
      type: string
      x-extensible-enum:
        - NEW
        - IN_PROGRESS
        - DONE
      default: NEW
      examples:
        - NEW

    TaskToBeCreated:
      description: Task to be created in Task Management
      type: object
      required:
        - title
        - creatorId
      properties:
        title:
          description: Title of the task
          type: string
          minLength: 1
          maxLength: 200
          examples:
            - Do something
        description:
          description: Description of the task
          type: string
          maxLength: 2000
          examples:
            - Do something useful
        creatorId:
          description: Identifier of the person who created the task
          type: string
          format: uuid
          examples:
            - 8257ae2c-7ef3-45a5-87cb-30a2820ad232
              
    TaskToBeUpdated:
      description: Task to be updated in Task Management
      type: object
      properties:
        title:
          description: Title of the task
          type: string
          minLength: 1
          maxLength: 200
          examples:
            - Do something
        description:
          description: Description of the task
          type: string
          maxLength: 2000
          examples:
            - Do something useful
        status:
          $ref: '#/components/schemas/TaskStatus'
        assigneeId:
          description: Identifier of the person who is assigned to the task
          type: string
          format: uuid
          examples:
            - aeda7f29-2a29-4d54-9e30-54e71bec3a2b

    TaskList:
      description: List of tasks as response object
      type: object
      required:
        - taskList
      properties:
        taskList:
          description: List of tasks
          type: array
          minItems: 0
          maxItems: 1023
          items:
            $ref: '#/components/schemas/Task'

    TaskLink:
      description: Link to a task
      type: object
      required:
        - taskLink
      properties:
        taskLink:
          description: Link to a task in Task Management
          type: string
          format: uri
          examples:
            - https://apis-online-library/task-management/tasks/3114e925-6fa4-4805-ae11-4e7b810066e3

    Error:
      description: Standard error object
      type: object
      required:
        - code
        - title
      properties:
        code:
          description: Short code of the Error to be recognized easily
          type: string
          minLength: 2
          maxLength: 10
          examples:
            - TASK-001
        title:
          description: Message of the error for better description
          type: string
          minLength: 2
          maxLength: 256
          examples:
            - Task 123 cannot be found.
        detail:
          description: Link to the object handled in the error
          type: string
          format: uri
          examples:
            - https://apis-online-library/task-management/tasks/3114e925-6fa4-4805-ae11-4e7b810066e3
        severity:
          description: Severity of the error
          type: string
          x-extensible-enum:
            - FATAL
            - ERROR
            - WARNING
            - INFO
          default: ERROR
          examples:
            - ERROR
        timestamp:
          description: Timestamp when the error occurs
          type: string
          format: date-time
          examples:
            - 2025-10-03T23:04:24Z

  securitySchemes:
    openIdConnect:
      description: Access to task management via OpenIDConnect
      type: openIdConnect
      openIdConnectUrl: https://auth.taskmanagement.com/.well-known/openid-configuration
```
