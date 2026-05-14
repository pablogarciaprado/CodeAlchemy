# Vector retrieval for RAG

◀️ [Home](../../../README.md)

Retrieval is the step that decides which passages the model will see. In semantic RAG, you turn text into **embeddings** (fixed-length vectors) and ask a **vector index** to return the nearest neighbors to the query embedding—fast enough to run on every request, even when the corpus is large.

That pipeline sounds simple, but the details matter: embedding **dimension** constrains storage and quality; **search algorithms** decide how you approximate nearest-neighbor search; **distance metrics** define what “near” means; **index/query consistency** and **metadata filtering** keep results trustworthy and correctly scoped; **re-ranking** can refine the final ordering; and **efficiency** choices (truncated dimensions, normalization) affect both cost and accuracy. The sections below walk through those pieces in the order you usually reason about them when designing or tuning a system.

## Size

In a RAG system, the vector dimension should match the output dimension of the embedding model you use. There’s no universally “correct” size — the important rule is: **All vectors in the same index must have the same dimensionality**.

> In vector databases, an index is the data structure that lets you search embeddings efficiently.

Think of dimensions as the number of "coordinates" used to describe the meaning of a piece of text. More dimensions can capture more nuance, but they also require more storage and computational power.

> The vector DB does not decide the dimension. The embedding model decides it.

### Small dimensions (384–768)

- fast retrieval
- lower memory usage
- cheaper ANN indexes
- smaller datasets

Tradeoff:

- slightly lower semantic precision

Typical use:

- prototypes
- small RAG apps
- edge/local deployments

### Medium dimensions (1024–1536)

This is the current “sweet spot” for many production RAG systems.

Good balance of:

- semantic quality
- retrieval accuracy
- storage cost
- latency

### Large dimensions (2048–4096+)

Useful when:

- documents are highly nuanced
- retrieval quality is critical
- multilingual/domain-specific semantics matter

Tradeoffs:

- larger indexes
- more RAM
- slower similarity search
- more expensive storage

These are common in:

- legal
- biomedical
- code retrieval
- research systems

## Search Algorithms

The search algorithm is the strategy used to find the best results without looking at everything. It uses shortcuts, indexes, and smart grouping to ignore 99% of the database that is obviously irrelevant. In the world of RAG and vector databases, a Search Algorithm is the logic used to find the most relevant "needles" in a massive "haystack" of data.

To understand search algorithms, we first have to distinguish between how they look for information:

- Keyword Search (Lexical): Uses algorithms like BM25. It looks for exact character matches. If you search for "feline," it might miss a document that only uses the word "cat."

- Vector Search (Semantic): Uses algorithms like HNSW (Hierarchical Navigable Small Worlds) or IVF (Inverted File Index). It looks for mathematical proximity. It knows "feline" and "cat" are neighbors in vector space.

The most advanced RAG systems don't pick just one; they use a Hybrid Search algorithm. It runs a Keyword Search (BM25) and a Vector Search (HNSW) simultaneously, then combines the results using a technique called Reciprocal Rank Fusion (RRF). This ensures that if a user searches for a specific part number (exact match) OR a general concept (semantic), the algorithm finds both.

## Distance Metrics

The distance metric is a mathematical formula. It defines the "ground truth" of how similar two items are. It compares two vectors and outputs a single number.

> Slow if used alone. To find the "best" result in a database of 1 million items using only a distance metric, you would have to run that math formula 1 million times. This is called a Brute Force or Linear Search.

### Cosine Similarity

When calculating the similarity between vectors, you'll use a distance metric. The most common for RAG is **Cosine Similarity**, calculated as:

$$\text{similarity} = \cos(\theta) = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|}$$

> It focuses entirely on the angle between them rather than the distance between their points.
>
> In the context of RAG, it tells the system: "How much is the 'meaning' of the user's query pointing in the same direction as this chunk of text?"

If your vectors are normalized (length of 1), Cosine Similarity is mathematically equivalent to the **Dot Product**, which is much faster for your CPU/GPU to compute.

Cosine Similarity, mathematically, calculates the cosine of the angle $\theta$ between two vectors.

- Result of 1: The vectors are identical in direction (maximum similarity).
- Result of 0: The vectors are orthogonal (90°, no similarity).
- Result of -1: The vectors are diametrically opposed (opposite meaning).

### Dot Product

The Dot Product is Cosine Similarity's faster, "no-frills" cousin.

$A \cdot B = \sum (A_i \times B_i)$

