# Knowledge Graphs

◀️ [Home](../../README.md)

## What is a Knowledge Graph?

A knowledge graph is a database that stores information in nodes and relationships.

Both nodes and relationships can have properties. Nodes can be given labels, which are a way of grouping multiple nodes together. Relationships always have a type and a direction.

## How to Build a Knowledge Graph

### 1. Start with your use case

If you don’t anchor the graph in real questions, it’ll turn into an expensive diagram.

Examples:

- “Which customers are at risk of churn?”
- “Who reports to whom across departments?”
- “What products are related and why?”

This step comes from principles in Knowledge Representation. You’re modeling reality for a purpose, not for completeness.

### 2. Identify entities and relationships

Entities (nodes): these are the “things”.

- Person
- Company
- Product
- Document

Relationships (edges): connect entities:

- works_at
- manages
- purchased
- depends_on

Example:

```txt
Pablo ──works_at──> CompanyX
Pablo ──manages──> Olbap
```

> Keep it simple at first. Start with a Minimum Viable Graph (MVG), then extract, enhance, expand, and repeat to grow the graph. Over-modeling is a common mistake.

### 3. Define a schema (ontology)

This is your structure:

- What types of entities exist?
- What relationships are allowed?
- What properties each entity has?

### 4. Assign unique identifiers

Every entity should have a unique ID.

### 5. Ingest your data

This is where most of the real work happens.

- Structured data: Databases (CRM, ERP), CSV files
- Unstructured data: PDFs, emails, docs

### 6. Store it in a graph database

You need a system designed for relationships.

Popular options that let you query relationships efficiently:

- Neo4j
- Amazon Neptune
- ArangoDB

> Neo4j is a graph database management system designed to store, query, and analyze data as networks of nodes and relationships rather than in traditional tables.

### 7. Query the graph

Instead of SQL, you’ll use graph query languages like Cypher (Neo4j).

Example query:

“Find all employees managed by Pablo who work in Sales”

### 8. Connect it to your AI assistant

Use the graph to answer structured queries directly and/or combine it with RAG.

## RAG vs Knowledge Graphs

### RAG (Retrieval-Augmented Generation)

Built on vector search, typically with embeddings from Natural Language Processing. It's best at:

- Searching unstructured data (docs, PDFs, emails, wikis)
- Answering natural-language questions
- Fast implementation and iteration

Strengths

- Simple architecture
- Works well with messy/internal documents
- Flexible—handles vague queries

Weak spots

- Can miss relationships across documents
- Less reliable for multi-step reasoning
- Harder to enforce strict correctness

### Knowledge Graph

Structured data with entities and relationships (nodes + edges), often used in Knowledge Representation. It's best at:

- Complex relationships (“Who reports to whom across departments?”)
- Logical queries and reasoning
- Situations where correctness and traceability matter

Strengths

- Precise, explainable answers
- Great for multi-hop reasoning
- Strong data governance

Weak spots

- Expensive to build and maintain
- Requires structured/clean data
- Slower to get started

### A practical decision framework

#### 1. What does your data look like?

- Mostly documents, PDFs, Slack, Notion → RAG
- Structured entities (customers, products, org charts) → Knowledge graph

#### 2. What kind of questions will users ask?

- “What’s our refund policy?” → RAG
- “Which customers bought product X and later upgraded within 6 months?” → Knowledge graph

#### 3. Do relationships matter deeply?

If your assistant needs to understand chains like:

- employee → manager → department → budget

then knowledge graphs shine.

If it’s mostly:

- “find relevant info and explain it”

RAG is enough.

#### 4. How important is correctness vs speed?

- Need fast MVP → RAG
- Need auditable, deterministic answers → Knowledge graph

### Hybrid approach

Often the sweet spot is not having to pick one:

Use RAG for:

- Document retrieval
- General Q&A

Use knowledge graph for:

- Core business entities
- Relationship-heavy queries

Let the assistant decide which to query.

> Graph → precise relationships
> RAG → unstructured knowledge

### Concrete examples

RAG:

- Internal company chatbot
- Support assistant
- Documentation search tool

Knowledge graphs:

- Fraud detection system
- Enterprise analytics assistant
- Compliance or legal reasoning system

## Interesting Resources

- [Course: Knowledge Graphs for RAG](https://learn.deeplearning.ai/courses/knowledge-graphs-rag/)
- [Neo4j Cypher Query Language](https://neo4j.com/product/cypher-graph-query-language/)
