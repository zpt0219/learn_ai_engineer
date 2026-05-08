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
> 

**Corrected & Refined (AI Revision):**
> 

---

### 2. Microsoft GraphRAG: Global vs. Local Search
**Question:** 
Microsoft's GraphRAG distinguishes between **Global Search** and **Local Search**. Explain the difference. How does **"Community Detection"** (clustering) allow the model to answer high-level, dataset-wide questions?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 3. Knowledge Graph Construction Workflow
**Question:** 
Briefly describe the pipeline for building a Knowledge Graph from unstructured text. Why is the "Entity Extraction" step (identifying nodes and edges) so expensive in terms of LLM tokens, and how can we optimize it?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

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
*Next Step: Week 3 Day 1 - AI Agent Basics.*
