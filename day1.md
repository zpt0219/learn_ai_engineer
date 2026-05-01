# Day 1: Transformer & LLM Foundations 🚀

Welcome to Day 1 of your AI Engineer interview preparation! Today we focus on the bedrock of modern AI: the Transformer architecture and how models process text.

Please try to answer the following 3 questions in your own words. You can write your answers directly below each question.

---

### 1. What is the Transformer model?
**Question:** 
Briefly describe its core architecture (Encoder-Decoder vs. Decoder-only). Why are most of today's LLMs (like GPT, Llama, Claude) Decoder-only?

**Your Answer:**
> Transformer model is proposed by Google, using K,Q,V matrix and tries to utilize GPU to process token generation in parallel. It first using Encoder to encode the input into some intermediate presentation. Then use Decoder to translate the intermediate presentation into next token prediction possibilities. 
Encoder-only is mostly used for text translation.
Decoder-only is mostly used for today's llm is due to its eaiser architecture and better performance. 

**AI Feedback:**
*   ✅ **What's Right:** Correctly identified Google as the origin, mentioned K/Q/V matrices, and understood the GPU/parallelization benefit.
*   🛠 **Needs Improvement:** Significant misconception on "Encoder-only for translation" (Translation is Seq-to-Seq). Needs to clarify that modern LLMs are purely Decoder-only (no separate Encoder).

**Corrected & Refined:**
> The Transformer was introduced in "Attention is All You Need" (Google). While the original model was **Encoder-Decoder** (for translation), modern LLMs are **Decoder-only**.
> *   **Encoder-only:** (e.g., BERT) Used for classification and understanding, NOT translation.
> *   **Decoder-only:** (e.g., GPT) Dominates today because it is more efficient for **Auto-regressive** (token-by-token) generation and scales better for massive pre-training.

> [!CAUTION]
> **Significant Error:** You mentioned "Encoder-only is for translation." This is a common trap. Encoder-only models are for **Understanding** (Classification/NER). Translation is a **Seq-to-Seq** task and requires a generative component (Decoder).

---

### 2. Attention & Self-Attention
**Question:** 
What are Attention and Self-Attention mechanisms? What specific problems in sequence modeling (like RNNs/LSTMs) did they solve?

**Your Answer:**
> Attention is the predicted next token relies on some token in the past.
Self Attention is that when llm generate next token, it automatically knows which tokens in past is important, and move forward automatically

For sequence model, the problems is
1. can not run in parallel.
2. if the sentence is too long, it is hard to track the high impact token long long time ago. it experiences the attention vanishing or exploding.

**AI Feedback:**
*   ✅ **What's Right:** Perfect identification of the two core RNN problems (sequential bottleneck and vanishing/exploding gradients).
*   🛠 **Needs Improvement:** Your explanation of Self-Attention is a bit "magical" ("automatically knows"). In an interview, explain it as **mathematical weighted averages** (dot products) between all tokens in the sequence.

**Corrected & Refined:**
> **Attention** allows the model to focus on specific parts of the input. **Self-Attention** specifically relates every token in a sequence to every other token in that *same* sequence.
> *   **RNN Problems solved:** 
>     1. **Vanishing Gradients:** RNNs "forget" long-term info. Self-attention has $O(1)$ distance between any two tokens, preserving context perfectly.
>     2. **Parallelism:** RNNs are $O(N)$ sequential. Transformers use **Matrix Multiplication** to process all tokens at once ($O(1)$ time complexity per layer on hardware).

---

### 3. Tokens & Tokenization
**Question:** 
What are Tokens and Tokenization? Explain the difference between BPE (Byte Pair Encoding) and WordPiece algorithms.

**Your Answer:**
> Tokens is the representation of a human word, uses a vector with large dimensions, to represent the meansing of a single word.
Tokenizaton is the method to translate a human word into token. So token can be feed into llm as input.
I don't know BPE and WordPiece

**AI Feedback:**
*   ✅ **What's Right:** Understood that tokenization is the bridge between human text and model input.
*   🛠 **Needs Improvement:** Fundamental confusion between **Token** and **Embedding**. You also need to learn the sub-word merging logic (BPE) which is critical for handling new words.

**Corrected & Refined:**
> **Tokenization** is the process of splitting text into small units (**Tokens**). 
> *   **Correction:** A Token is a **discrete integer ID**. The "vector with large dimensions" is the **Embedding**, which the model learns to represent the token's meaning.
> *   **BPE (Byte Pair Encoding):** Merges the most frequent *pairs* of characters/tokens (GPT style).
> *   **WordPiece:** Merges units that maximize the *likelihood* of the language model (BERT style).

> [!WARNING]
> **Terminology Error:** Never call a Token a "vector" in an interview. A Token is an **index** (ID). The **Embedding** is the vector. Confusing these shows a lack of familiarity with the model's architecture layers.

---

## 🤖 AI Assistant Review & Deep-Dive Reference

To truly master these for an interview, you should be able to explain the "mechanics" behind the concepts.

### 1. Transformer Architecture: Why Decoder-Only?
*   **The Original (Encoder-Decoder):** Designed for Sequence-to-Sequence tasks (Translation). The Encoder builds a global representation, and the Decoder uses **Cross-Attention** to look at that representation while generating.
*   **Decoder-Only (The GPT approach):**
    *   **Causal Masking:** Decoders use a "mask" in the self-attention layer to ensure that when predicting token $t$, the model can only "see" tokens $1$ to $t-1$. It cannot peek at the future.
    *   **Pre-training Efficiency:** It is much easier to train a decoder-only model on raw, unlabeled text (Next Token Prediction). Since there is no "input" sequence to encode, the model learns features directly from the distribution of language.
*   **Key Components:** Every Transformer block consists of **Multi-Head Self-Attention**, **Layer Normalization**, **Residual Connections**, and a **Position-wise Feed-Forward Network (FFN)**.

### 2. Attention Mechanism: The Math & Intuition
*   **Scaled Dot-Product Attention Formula:**
    $$Attention(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
    *   **$Q$ (Query):** What I'm looking for.
    *   **$K$ (Key):** What information I have.
    *   **$V$ (Value):** The actual content.
    *   **Scaling ($\frac{1}{\sqrt{d_k}}$):** Prevents the dot product from growing too large, which would push the softmax into regions with extremely small gradients (the "gradient vanishing" problem during training).
*   **Solving RNN Problems:** 
    *   **RNNs:** Sequential $O(N)$ complexity. Information must pass through $N$ hidden states to reach the end.
    *   **Transformers:** $O(1)$ path length between any two tokens. This allows for massive **Parallelism** on GPUs and eliminates the distance-based "forgetting" issue.

### 3. Tokenization: BPE vs. WordPiece
*   **BPE (Byte Pair Encoding) - Frequency Based:**
    1.  Start with individual characters.
    2.  Count all adjacent pairs (e.g., "e" and "r").
    3.  Merge the most frequent pair into a new token ("er").
    4.  Repeat until the desired vocabulary size is reached.
    *   *Used by:* GPT-2/3/4, Llama, RoBERTa.
*   **WordPiece - Likelihood Based:**
    *   Instead of just frequency, it calculates which merge would **maximize the likelihood** of the training data. It asks: "If I merge $A$ and $B$, does it make the whole dataset more predictable?"
    *   *Used by:* BERT, DistilBERT.
*   **Crucial Concept:** Both allow the model to handle "Out of Vocabulary" words by breaking them into meaningful sub-words (e.g., "unfriendly" -> "un" + "friendly").

---
*Next Step: Day 2 - LLM Context, KV Cache, and Long-Context Techniques.*


