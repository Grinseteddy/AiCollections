# AI Comparison

This is path contains comparisons of generated APIs with different LLMs in different versions.
The APIs are created based 
- on a picture of an API Product Canvas _(see Junker, A; Lazzaretti, F. (2025) Crafting Great APIs using DDD, Apress)_,
- a Visual Glossary _(see Zörner, S. (2015). Softwarearchitekturen dokumentieren und kommunizieren, Entwürfe, Entscheidungen und Lösungen nachvollziehbar und wirkungsvoll festhalten (Document and Communicate Software Architectures: Traceable and Effective Capturing of Decisions and Solutions). München: Hanser Verlag.)_
- an example in OpenAPI and AsyncAPI.

The generated APIs are linted with the [Spectral](https://docs.stoplight.io/docs/spectral/674b27b261c3c-overview) linter and custom rules.

## Scenario

The scenario handles the bounded context _Catalog Management_ in an online library. 

The _Catalog Management_ bounded context creates, updates, and deletes Catalog Entries in the library catalog. Those entries can be created automatically triggered by new purchases or can be cared for by a librarian.

## API Product Canvas

The following figure shows the API Product Canvas of the bounded context.

![API Prodcut Canvas Catalog Management](./Junker_AIandDDD_APIProductCanvas.jpg)

The canvas shows the endpoints of a REST-API and the events for asynchronous communication.

## Visual Glossary

The belonging Visual Glossary shows the domain model of the bounded context with the aggregate _Catalog Entries_.

![Visual Glossary Catalog Management](Junker_AIandDDD_VisualGlossary.jpg)

For the asynchronous communication the Visual Glossary needs to be enhanced by Published Language models of the events consumed by the bounded context. Therefore, the Visual Glossary is enhanced to cover the consumed events from Purchase.

![Enhanced Visual Glossary](./Junker_AIandDDD-Visual_Glossary_with_Book.jpg)

## Example

As OpenAPI example a simple [Task Management API](./TaskManagement.yaml) is used.

As AsyncAPI example the definition of an [Inventory Management](./InventoryManagement.aas.yaml) of an online library is used. It contains besides the event and schema definition the necessary Kafka bindings.

## Prompts

- The prompt for the OpenAPI generation: [Prompt](PromptOpenApi).
- The prompt for the AsyncAPI generation: [Prompt](./PromptAsyncApi.md).

## Linting

The generated results are linted using the standard rule set by [Spectral](https://docs.stoplight.io/docs/spectral/aa15cdee143a1-java-script-ruleset-format).

## Results

### Anthropic

#### Sonnet 4.5

[OpenAPI](./Claude/Sonnet45CatalogManagement.oas.yaml),
[Linting OpenAPI](./Claude/Sonnet45Linting.oas.md), [Chat](https://claude.ai/share/85a0b43e-6ca0-4c51-92a9-df5b8601509f)

The result is good and satisfying.
Examples in the schemas are missing.

[AsyncAPI](./Claude/Sonnet45CatalogManagement.aas.yaml),
[Linting AsyncAPI](./Claude/Sonnet45Linting.aas.md),
[Chat](https://claude.ai/share/dfa9af37-d00b-461b-8fa0-2f9c25d25c2f)

The result is very good.
Examples are given based on the near example of the same domain.
The linting shows no errors.

#### Opus 4.1

[OpenAPI](./Claude/Opus41CatalogManagement.oas.yaml), [Linting OpenAPI](./Claude/Opus41Linting.oas.md), [Chat](https://claude.ai/share/d262fcf6-6097-4523-a90b-63c4a9393596)

The result is good.
The generator has not done a separation between event and synchronous API. 
Book is defined as schema, but is not used, because it is only used in events.

[AsyncAPI](./Claude/Opus41CatalogManagement.aas.yaml), [Linting AsyncAPI](./Claude/Opus41Linting.aas.md), [Chat](https://claude.ai/share/2131b43e-5379-4ace-84f5-fe94a219e1ca)

The result is very good.
Examples are given based on the example of the same domain.
The linting shows no errors.

The AsyncAPIs of Sonnet4.5 (left) and Opus4.1 (right) are almost identical.
![Comparison AsyncAPI Sonnet4.5 and Opus4.1](./Claude/ImageAasComparison.jpg)

### OpenAI

#### ChatGPT 5

[OpenAPI](./OpenAI/ChatGpt5CatalogManagement.oas.yaml), [Linting OpenAPI](./OpenAI/ChatGpt5Linting.oas.md), [Chat](https://chatgpt.com/share/68e26af7-4df0-800e-b4e8-6749a3ce586b)

The result is good.
The generator has not done a separation between event and synchronous API.
The schema does not contain examples.
Book is defined as schema, but is not used, because it is only used in events.
Single type definition of every property increases complexity and makes the definition less human-readable.

[AsyncAPI](./OpenAI/ChatGpt5CatalogMangement.aas.yaml), [Linting AsyncAPI](./OpenAI/ChatGpt5Linting.aas.md), [Chat](https://chatgpt.com/share/68e26c6d-aadc-800e-8173-27a5da6d247a)

The result is good.
The linting is not free of warnings.
The generated code does not contain examples.
Recognition of definitions which can be taken over is very good.
Code comments are very good.