Use this if your vectors are normalized (length of 1). If they are normalized, the math for Dot Product and Cosine Similarity is effectively the same, but Dot Product is much faster to compute because it skips the division and square root steps. This is the industry standard for production RAG systems using Gemini or OpenAI embeddings.

### Euclidean Distance ($L_2$)

This measures the straight-line distance between two points in space.

$\sqrt{\sum (A_i - B_i)^2}$

Use this if the magnitude of your vectors matters (e.g., if the length of the vector represents how "important" or "frequent" a concept is).

> The length (often called the $L_2$ Norm) is a single scalar value representing the vector's magnitude in space. It is calculated using the Pythagorean theorem across all those dimensions. It is calculated as the square root of the sum of the squared components.
>
> $$\|\mathbf{v}\|_2 = \sqrt{x_1^2 + x_2^2 + \dots + x_n^2}$$

Generally less effective for text RAG than Cosine Similarity because long documents might have higher magnitude vectors simply due to word count, which can skew the "distance".

### Manhattan Distance ($L_1$)

Imagine traveling along a grid (like city blocks) rather than a straight line. Mostly used in high-dimensional spaces where "outliers" are a concern, as it is less sensitive to extreme values than Euclidean distance. Rarely the first choice for RAG, but common in specific types of recommendation engines.

## How do Search Algorithms and Distance Metrics work together?

When you query your vector database:

1. The Search Algorithm (e.g., HNSW) quickly narrows down the 1,000,000 vectors to a "neighborhood" of perhaps 100 likely candidates.

2. The Distance Metric (e.g., Cosine Similarity) is then used to precisely rank those 100 candidates to give you the final Top 5 results.

> You choose a Distance Metric based on your embedding model, and you choose a Search Algorithm based on how much data you have and how fast you need your RAG system to be.

## Index and query consistency

For scores and rankings to mean anything, **documents at index time** and **queries at search time** must go through the **same embedding pipeline**.

Checklist:

- **Same model and version** — Swapping the model or a materially different checkpoint usually means **re-embedding the whole corpus** (or running separate indexes per model version).
- **Same output dimension** — Must match what the index was built for; if you use MRL-style truncation, use the **same** truncated width for every stored vector and for every query.
- **Same optional prefixes or prompts** — Many models expect different text wrappers for queries vs passages (for example `query: …` vs `passage: …`). Follow provider documentation exactly, and do not mix conventions between indexing and search.
- **Same preprocessing** — Truncation limits, unicode normalization, pooling, and any batching quirks should match what you used when you built the index.
- **Same normalization policy** — If you L2-normalize stored vectors, normalize query vectors the same way; if the model was trained for raw magnitudes and Euclidean geometry, keep them raw.

> A common failure mode is **train/serve skew**: the corpus was embedded with one code path and queries with another. Retrieval still returns results, but **ordering is wrong** and debugging is slow because nothing throws a hard error.

## Metadata filtering

Chunks almost always carry **metadata** (tenant id, user id, product, language, validity dates, `doc_id`, source URL, and so on). **Filtering** restricts candidate vectors to rows that satisfy those predicates, so you do not surface another customer’s documents, the wrong locale, or revoked policy text.

Two patterns:

- **Pre-filtering** — Metadata predicates are applied **during** or **before** approximate nearest-neighbor search, so neighbors are drawn only from the allowed subset. Works well when filters are selective and the engine integrates filters with the ANN structure (graph, IVF lists, etc.).
- **Post-filtering** — Retrieve **more** than `top_k` hits, drop anything that fails metadata checks, then keep the first `k` survivors. Easy to implement, but risky: if the top vector hits are almost all outside the filter, you return a **short or empty** list. Mitigations include **oversampling** (request several times `top_k` before filtering), tuning ANN parameters, or leaning on hybrid lexical paths when filters are very selective.

> With **hybrid search**, apply filtering **consistently** on both the vector and lexical sides before you fuse ranks (for example with RRF), or you can bias one branch and starve the other.

## Re-ranking

The first retrieval stage optimizes for **speed and recall**: bi-encoder embeddings plus ANN (and optionally BM25 with fusion) return a **candidate set**, often on the order of tens to a few hundred chunks.

> The name "Bi-Encoder" comes from the fact that the model processes two inputs (like the Query and the Passage) as two independent streams.

**Re-ranking** adds a second stage: a model scores each `(query, chunk)` **pair** and **reorders** only that small list. It is too expensive to run across the full corpus, but cheap enough on a short list to improve **precision at the top**—what the generator actually reads. There are two different ways to solve the same problem:

