# Week 4 Day 1: AI System Design & Architecture 🏗️

Welcome to **Week 4**! Having mastered LLM basics, Prompt Engineering, RAG, and Agents, we now shift our focus to **AI System Design & Production Architecture**. In real-world enterprise applications, having a great model is not enough; you must be able to deploy it at scale, manage costs, ensure low latency, and handle high concurrency.

Today's focus is on designing scalable LLM architectures, understanding deployment trade-offs, and implementing model routing strategies.

---

## 📝 Today's Interview Questions

### 1. High-Concurrency LLM Architecture
**Question:** How would you design a scalable architecture for a high-concurrency LLM application (e.g., handling 10k QPS)? Consider load balancing, queueing, and caching.

**Core Requirements:**
- Outline the end-to-end flow from client request to model inference.
- Explain how to handle the slow nature of LLM generation (e.g., async processing, streaming).
- Discuss caching strategies and queue management.

**Draft your answer here:**
> *Your Draft...*

**Corrected & Refined (AI Revision):**
To design a scalable architecture for a high-concurrency LLM application (e.g., 10k QPS), we must decouple the fast components (API gateway, routing) from the slow components (LLM inference) and implement multi-layered optimizations.

A robust architecture includes the following layers:
1.  **API Gateway & Load Balancing:**
    *   **Nginx / Envoy:** Handles SSL termination, rate limiting, and initial load balancing across stateless application servers.
    *   **WebSocket/SSE Support:** Since LLM inference takes time, Server-Sent Events (SSE) or WebSockets are essential to stream tokens back to the client, improving perceived latency (Time To First Token - TTFT).
2.  **Semantic Caching Layer:**
    *   **Redis + Vector DB:** Before hitting the model, we use a semantic cache (e.g., Redis with GPTCache). The user's query is embedded, and if a semantically similar query (cosine similarity > threshold) exists in the cache, the cached response is returned immediately in milliseconds.
3.  **Message Queue / Event Bus (Asynchronous Processing):**
    *   **Kafka / RabbitMQ:** If the cache misses, the request is placed into a message queue. LLM inference is highly compute-bound and takes seconds; direct synchronous API calls would block threads and crash under high load. The queue acts as a shock absorber.
4.  **Inference Workers & Continuous Batching:**
    *   **vLLM / TensorRT-LLM:** Inference workers pull requests from the queue. Instead of processing requests one by one, they use **Continuous Batching** (or iteration-level scheduling) combined with **PagedAttention**. This allows the GPU to process multiple requests dynamically at the token level, drastically increasing throughput.
5.  **Autoscaling & Orchestration:**
    *   **Kubernetes (K8s) + KEDA:** The inference pods are scaled automatically based on the length of the message queue or GPU utilization metrics (e.g., using KEDA).

**Summary for Interviews:** "My design emphasizes decoupling via Kafka, latency reduction via Semantic Caching, and high-throughput inference using vLLM with Continuous Batching. K8s handles dynamic scaling based on queue depth."

---

### 2. Commercial APIs vs. Open-Source Deployment
**Question:** What are the architectural trade-offs between deploying local open-source models (e.g., Llama 3 via vLLM) vs. using commercial APIs (e.g., OpenAI/Anthropic) in an enterprise environment?

**Core Requirements:**
- Compare Cost, Latency, Data Privacy, and Customization.
- Discuss the operational overhead (LLMOps).

**Draft your answer here:**
> *Your Draft...*

**Corrected & Refined (AI Revision):**
When deciding between Commercial APIs and Self-Hosted Open-Source models, enterprise architects must evaluate four primary dimensions: **Data Privacy, Cost, Performance, and Operational Overhead**.

1.  **Data Privacy & Security (The biggest enterprise driver):**
    *   **Open-Source (Local):** Total control over data. Crucial for healthcare (HIPAA), finance, or defense. No PII leaves the VPC.
    *   **Commercial APIs:** Data leaves the enterprise perimeter. Even with "Zero Data Retention" agreements, compliance teams are often hesitant.
