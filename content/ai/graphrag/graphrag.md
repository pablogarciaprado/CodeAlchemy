# GraphRAG

◀️ [Home](../../../README.md)

GraphRAG is RAG where R path includes a Knowledge Graph.

![Process Overview](<Screenshot 2026-04-17 at 15.51.11.png>)

## GraphRAG Retrieval Patterns

1. Do a vector search to find an initial set of nodes.
2. Traverse the graph around those nodes to add context.
3. (Optional) Rank the results using the graph and pass the top-k documents to the LLM.

## Benefits

- Higher accuracy
- Easier development
- Explainability & Governance

## Data semantics

Not competing ways of doing it, but complimentary.

![Data semantics 1](<Screenshot 2026-04-17 at 16.00.12.png>)
![Data semantics 2](<Screenshot 2026-04-17 at 16.00.27.png>)
![Data semantics 3](<Screenshot 2026-04-17 at 16.00.46.png>)

## Knowledge Graph Construction

We can have structured, unstructured data, or a mix of both.

### Structured Data

Structured Data with short text values.

- Good methodology,
- Good tools,
- Common in enterprises

### Unstructured Data

Typically PDFs or other text documents.

- Intrinsically hard,
- Immature tooling,
- "hello world" use case

### Mix of Structured & Unstructured Data

Structured Data with long-form text.

- Good methodology,
- Good tools,
- Real-world use case

## Two Types of Knowledge Graphs

### Lexical Graph

A graph representation of for example the words, paragraphs, chunks, documents, and the relationship between them.

### Domain Graph

A graph representation of the real or digital world, i.e. the domain you're modeling.

> These two are not exhaustive. They're also not mutually exclusive.

Sources:

- [GraphRAG: The Marriage of Knowledge Graphs and RAG: Emil Eifrem](https://www.youtube.com/watch?v=knDDGYHnnSI)