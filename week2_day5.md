# Week 2 Day 5: GraphRAG & Knowledge Graphs 🕸️

Traditional RAG often fails when questions require connecting multiple pieces of information across different documents or understanding the global structure of a dataset. **GraphRAG** addresses this by organizing information into a Knowledge Graph (Entities and Relationships). Today, we dive into why graphs are the "missing link" for complex RAG tasks.

## 📚 Recommended Learning Resources
- **Microsoft GraphRAG (Top Pick):** [Welcome to GraphRAG](https://microsoft.github.io/graphrag/)
- **Anthropic: Contextual Retrieval vs Graph:** [Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) (Compare with GraphRAG)
- **Neo4j: GraphRAG Manifesto:** [The GraphRAG Manifesto](https://neo4j.com/blog/graphrag-manifesto/)
- **Arxiv Paper:** [From Local to Global: A GraphRAG Approach to Query-Focused Summarization](https://arxiv.org/abs/2404.16130)

---

Please try to answer the following 3 questions in your own words:

---

### 1. GraphRAG vs. Vector RAG
**Question:** 
What are the fundamental differences between retrieving raw text chunks (**Vector RAG**) and retrieving subgraphs/relationships (**GraphRAG**)? In what specific scenarios (e.g., "Summarize the major themes of this entire book collection") does Vector RAG typically fail?

**Your Answer:**
> Yes, Vector RAG typically fail fo the Summarize query. Since Vector RAG only retrieves chunks relates to user's query. it doesn't have a high level knowledge to reference. So it can not handle summarize/ long logic chain work well. Graph RAG, on the other hand, treat each chunk as Node and combine similar node together to form a subgraph,and then connect subgraph to build larger graphs, it tries to use LLM to summarize every sub graph and larget graph. So LLM retreiving the subgraph as a whole data and can use the generated context appended on subgraph as a reference doc.

**Corrected & Refined (AI Revision):**
> **Vector RAG** relies on semantic similarity (Point Retrieval), which works well for "needle in a haystack" queries but lacks a "map of the haystack." It fails at summarization because the top-k chunks are often disjointed fragments that don't represent the global themes. **GraphRAG** solves this by pre-summarizing clusters (Communities) of information. It builds a hierarchical structure where low-level entities are clustered into subgraphs, and those subgraphs are clustered into higher-level themes. During retrieval, the LLM treats these community summaries as "synthetic context," allowing it to reason over the entire dataset's structure rather than just local text fragments.

---

### 2. Microsoft GraphRAG: Global vs. Local Search
**Question:** 
Microsoft's GraphRAG distinguishes between **Global Search** and **Local Search**. Explain the difference. How does **"Community Detection"** (clustering) allow the model to answer high-level, dataset-wide questions?

**Your Answer:**
> Global search is retrieving the context already generated for the subgraph to quickly get a summary of the context. Local search is to start from a special node most related to user's query, and then try to expand until reaching threshold or find enough content. then use LLM to make a answer based on these context.
The Community Detection is that at generation stage, LLM will add summary to the community (a node with similar meaning connection).So query stage, LLM doesn't need to go to each node, it simply read the summary, if it matches user's query, then return.

**Corrected & Refined (AI Revision):**
> **Global Search** is designed for dataset-wide questions (e.g., "What are the main risks?"). It bypasses individual nodes and instead queries the pre-generated summaries of the highest-level communities. It aggregates insights across the entire graph. **Local Search** is entity-centric; it retrieves a "subgraph" surrounding a specific entity, including its neighbors, related documents, and community summaries, making it ideal for "Who is X and how are they related to Y?" 
> **Community Detection** (using algorithms like Leiden) is the "magic" of GraphRAG. It clusters related nodes into a hierarchy. At indexing time, an LLM generates a summary for *every* cluster at *every* level. This turns a massive, messy graph into a structured "book of summaries," allowing the model to find the right level of abstraction for any query.

---

### 3. Knowledge Graph Construction Workflow
**Question:** 
Briefly describe the pipeline for building a Knowledge Graph from unstructured text. Why is the "Entity Extraction" step (identifying nodes and edges) so expensive in terms of LLM tokens, and how can we optimize it?

**Your Answer:**
> Because first LLM needs to know the whole context and extract every Entity, then try to connect each Entity, these 2 steps are super heavy.
To optimize, first cache the origin context since it will be referenced multiple time. Then combine similar entity to a small graph, treat the graph as a whole and connect subgraph. repeat until every node is in a top leve graph. To build the hierarchy of the graph system.

**Corrected & Refined (AI Revision):**
> The GraphRAG pipeline involves: **Chunking** -> **Entity/Relationship Extraction** -> **Hierarchical Clustering (Leiden)** -> **Community Summarization**. 
> **Extraction is expensive** because it requires the LLM to parse every chunk multiple times to identify all entities and their nuanced relationships (triplets). The O(N) or O(N^2) complexity of relationship mapping consumes massive token volumes.
> **Optimizations:** 
> 1. **Prompt Engineering:** Use "few-shot" or "gleaning" (asking the LLM if it missed anything in a second pass) to increase extraction density per call.
> 2. **Batching & Deduplication:** Consolidate identical entities across chunks *before* building the graph to reduce the node count.
> 3. **Hierarchical Summarization:** Only summarize communities that have changed during incremental updates, and use cheaper models for lower-level entity extraction while reserving top-tier models for high-level community synthesis.

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### The "Global Reasoning" Gap
Standard Vector RAG is excellent at **Point Retrieval** (e.g., "What is the warranty period for X?"). However, it struggles with:
- **Global Reasoning:** Questions that span the entire dataset (e.g., "What are the top 3 risks mentioned across all 500 reports?").
- **Multi-hop Reasoning:** Questions where Fact A depends on Fact B which depends on Fact C.

### Microsoft GraphRAG Key Concepts
| Feature | Local Search | Global Search |
| :--- | :--- | :--- |
| **Focus** | Specific entities and their neighbors. | Broad themes across the whole dataset. |
| **Mechanism** | Traverses the graph starting from query keywords. | Uses pre-generated **Community Reports**. |
| **Use Case** | "What is the relationship between Alice and Bob?" | "Summarize the main conflicts in the story." |

### Why Community Detection?
Microsoft's approach uses algorithms like **Leiden** to cluster the graph into hierarchical "communities." 
1. **Summarization:** An LLM generates a summary for every community (cluster).
2. **Retrieval:** For global queries, the system searches these summaries instead of raw chunks. This bypasses the "lost in the middle" problem of long context windows and provides a truly holistic view.

### Cypher Query Example (Neo4j)
To gather 2-hop context for an entity to enrich a prompt:
```cypher
MATCH (e:Entity {name: "Project X"})-[r*1..2]-(neighbor)
RETURN e, r, neighbor
```
This structured context is much denser and more relevant than a random bag of text chunks.

---
*Next Step: Week 2 Day 6 - Multimodal RAG & High-Performance Retrieval.*
