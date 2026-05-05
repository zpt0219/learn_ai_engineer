# Week 2 Day 1: RAG Basics & Document Parsing 🗂️

Welcome to Week 2! We are shifting focus to **Retrieval-Augmented Generation (RAG)**—one of the most critical skills for an AI Engineer. RAG allows LLMs to "see" your private data without retraining.

## 📚 Recommended Learning Resources
Before you start answering, these resources will provide a solid foundation:
- **RAG for Beginners:** [IBM Technology - What is RAG? (YouTube)](https://www.youtube.com/watch?v=T-D1OfcDW1M)
- **Chunking Strategies:** [Pinecone - Data Chunking for Vector Databases](https://www.pinecone.io/learn/chunking-strategies/)
- **PDF Parsing Challenges:** [Unstructured.io Blog - The 5 Hardest Problems in PDF Extraction](https://unstructured.io/blog/the-5-hardest-problems-in-pdf-extraction)

---

Please try to answer the following 3 questions in your own words:

---

### 1. RAG Overview & Workflow
**Question:** 
What is **RAG**? Briefly describe the standard RAG process (Parsing, Chunking, Vectorization, Retrieval, Generation). What are the pros and cons of RAG compared to direct fine-tuning?

**Your Answer:**
> RAG is retrieval augmented graph. When you need LLM to quickly locate the useful information with many data or documents. RAG first divide the documents into small chunks, then generate the embedded vector label on each of the chunk, save chunk into a special vector database. So when LLM wanna get documents related to user's question, it can generate a vector based on user's question, then search the database, find severals most matches chunks, retrieve them as reference to help answer user's question better.

compare to direct fine tuning:
Pro: doesn't need to train the model, you are use LLM's logic ability, not the memory of LLM itself. Database can evolve time to time without re-train model. Can fit into different working environments.
Con: generally slower then direct fine tuning, use much more context window.

**Corrected & Refined (AI Revision):**
> **RAG** stands for **Retrieval-Augmented Generation** (not Graph, though "GraphRAG" is a subset).
>
> **Workflow:** 
> 1. **Index Phase:** Load -> Parse -> Chunk -> Embed -> Store (Vector DB). 
> 2. **Query Phase:** User Query -> Embed -> Search (Vector DB) -> Retrieve (Top-K) -> Prompt Augmentation -> LLM Generation.
>
> **Pros vs. Fine-tuning:**
> - **Real-time Updates:** Just update the database; no expensive retraining/fine-tuning.
> - **Traceability:** RAG can cite specific document sources, reducing hallucinations and improving trust.
> - **Privacy:** Easier to control data access permissions at the retrieval layer.
>
> **Cons:**
> - **Retrieval Bottleneck:** If the search returns irrelevant "noise," the model will generate poor answers ("Garbage In, Garbage Out").
> - **System Complexity:** Requires managing a database, embedding models, and retrieval logic.

---

### 2. Document Chunking Strategies
**Question:** 
What are the strategies for **document Chunking**? How do you choose an appropriate Chunk Size and Overlap? Why is Overlap important?

**Your Answer:**
> many strategies, such as, if document is HTML or markdown, we can utilize the format to separate documents by paragraph. And there are "parent-child" type, parent contains multiple paragraph, each paragraph is a child, which has vector embedding, when matched, return parent to provide more context. Use another LLM tool to decide how to devide each chunk. Add some overlap after spliting the docment so the chunk knows some more context. 

appropriate chunk size is depend on paragraph of the doc, generally it should not too small, e.g. My feeling is 500-1000 per chunk. If chunk is too large, needs to divide them. For overlap. normally 10-15% overlap between each chunk.
Overlap is important because it contains the context for the logic why this chunk is introduces and where this chunk probabily directing to. Can help LLM for better decision.

**Corrected & Refined (AI Revision):**
> **Strategies:**
> 1. **Fixed-size/Character Chunking:** Simple but often breaks sentences.
> 2. **Recursive Character Text Splitting:** Splits by a list of separators (e.g., `\n\n`, `\n`, ` `) to keep semantic units together.
> 3. **Document-specific Splitting:** Using Markdown/HTML headers or Python/JS code structure.
> 4. **Hierarchical (Parent-Child):** Retrieve small snippets for precision, but feed the larger "parent" context to the LLM.
> 5. **Semantic Chunking:** Using embedding similarity to detect topic shifts and split accordingly.
>
> **Optimization:** 
> - **Chunk Size:** Usually 300-800 tokens. Smaller = more precise retrieval; Larger = better context for LLM.
> - **Overlap (10-20%):** Essential to ensure that a key concept split across two chunks isn't lost. It maintains "semantic continuity."

---

### 3. Parsing Complex Documents
**Question:** 
What are the common pain points in parsing complex PDFs or web pages (containing tables, images, or multi-column layouts)? What are some mainstream tools or optimization ideas for these "unstructured" documents?

**Your Answer:**
> PDF and web pages are unstructured documents (compare to markdown or json), also the tables, images, etc needs separate algorithm to help retrieving them correctly. 
not sure the mainstream tools. Optimization ideas from my point of view is to extract text first, then extract images, table, etc with position identified, then use tools to convert them, merge back to original text (do some pre-processing), after pre-processing then starts the actual RAG process.

**Corrected & Refined (AI Revision):**
> **Pain Points:** Reading order (multi-column), table structure extraction, and "invisible" text in PDFs.
>
> **Mainstream Tools:**
> - **Unstructured.io:** Excellent for general document partitioning.
> - **PyMuPDF / Marker:** High-speed PDF-to-Markdown conversion.
> - **LlamaParse:** Specialized for complex layouts and tables.
> - **Azure Document Intelligence / AWS Textract:** Enterprise-grade OCR and layout analysis.
>
> **Optimization Strategy (Layout-Aware):**
> Instead of just "flattening" text, use a **Vision-Language Model (VLM)** like GPT-4o or a dedicated layout model (LayoutLM) to convert PDF pages into **Markdown**. Markdown preserves table structures and heading hierarchies, which significantly improves retrieval quality.

---

*💡 Tip: When comparing RAG to Fine-tuning, think about "Knowledge Update Frequency" and "Traceability" (knowing where the answer came from).*

---
*Next Step: Week 2 Day 2 - Vector Databases & Embeddings.*
