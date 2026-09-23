# Day 18 — LLM Hallucination Measurement

## 📌 Project Overview

This project investigates why Large Language Models (LLMs) can produce hallucinated or unsupported answers and demonstrates simple methods for measuring potential hallucinations.

The experiment compares LLM answers against known ground-truth answers and applies a 4-signal heuristic detector.

---

## 🎯 Objectives

- Create a factual question dataset across multiple domains
- Generate answers using an LLM
- Compare generated answers with ground-truth answers
- Classify responses as:
  - Correct
  - Partially Correct
  - Hallucinated
  - API Error
- Detect potentially unsupported responses using multiple signals
- Analyze the limitations of automated hallucination detection

---

## 📚 Dataset

The experiment contains **20 factual questions** from four domains:

- Science
- Technology
- General Knowledge
- History

Each question contains a known ground-truth answer.

---

## 🤖 LLM Used

**Google Gemini**

Model used in the experiment:

`gemini-3.6-flash`

The experiment was implemented in **Google Colab using Python**.

---

## 🔍 Baseline Evaluation

The LLM was asked to answer factual questions without additional retrieved context.

### Results

| Metric | Result |
|---|---:|
| Total Questions | 20 |
| Valid Responses | 16 |
| Correct | 15 |
| Partially Correct | 1 |
| Explicitly Hallucinated | 0 |
| API Errors | 4 |
| Baseline Hallucination Rate | 0.00% |

API errors were excluded from the hallucination calculation because they represent generation failures rather than hallucinated content.

---

## 🧪 4-Signal Hallucination Detector

A simple heuristic detector was implemented using four signals:

### 1. Unsupported Claim Detection

Checks whether the generated answer has sufficiently high lexical overlap with the retrieved context.

### 2. Retrieval Overlap

Jaccard similarity was used to measure word overlap between the generated answer and retrieved context.

### 3. Contradiction Detection

A simple rule-based check was used to detect numerical contradictions between the ground truth and generated answer.

### 4. Citation Detection

The system checks whether the answer contains common citation or source indicators such as:

- Source
- Reference
- HTTP/HTTPS links
- `[1]`
- `[2]`

---

## 📊 Detector Results

| Detector Metric | Result |
|---|---:|
| Supported | 10 |
| Potential Hallucinations | 6 |
| Contradictions Detected | 0 |
| Citations Present | 0 |
| API Errors | 4 |

**Important:** Potential hallucinations detected by this heuristic are not automatically confirmed hallucinations. Short but correct answers can have low lexical overlap with retrieved context.

---

## 🔎 RAG Experiment

A small TF-IDF based retrieval system was created to provide relevant context to the LLM.

The RAG experiment could not be completed for all 20 questions because the Gemini API returned quota/rate-limit errors during generation.

Therefore, a complete RAG hallucination rate was **not calculated**.

No fabricated RAG comparison was used.

---

## 🛠️ Technologies Used

- Python
- Google Colab
- Google Gemini API
- Pandas
- Scikit-learn
- TF-IDF
- Cosine Similarity
- Jaccard Similarity
- Regular Expressions

---

## 📁 Project Output

The project generates:

`Day_18_Hallucination_Final_Report.csv`

The report contains:

- Question
- Domain
- Ground Truth
- LLM Answer
- Evaluation
- Jaccard Overlap
- Unsupported Claim Signal
- Contradiction Signal
- Citation Signal
- Final Detector Status

---

## ⚠️ Limitations

This experiment uses a small 20-question factual dataset.

The hallucination detector is a heuristic system and should not be considered a definitive fact-checking system.

The RAG comparison was incomplete because of Gemini API quota/rate-limit errors.

Therefore, the reported baseline results describe this specific experiment and should not be generalized to all LLMs.

---

## 🏁 Conclusion

This project demonstrates a practical approach to studying LLM hallucinations.

The experiment shows that automated hallucination detection can combine multiple signals such as retrieval overlap, unsupported claims, contradictions, and citation presence.

It also demonstrates an important limitation: a simple lexical-overlap detector can flag correct short answers as potential hallucinations.

The project therefore treats detector output as a **potential hallucination signal rather than definitive proof of hallucination**.
