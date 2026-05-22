# Week 4 Day 5: AI System Security, Privacy & Red Teaming

## 1. What is Red Teaming in the context of LLMs? How do you systematically test an AI application for vulnerabilities?

**LLM Red Teaming** is the process of adversarially probing, testing, and attacking an AI system to identify security vulnerabilities, biases, potential for policy violations, and unexpected behaviors before deployment.

### Common LLM Vulnerabilities:
*   **Prompt Injection:** Tricking the model into ignoring its system instructions to execute malicious user instructions.
*   **Jailbreaking:** Using clever prompting (e.g., roleplay, hypothetical scenarios) to bypass safety guardrails and force the model to output harmful content.
*   **Data Leakage:** Forcing the model to reveal sensitive system prompts or confidential training data.

### Systematic Testing Methods:
1.  **Manual Red Teaming:** Safety experts craft creative, adversarial prompts to manually probe the system for weaknesses.
2.  **Automated Adversarial Probing (LLM-as-a-Red-Teamer):** Use a secondary, uncensored "Red-Team LLM" programmed to automatically generate hundreds of diverse attack prompts against the target system.
3.  **Vulnerability Scanners (Framework-based):** Integrate open-source tooling (e.g., **Promptfoo**, **Giskard**, **Garak**) into CI/CD pipelines to run standardized regression tests for jailbreaks, bias, and injection.
4.  **Guardrail Evaluation:** Specifically test the performance of output filters (like Llama Guard or NeMo Guardrails) to see how effectively they block toxic or unsafe outputs.

---

## 2. How do you protect data privacy in Enterprise AI applications (e.g., PII redaction, Data Loss Prevention, RBAC for RAG)?

Enterprise environments demand strict compliance (GDPR, HIPAA) and zero leak of proprietary IP. Protecting privacy requires a multi-layered security architecture:

### 1. PII Redaction & Data Anonymization (Input Sanitization)
*   **Mechanism:** Deploy a lightweight, local model or rule-based engine (e.g., **Microsoft Presidio**) before the LLM gateway.
*   **Action:** Detect and redact Personally Identifiable Information (PII) like names, emails, SSNs, and credit card numbers from user queries, replacing them with placeholders (e.g., `[REDACTED_NAME]`) before forwarding to third-party APIs.

### 2. Enterprise Data Loss Prevention (DLP) Gateways
*   **Mechanism:** Run incoming and outgoing traffic through a proxy/gateway.
*   **Action:** Intercept and block queries containing intellectual property (e.g., source code, internal financial spreadsheets) and block model responses containing sensitive system internals.

### 3. Role-Based Access Control (RBAC) in RAG
*   **Mechanism:** Prevent unauthorized information retrieval at the database level.
*   **Action:** 
    *   **Metadata Tagging:** When chunking and embedding documents, attach access control lists (ACLs/roles) as metadata (e.g., `{"roles_allowed": ["HR", "Admin"]}`).
    *   **Pre-filtering:** During retrieval, pass the user's role tokens to the vector database. The database filters search results *before* similarity matching, ensuring the user only retrieves chunks they are authorized to view.

---

## 3. What are Data Poisoning and Model Inversion attacks? How do they affect the security of fine-tuned models?

When we fine-tune models (SFT or Continual Pre-training) on custom datasets, we expose them to unique vulnerabilities.

### 1. Data Poisoning Attacks
*   **What it is:** A malicious actor tampers with the fine-tuning dataset, injecting corrupted, biased, or "backdoored" training examples.
*   **How it affects the model:**
    *   **Backdoor Trigger:** The model performs perfectly on normal inputs, but when a specific "trigger" phrase/token is used, it outputs a malicious response (e.g., executing a remote shell payload or recommending an insecure software package).
*   **Mitigation:** Strict data curation pipelines, automated outlier detection, and auditing fine-tuning data sources.

### 2. Model Inversion / Reconstruction Attacks
*   **What it is:** An attacker queries the fine-tuned model repeatedly (often accessing log probabilities or outputs) to reconstruct the private data used in training.
*   **How it affects the model:**
    *   **Privacy Leak:** Because models tend to memorize training data, an attacker can extract sensitive training samples (e.g., proprietary code, patient medical records) simply by prompting the model strategically.
*   **Mitigation:** 
    *   **Differential Privacy (DP-SGD):** Inject controlled noise during fine-tuning to prevent the model from memorizing exact training samples.
    *   **API Security:** Restrict public access to log probabilities, rate-limit queries, and deploy output guardrails to prevent literal regurgitation of memorized text.