2.  **Cost Economics:**
    *   **Commercial APIs:** Opex model (pay-per-token). Very cost-effective for low-to-medium traffic or bursty workloads. No upfront hardware costs.
    *   **Open-Source (Local):** Capex model (buying/renting GPUs). High fixed cost. However, at a massive scale (e.g., billions of tokens per day), self-hosting a smaller model (e.g., Llama 3 8B) becomes significantly cheaper per token than calling GPT-4o.
3.  **Customization & Fine-Tuning:**
    *   **Open-Source:** Full access to model weights. We can perform full fine-tuning, LoRA, and deploy custom inference optimizations (e.g., specific quantization like AWQ/GPTQ).
    *   **Commercial APIs:** Limited to the fine-tuning APIs provided by the vendor (often just SFT), which are restricted and opaque.
4.  **Operational Overhead (LLMOps) & Latency:**
    *   **Commercial APIs:** "Serverless" experience. The vendor handles scaling, load balancing, and uptime. Network latency to the vendor's servers is added.
    *   **Open-Source:** Requires a dedicated ML infrastructure team to manage Kubernetes clusters, GPU drivers, vLLM/TGI setup, and model versioning. Highly complex but allows for latency optimization (e.g., deploying on edge nodes).

**Summary for Interviews:** "For highly sensitive data, strict compliance, or massive predictable scale, self-hosting open-source models is better. For fast time-to-market, complex reasoning capabilities out-of-the-box, and low upfront costs, commercial APIs are superior. Many enterprises use a **hybrid approach**."

---

### 3. LLM Routing Systems
**Question:** How do you design an LLM routing system (Model Router) to dynamically route queries to different models based on complexity, cost, and latency requirements?

**Core Requirements:**
- Explain the concept of LLM Routing.
- Describe the heuristics or ML models used to classify queries.
- Give examples of how this optimizes ROI.

**Draft your answer here:**
> *Your Draft...*

**Corrected & Refined (AI Revision):**
LLM Routing is a system design pattern where an intelligent middleware evaluates an incoming prompt and routes it to the most appropriate model (e.g., routing to GPT-4o for complex reasoning, and Llama-3-8B for simple summarization). The goal is to optimize the **Cost-Performance-Latency** triad.

A Model Router is typically designed with the following mechanisms:
1.  **Rule-Based / Heuristic Routing:**
    *   **Keyword/Regex Matching:** If a query contains "summarize" or "translate", route to a cheap, fast model (e.g., Haiku or Llama 3).
    *   **Length-Based:** If the context is over 100k tokens, route to a model with a massive context window (e.g., Gemini 1.5 Pro).
2.  **Machine Learning / Classifier Routing (Semantic Routing):**
    *   **Embedding + Vector Classification:** We embed the prompt and compare it against a database of clustered queries. For example, if it clusters with "Math/Coding", route to GPT-4o. If it clusters with "Chitchat", route to a smaller model.
    *   **Small LLM / Classifier Model:** Train a very small, fast model (e.g., BERT or a 1B parameter model) specifically to predict the "complexity score" of a prompt in <10ms.
3.  **Cascade Routing (Fallback Strategy):**
    *   Route the query to a small, cheap model first. If the small model outputs a specific token (e.g., "I don't know" or has a low confidence score), the router automatically falls back and re-runs the query on a larger, more expensive model.
4.  **Load/Health-Based Routing:**
    *   The router monitors the latency and rate limits of various APIs. If Azure OpenAI is rate-limited, it routes traffic to Anthropic or a self-hosted backup.

**Summary for Interviews:** "An LLM Router acts as an intelligent API gateway. By using a fast classifier model or semantic embeddings, it predicts query complexity. Simple queries go to fast, cheap models (like Llama 3 8B), while complex reasoning tasks go to GPT-4. This can reduce LLM API costs by up to 80% while maintaining high quality."

---

## 📚 Recommended Resources
- [vLLM: Easy, Fast, and Cheap LLM Serving with PagedAttention](https://vllm.ai/)
- [Semantic Router: Super Fast Decision Making for LLMs](https://github.com/aurelio-labs/semantic-router)
- [RouteLLM: A Framework for LLM Routing (LMSYS)](https://github.com/lm-sys/RouteLLM)
- [Architecting LLM Applications - Databricks](https://www.databricks.com/blog/architecting-llm-applications)
