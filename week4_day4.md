# Week 4 Day 4: Domain-Specific AI & Enterprise Integration

## 1. How do you adapt an LLM for highly specialized domains (e.g., Medical, Legal, Financial) ensuring strict compliance and zero hallucination?

Adapting an LLM for highly specialized, high-stakes domains requires a multi-layered approach prioritizing accuracy, auditability, and data security over general creativity.

### 1. Domain-Specific RAG (Retrieval-Augmented Generation)
*   **The Foundation:** Rather than relying solely on the LLM's internal weights, use a strict RAG pipeline grounded entirely in verified, domain-specific corpora (e.g., internal legal contracts, FDA guidelines, enterprise financial reports).
*   **Strict Grounding:** Instruct the model via system prompts to answer *only* using the provided context. If the answer is not in the context, the model must explicitly state "I do not have enough information."

### 2. Fine-Tuning (Continual Pre-training & SFT)
*   **Domain Adaptation:** If the vocabulary is highly specialized (e.g., medical jargon), Continual Pre-Training (CPT) on domain literature helps the model understand the specific terminology.
*   **Supervised Fine-Tuning (SFT):** Fine-tune the model on high-quality instruction-response pairs curated by domain experts to teach it the expected format, tone, and compliance constraints.

### 3. Strict Output Constraints & Guardrails
*   **Format Constraints:** Use structured output (like JSON Schema) to enforce the shape of the data.
*   **Output Guardrails (e.g., NeMo Guardrails):** Implement a secondary verification layer that checks the LLM's output against compliance rules, PII (Personally Identifiable Information) filters, and prohibited topics before showing it to the user.

### 4. Hallucination Mitigation (Zero Hallucination Goal)
*   **Citation & Provenance:** Require the model to cite the specific document and paragraph it used to generate the answer.
*   **Self-Verification/Multi-Agent Review:** Use a secondary "Critic" agent whose sole job is to cross-check the generated answer against the retrieved context to detect hallucinations or unsupported claims.

---

## 2. What is Text-to-SQL? What are the common architectures and challenges when allowing LLMs to interact directly with enterprise relational databases?

**Text-to-SQL** is the task of translating a natural language question (e.g., "Show me the top 5 customers by revenue this quarter") into a valid, executable SQL query.

### Common Architectures for Text-to-SQL:
1.  **Schema-Aware Prompting:** 
    *   The prompt includes the database schema (table names, column names, data types, and primary/foreign key relationships) and sometimes a few example rows.
    *   The LLM generates the SQL based on this context.
2.  **RAG for Schema/Queries:** 
    *   For massive enterprise databases with hundreds of tables, the entire schema cannot fit in the context window.
    *   A vector database is used to retrieve only the *relevant* table schemas or similar historical Text-to-SQL examples (Few-Shot RAG) based on the user's query.
3.  **Agentic Execution & Correction:**
    *   The LLM generates SQL -> a secure agent executes it -> if it fails (syntax error, missing column), the error is fed back to the LLM to fix the query.

### Primary Challenges:
*   **Hallucinations (Invalid SQL):** Hallucinating column names that don't exist or misunderstanding table joins.
*   **Security & Data Privacy (Crucial):** 
    *   **SQL Injection:** Preventing the LLM from generating malicious queries (e.g., `DROP TABLE`).
    *   **RBAC (Role-Based Access Control):** Ensuring the LLM only queries data the user is authorized to see. Best practice is executing the SQL using a database role restricted to read-only access and the user's permissions.
*   **Ambiguity in Natural Language:** "Revenue" might mean Gross Revenue or Net Revenue. Enterprise data requires precise definitions.

---

## 3. Describe an architecture for an AI-powered coding assistant (like GitHub Copilot) at an enterprise scale. How do you handle codebase context?

An enterprise coding assistant requires low latency, strict code privacy, and the ability to understand a massive, proprietary codebase that far exceeds the context window of any LLM.

### High-Level Architecture:
1.  **IDE Plugin (Client):** The developer's interface (VS Code, IntelliJ). It captures the current cursor position, recent edits, open files, and IDE state.
2.  **Context Engine (Backend):** The orchestrator that processes the client request, retrieves necessary context, and constructs the prompt.
3.  **LLM Router / Inference Engine:** Routes queries to different models (e.g., a fast, small model for inline autocomplete, a large model for complex chat/refactoring) and handles high-throughput inference (e.g., using vLLM).

### Handling Codebase Context (The Core Challenge):
You cannot send the entire repo in every prompt. You must intelligently retrieve context:

1.  **Local IDE Context (Heuristics):**
    *   Include the currently open file (or sliding window around the cursor).
    *   Include recently viewed/edited files.
    *   Include imported libraries and function signatures present in the current file.
2.  **Repository-Level RAG (Vector Search):**
    *   Parse the entire codebase (using AST - Abstract Syntax Trees) into functions, classes, and documentation.
    *   Embed these code chunks into a Vector Database.
    *   When the user asks "How is authentication handled?", use vector search to retrieve the relevant auth modules.
3.  **Symbolic Search / Code Graph:**
    *   Use exact keyword search or LSP (Language Server Protocol) tools to find exact definitions and references of variables/functions. E.g., if the user types `getUser()`, the system retrieves the definition of `getUser` from another file.
4.  **Prompt Assembly & Reranking:**
    *   Combine the local context, retrieved snippets, and symbolic data.
    *   Rerank them to fit within the model's context window (e.g., 32k tokens), prioritizing the most immediately relevant code.
