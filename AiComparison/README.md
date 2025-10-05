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

### Overview

The detail information can be found below in the descriptions of the result.
The following table gives an overview in comparison of the different LLMs.

| Provider  | Model             | Sync/Async |   Grade    | Details |
|-----------|-------------------|------------|:----------:|---------|
| Anthropic | Claude Sonnet 4.5 | Sync |    good    |[Sonnet 4.5](#Sonnet-45)
| Anthropic | Claude Sonnet 4.5 | Async | very good  |
| Anthropic | Claude Opus 4.1   | Sync |    good    |
| Anthropic | Claude Opus 4.1   | Async | very good  |
| OpenAI    | ChatGPT 5         | Sync |  Adequate  |
| OpenAI    | ChatGPT 5         | Async |  Adequate  |
| OpenAI    | ChatGPT 4 | Sync | Not usable |
| OpenAI    | CChatGPT 4 | Async | Not usable | [ChatGPT 4](#ChatGPT-4)

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

The result is medium.
The generator has not done a separation between event and synchronous API.
The schema does not contain examples nor descriptions.
Book is defined as schema, but is not used, because it is only used in events.
Single type definition of every property increases complexity and makes the definition less human-readable.

[AsyncAPI](./OpenAI/ChatGpt5CatalogManagement.aas.yaml), [Linting AsyncAPI](./OpenAI/ChatGpt5Linting.aas.md), [Chat](https://chatgpt.com/share/68e26c6d-aadc-800e-8173-27a5da6d247a)

The result is medium.
The linting is not free of warnings.
The generated code does not contain examples nor descriptions.
Recognition of definitions which can be taken over is very good.
Code comments are very good.

### ChatGPT 4

[OpenAPI](./OpenAI/ChatGpt4CatalogManagement.oas.yaml), [Linting OpenAPI](./OpenAI/ChatGpt4Linting.oas.md), [Chat](https://chatgpt.com/share/68e275ad-c8e8-800e-a065-5cac4c55b701)

The chat takes a while and several prompts until the result could be reached.

The info block defines events in an extensible tag, which is not necessary.
Not all required response codes are defined.

The result could be usable, but needs a lot of rework.
The linting contains warnings.
There are neither descriptions nor examples.
The security scopes are left out.
Strings do not contain formatting information.

[AsyncAPI](./OpenAI/ChatGpt4CatalogManagement.aas.yaml), [Linting AsyncAPI](./OpenAI/ChatGpt4Linting.aas.md), [Chat](https://chatgpt.com/share/68e27881-07b8-800e-86aa-500600f5c960)

The chat needs two prompts.

The result is not usable.

Not all required response codes are defined.

Descriptions and examples are missing.
The schema formulation needs a lot of rework.
Book is not seen as source of the catalog entry values.

Even though ChatGPT 5 is difficult to use for API generation, there are principle differences between ChatGPT5 and 4.

![Comparison ChatGPT 5 (left) and ChatGPT 4 (right)](./OpenAI/ImageOasComparison.jpg)








