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

## Rule Set