- **Cross-encoder** — A single Transformer sees the query and the passage **in one joint sequence** (with special separators). Self-attention can relate every query token to every passage token, and the model outputs one **relevance score** per pair. Nothing is precomputed for the corpus: each candidate needs its own forward pass, but those passes are still much smaller and more predictable than a full LLM call.

- **LLM reranking** — You prompt a general-purpose LLM with the query and each candidate (or a batched list) and ask for a score, a rank, or a short justification you map to a score. You gain **flexibility** (rubrics, domain instructions, “prefer primary sources”), but you pay **higher latency and cost**, deal with **format reliability** (unless you use structured output), and behavior can be **less stable** across model versions than a dedicated reranker.

In practice: **cross-encoders** (or API rerank endpoints backed by them) are the default when you need cheap, high-throughput rescoring; **LLM reranking** is for smaller candidate sets or when instructions and reasoning about the pair matter more than raw throughput.

Tradeoffs:

- **Latency and cost** — One or more extra model calls per query; batching, smaller rerankers, or async pipelines help.
- **Context limits** — Rerankers cap input length; very long chunks may need truncation or a hierarchical approach.
- **When it is optional** — Tiny corpora, very tight latency budgets, or when first-stage retrieval is already strong; otherwise re-ranking is a common production upgrade path.

## Thinking about efficiency

### Controlling embedding size

Nowadays, embedding models are usually trained using the Matryoshka Representation Learning (MRL) technique which teaches a model to learn high-dimensional embeddings that have initial segments (or prefixes) which are also useful, simpler versions of the same data. For example, both `gemini-embedding-001` and `gemini-embedding-2` are.

Normally there's a `output_dimensionality` parameter to control the size of the output embedding vector. Selecting a smaller output dimensionality can save storage space and increase computational efficiency for downstream applications, while sacrificing little in terms of quality. By default, the previously mentioned models output a 3072-dimensional embedding, but you can truncate it to a smaller size without losing quality to save storage space. We recommend using 768, 1536, or 3072 output dimensions.

> Rule of thumb for a first serious implementation of a RAG system
>
> - use 768 or 1536 dimensions
> - prefer a modern embedding model over “more dimensions”
> - chunking quality matters more than vector size
> - retrieval strategy matters more than vector

### Normalizing Vectors

Normalizing vectors is a standard practice in RAG systems, essentially ensuring that every vector has a "length" of 1 while maintaining its original direction. Mathematically, this means the vector is projected onto a unit hypersphere.

For a vector $\mathbf{v} = [x_1, x_2, \dots, x_n]$, the norm is calculated as:$$\|\mathbf{v}\| = \sqrt{x_1^2 + x_2^2 + \dots + x_n^2}$$The normalized vector $\mathbf{\hat{v}}$ is then:$$\mathbf{\hat{v}} = \frac{\mathbf{v}}{\|\mathbf{v}\|}$$

In code:

```py
import numpy as np

vector = np.array([3, 4])
norm = np.linalg.norm(vector)
normalized_vector = vector / norm
# result: [0.6, 0.8]
```

#### Why Normalize? (The Benefits)

- **Computational Efficiency**: If all vectors in your database are normalized, Cosine Similarity becomes identical to the Dot Product. Calculating a Dot Product is significantly faster for hardware (GPUs/CPUs) because it avoids the square root and division operations during the search phase.

- **Consistency**: It removes the influence of "magnitude." In embeddings, magnitude can sometimes be an artifact of how often a word appeared in training rather than what it actually means. Normalizing ensures you are comparing the "angle" (meaning) only.

#### Are there Drawbacks?

While normalization is standard, there are a few "gotchas" to keep in mind:

1. **Loss of Magnitude Information**: In some rare models, the length of a vector actually carries semantic meaning (e.g., the "strength" or "confidence" of a concept). If you normalize, that information is permanently discarded. However, for 99% of RAG use cases involving modern LLM embeddings, this is not an issue.

2. **Model Specificity**: The Golden Rule: You must check if your embedding model was trained to be used with normalized vectors. If a model was trained using Euclidean Distance ($L_2$), it expects the magnitudes to stay intact. If you normalize vectors for a model that wasn't designed for it, your retrieval accuracy will actually drop.

3. **Precision Errors**: If you have extremely small values in your vectors, dividing by a very small norm can lead to floating-point precision issues (though this is rare with standard 32-bit floats).
