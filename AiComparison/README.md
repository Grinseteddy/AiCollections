# AI Comaparison

This is path contains comparisons of generated APIs with different LLMs in different versions.
The APIs are created based 
- on a picture of an API Product Canvas _(see Junker, A; Lazzaretti, F. (2025) Crafting Great APIs using DDD, Apress)_,
- a Visual Glossary _(see   Zörner, S. (2015). Softwarearchitekturen dokumentieren und kommunizieren, Entwürfe, Entscheidungen und Lösungen nachvollziehbar und wirkungsvoll festhalten (Document and Communicate Software Architectures: Traceable and Effective Capturing of Decisions and Solutions). München: Hanser Verlag.)_
- an example in OpenAPI and AsyncAPI.

The generated APIs are linted with the [Spectral](https://docs.stoplight.io/docs/spectral/674b27b261c3c-overview) linter and custom rules.

## Scenario

The scenario handles the bounded context _Catalog Management_ in an online library. 

The _Catalog Management_ bounded context creates, updates, and deletes Catalog Entries in teh book catalog. Those entries can be created automatically triggered by new purchases or can be cared for by a librarian.

## API Product Canvas

The following figure shows the API Product Canvas of the bounded context.

![API Prodcut Canvas Catalog Management](./Junker_AIandDDD_APIProductCanvas.jpg)

The canvas shows the endpoints of a REST-API and the events for asynchronous communication.

## Visual Glossary

The belonging Visual Glossary shows the domain model of the bounded context with the aggregate _Catalog Entries_.

![Visual Glossary Catalog Management](Junker_AIandDDD_VisualGlossary.jpg)

## Example

As OpenAPI example a simple [Task Management API](./TaskManagement.yaml) is used.

## Prompts

The prompt for the OpenAPI Generation: [Prompt](PromptOpenApi).

## Linting

The generated results are linted using the standard rule set by [Spectral](https://docs.stoplight.io/docs/spectral/aa15cdee143a1-java-script-ruleset-format).

## Results
 

### Anthropic

#### Sonnet 4.5

[OpenAPI](./Claude/Sonnet45CatalogManagement.oas.yaml),
[Linting OpenAPI](./Claude/Sonnet45Linting.oas.md), [Chat](https://claude.ai/share/85a0b43e-6ca0-4c51-92a9-df5b8601509f)

The result is good and satisfying.
Examples in the schemas are missing.

#### Opus 4.1

[OpenAPI](./Claude/Opus41CatalogManagement.oas.yaml), [Linting OpenAPI](./Claude/Opus41Linting.oas.md), [Chat](https://claude.ai/share/d262fcf6-6097-4523-a90b-63c4a9393596)

The result is good.
The generator has not done a separation between event and synchronous API. 
Book is defined as schema, but is not used, because it is only used in events.




