# Learning Workflow: AI Engineer Interview Prep 🎓

This document defines the standard procedure for our daily learning sessions. 

## 🔄 Daily Cycle

### 1. Preparation (AI)
- Create a new file `dayX.md` based on the `plan.md` curriculum.
- Present 3 technical questions.
- Include a "Your Answer" placeholder for the user.

### 2. Drafting (User)
- The user fills in the "Your Answer" sections with their current understanding.

### 3. Correction & Refinement (AI)
- **AI Feedback:** Provide a summary using ✅ **What's Right** and 🛠 **Needs Improvement** to specifically critique the user's answer.
- **Insert "Corrected & Refined" blocks:** Directly under the feedback.
- **Highlight Significant Errors:** Use GitHub Alerts (`[!CAUTION]` or `[!WARNING]`) to point out critical misconceptions.

### 4. Deep-Dive (AI)
- **Append "Deep-Dive Reference" section:** At the bottom of the file.
- **Content:** 
    - Mathematical formulas (Attention, Softmax, etc.).
    - Architectural diagrams/descriptions (Masking, FFN, LayerNorm).
    - Hardware/Efficiency considerations (GPU Parallelism, Memory Bandwidth).
    - Algorithm details (BPE merging steps, WordPiece likelihood).

## 📂 File Structure Example
```markdown
### 1. Question Title
**Question:** [The Question Text]
**Your Answer:** [User's input]
**Corrected & Refined:** [AI's terminology correction]

---
## 🤖 AI Assistant Review & Deep-Dive Reference
[High-level technical details]
```
