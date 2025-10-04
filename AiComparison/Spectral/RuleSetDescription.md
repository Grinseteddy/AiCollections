# Ruleset Description

## 01 Metadata Information

### 01-01
The OpenAPI definition MUST contain a description in the info block.

### 01-02
The info block of an OpenAPI MUST contain contact information.

### 01-03
The info block MUST contain a unique identifier of the API.

### 01-04
The API specification MUST contain a semantic versioning.

### 01-05
The API metadata MUST provide a unique API identifier.

### 01-06
The API metadata MUST define a target group.

### 02 Security

### 02-01
The access security is defined by OpenIdConnect.
The security scope is defined in the components.

### 02-02
Each endpoint MUST contain a security scope following the naming convention:
[bounded-context-name]:[ressource-name]-[read/write/admin]

## 03 Formatting

### 03-01
If using number or integer type, formatting MUST be defined 

if using number types with
- float,
- double, or
- decimal for number types, 

if using integer types with
- int32,
- int64, or
- bigint.

### 03-02
If using date or time properties, the formatting has to be given.
- date-time
- date
- time
- duration
- period

## 04 Identifiers

### 04-01

Identifiers of properties MUST use UUIDs.

## 05 Paths and Responses

### 05-01

The path SHOULD NOT start with /api.

### 05-02

The path segments MUST be separated words in lowercase with hyphens.
The path segments MUST use kebab-cases.

### 05-03

Path MUST use normalized without empty path segments and trailing slashes.

### 05-04

Responses SHOULD use well understood status codes.

### 05-05

The API MUST specify success and error responses.

### 05-06

The API MUST use standard HTTP status codes.

### 05-07

The error responses MUST use Error object.

### 05-08

An endpoint MUST always return a JSON object as top-level data structure.

## 06 Ressource Handling

### 06-01

The API should limit the number of resource types.

### 06-02

The API should limit the number of sub-resource levels.

## 07 Naming Conventions

### 07-01

The API MUST use camelCase for query parameters.

### 07-02

The API MUST use camelCase for property names.

### 07-03

The API SHOULD declare enum values using UPPER_SNAKE_CASE format.

### 07-04

The API SHOULD use hyphenated-pascal-case for header parameters.

## 08 Schema Definitions

### 08-01

Boolean types MUST NOT use Null.

### 08-02

NULL SHOULD NOT be used for empty array.

### 08-03

The schema SHOULD use open-ended list of values (x-extensible-enum) for enum.

## 09 Versioning

### 09-01

The API MUST NOT use URL versioning.

### 09-02

The API MUST use a header parameter for versioning.
 


