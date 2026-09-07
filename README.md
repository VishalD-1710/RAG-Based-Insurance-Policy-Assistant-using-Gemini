# RAG-Based-Insurance-Policy-Assistant-using-Gemini
# RAG-Based Insurance Policy Assistant using Gemini

A Retrieval-Augmented Generation (RAG) chatbot that answers insurance-policy questions using a supplied `knowledge_base.json` dataset.

The project is implemented in **Google Colab** and combines semantic embeddings, FAISS vector retrieval, Gemini generation, and a Gradio chatbot interface.

---

## 📌 Project Overview

Insurance policy documents contain large amounts of information about coverage, exclusions, waiting periods, claims, premiums, renewals, and other policy conditions.

Finding the correct information using traditional keyword-based search can be difficult. This project solves the problem using a **Retrieval-Augmented Generation (RAG)** approach.

The system first retrieves relevant information from the insurance knowledge base and then provides the retrieved context to Gemini. Gemini generates a concise answer based on the retrieved policy information.

### Main Pipeline

```text
Insurance Knowledge Base
        ↓
Data Loading
        ↓
Text Cleaning & Preprocessing
        ↓
Document Chunking
        ↓
Sentence Embeddings
        ↓
FAISS Vector Index
        ↓
Top-5 Relevant Chunks
        ↓
Gemini Generation
        ↓
Grounded Answer
        ↓
Gradio Chatbot
```

---

## 🎯 Project Objectives

The main objectives of this project are:

* Load insurance policy information from a JSON knowledge base.
* Preprocess and clean the policy text.
* Divide documents into smaller overlapping chunks.
* Generate semantic embeddings for the chunks.
* Store embeddings using FAISS.
* Retrieve the most relevant policy information for a user query.
* Use Gemini to generate a grounded answer.
* Provide an interactive chatbot using Gradio.
* Evaluate the retrieval system using predefined questions.

---

## 🏗️ System Architecture

The project follows the architecture shown below:

![RAG Architecture](architecture_diagram.png)

### Architecture Components

1. **Knowledge Base**

   * Input dataset: `knowledge_base.json`
   * Contains insurance policy documents.

2. **Preprocessing**

   * Removes HTML-like tags.
   * Normalizes whitespace.
   * Cleans the policy content.

3. **Chunking**

   * Maximum chunk size: **220 words**
   * Chunk overlap: **40 words**
   * Overlapping chunks help preserve context between neighboring sections.

4. **Embedding Generation**

   * Model: `all-MiniLM-L6-v2`
   * Converts policy chunks into numerical vectors.
   * Embeddings are normalized before indexing.

5. **FAISS Retrieval**

   * Uses `IndexFlatIP`.
   * Performs similarity search using normalized embeddings.
   * Retrieves the **top 5** relevant chunks.

6. **Gemini Generation**

   * Receives the retrieved policy context.
   * Generates a concise answer.
   * Is instructed not to invent unsupported information.

7. **Gradio Chatbot**

   * Provides an interactive question-answering interface.
   * Displays the generated answer, source document IDs, and latency.

---

## 🛠️ Technologies Used

| Technology            | Purpose                               |
| --------------------- | ------------------------------------- |
| Python                | Main programming language             |
| Google Colab          | Development and execution environment |
| Google GenAI          | Gemini API integration                |
| Gemini                | Answer generation                     |
| Sentence Transformers | Text embeddings                       |
| `all-MiniLM-L6-v2`    | Embedding model                       |
| FAISS                 | Vector similarity search              |
| NumPy                 | Numerical operations                  |
| Pandas                | Data processing                       |
| Gradio                | Chatbot interface                     |
| scikit-learn          | Evaluation utilities                  |

---

## 📂 Project Structure

```text
RAG-Insurance-Policy-Assistant/
│
├── RAG_Insurance_Assistant_Gemini_REQUIRED_ONLY.ipynb
│
├── knowledge_base.json
│
├── architecture_diagram.png
│
├── project_report_7_pages.pdf
│
├── README.md
│
└── .gitignore
```

---

## 📊 Dataset

The project uses:

```text
knowledge_base.json
```

The dataset contains insurance policy information used as the retrieval source.

The notebook supports JSON data organized either as a list or through common wrapper keys such as:

```text
data
documents
knowledge_base
```

Each document is normalized into fields such as:

```text
document ID
category
title
content
```

---

## 🔄 Data Preprocessing

Before generating embeddings, the policy documents are cleaned.

The preprocessing pipeline includes:

### HTML Cleaning

HTML-like tags are removed from the document text.

### Whitespace Normalization

Repeated whitespace and unnecessary spaces are removed.

### Text Chunking

Documents are divided into smaller chunks.

```text
Maximum chunk size = 220 words
Overlap = 40 words
```

The overlap ensures that information near the boundary of two chunks is not completely separated.

---

## 🧠 Embedding Generation

The project uses:

```text
all-MiniLM-L6-v2
```

from Sentence Transformers.

The embedding model converts each policy chunk into a numerical vector.

For example:

```text
Policy Text
     ↓
Sentence Transformer
     ↓
Numerical Embedding
```

The embeddings are normalized before being inserted into the FAISS index.

The same embedding model is used for both:

* policy chunks
* user queries

This allows the query vector to be compared with the stored policy vectors.

---

## 🔎 FAISS Retrieval

FAISS is used as the vector search engine.

The project uses:

```text
IndexFlatIP
```

The normalized vectors allow inner-product similarity to be used for ranking relevant chunks.

For every user question, the system retrieves:

```text
Top K = 5
```

relevant policy chunks.

The retrieved results contain information such as:

```text
Document ID
Category
Title
Chunk Text
Similarity Score
```

---

## 🤖 Gemini Generation

After retrieval, the relevant policy chunks are provided to Gemini as context.

The generation process is:

```text
User Question
      ↓
Query Embedding
      ↓
FAISS Search
      ↓
Top-5 Policy Chunks
      ↓
Retrieved Context
      ↓
Gemini
      ↓
Final Answer
```

The Gemini prompt instructs the model to:

* Answer only using the retrieved policy context.
* Avoid unsupported facts.
* Avoid hallucinating information.
* Clearly state when the retrieved context is insufficient.
* Explain policies in simple language.
* Avoid inventing premium amounts.
* Avoid inventing claim outcomes.
* Provide relevant document IDs.

This grounding approach helps keep answers connected to the supplied insurance policy data.

---

## 💬 Chatbot Interface

The project uses **Gradio ChatInterface** to provide the chatbot.

The interface allows users to ask natural-language questions about insurance policies.

Example questions include:

```text
What is covered under hospitalization?
```

```text
How do I file a cashless claim?
```

```text
What is the waiting period for pre-existing diseases?
```

```text
What factors determine premium?
```

```text
What is the free look period?
```

The chatbot displays:

* Generated answer
* Retrieved document IDs
* Response latency

---

## ⚙️ Gemini Configuration

The notebook uses the Gemini generation configuration defined in the project.

| Parameter             | Value   |
| --------------------- | ------- |
| Temperature           | `0.2`   |
| Maximum output tokens | `500`   |
| Retrieved chunks      | `Top 5` |

The low temperature is intended to make the generated answers more consistent and less creative.

---

## 📈 Evaluation

The project includes a predefined evaluation set containing **20 insurance-related questions**.

The evaluation covers topics such as:

* Hospitalization
* Pre-hospitalization expenses
* Post-hospitalization expenses
* Day-care procedures
* Ambulance charges
* Pre-existing disease waiting periods
* Initial waiting periods
* Permanent exclusions
* Cashless claims
* Reimbursement claims
* Claim settlement
* Claim rejection
* Premium calculation
* Renewal grace period
* No Claim Bonus
* Portability
* Maternity benefits
* Critical illness rider
* Network hospitals
* Free-look period

---

## 📊 Evaluation Metrics

### Hit@1

Measures whether the expected document is ranked first.

```text
Hit@1 =
Number of correct top-1 retrievals
----------------------------------
Total number of questions
```

### Recall@5

Measures whether the expected document appears anywhere within the top five retrieved results.

```text
Recall@5 =
Questions where expected document is in Top-5
----------------------------------------------
Total number of questions
```

### Rank

The position of the expected document in the retrieved results.

### Mean Retrieval Latency

Measures the average time required to perform retrieval.

Latency is reported in milliseconds.

---

## ▶️ How to Run the Project

### Step 1 — Open Google Colab

Open the notebook:

```text
RAG_Insurance_Assistant_Gemini_REQUIRED_ONLY.ipynb
```

in Google Colab.

### Step 2 — Install Dependencies

The notebook installs the required Python packages, including:

```text
google-genai
faiss-cpu
sentence-transformers
gradio
pandas
numpy
scikit-learn
```

### Step 3 — Provide Gemini API Key

The notebook requests the Gemini API key interactively.

Do not hard-code the API key.

### Step 4 — Upload Dataset

Upload:

```text
knowledge_base.json
```

when requested by the notebook.

### Step 5 — Execute the Notebook

Run the notebook cells in order:

```text
1. Import libraries
2. Configure Gemini
3. Load knowledge base
4. Preprocess documents
5. Create chunks
6. Generate embeddings
7. Build FAISS index
8. Test retrieval
9. Generate Gemini responses
10. Run evaluation
11. Launch Gradio chatbot
```

### Step 6 — Use the Chatbot

Once the Gradio interface launches, enter insurance-policy questions and review the grounded responses.

---

## 🔐 Security

**Never upload API keys or other secrets to GitHub.**

Do not commit files containing:

```text
GEMINI_API_KEY
GOOGLE_API_KEY
.env
API tokens
Passwords
Private credentials
```

The Gemini API key should be entered securely at runtime.

---

## ⚠️ Project Scope

This project intentionally focuses on the required RAG components:

```text
Data Loading
      ↓
Preprocessing
      ↓
Chunking
      ↓
Embeddings
      ↓
FAISS Retrieval
      ↓
Gemini Generation
      ↓
Chatbot
      ↓
Evaluation
```

The project does **not** implement optional advanced components such as:

* BM25 hybrid retrieval
* Cross-encoder reranking
* Agentic tools
* PII/guardrail modules
* Conversational memory
* Observability and cost monitoring

These components are outside the required project scope.

---

## ⚠️ Limitations

The current implementation has several limitations:

1. Retrieval uses dense semantic similarity rather than hybrid retrieval.
2. No cross-encoder reranking is implemented.
3. The chatbot does not maintain dedicated conversational memory.
4. There is no production monitoring system.
5. The application is demonstrated through Google Colab and Gradio.
6. Retrieval evaluation does not represent a complete human evaluation of answer quality.
7. Performance depends on the quality and coverage of the supplied knowledge base.

---

## 🚀 Future Enhancements

Future versions could include:

* Hybrid BM25 + vector retrieval.
* Cross-encoder reranking.
* Larger evaluation datasets.
* Human evaluation of generated answers.
* Automated answer-quality metrics.
* Persistent FAISS indexes.
* Production deployment.
* Authentication and access control.
* Monitoring and logging.
* Cost tracking.
* Improved conversational memory.

---

## 📄 Project Report

A detailed **7-page project report** is included in the repository:

```text
project_report_7_pages.pdf
```

The report covers:

* Introduction
* Problem definition
* Objectives
* System architecture
* Implementation
* RAG pipeline
* Gemini generation
* Chatbot
* Evaluation
* Limitations
* Future scope
* Conclusion

---

## 📌 Example Workflow

```text
User:
"What is the waiting period for pre-existing diseases?"

              ↓

Query Embedding

              ↓

FAISS Similarity Search

              ↓

Top 5 Relevant Policy Chunks

              ↓

Retrieved Policy Context

              ↓

Gemini

              ↓

Grounded Insurance Answer

              ↓

Source Document IDs + Latency
```

---

## 📚 Conclusion

This project demonstrates how Retrieval-Augmented Generation can be applied to insurance policy question answering.

By combining:

```text
Sentence Transformers
        +
FAISS
        +
Gemini
        +
Gradio
```

the system can retrieve relevant insurance policy information and transform it into understandable natural-language responses.

The separation between **retrieval** and **generation** provides a clear architecture in which FAISS supplies relevant evidence and Gemini explains that evidence while following grounding instructions.

---

## 👨‍💻 Project Files[RAG_Insurance_Assistant_Gemini_Colab_REQUIRED_ONLY (1).ipynb](https://github.com/user-attachments/files/31919620/RAG_Insurance_Assistant_Gemini_Colab_REQUIRED_ONLY.1.ipynb)
<img width="2260" height="1263" alt="architecture_diagram" src="https://github.com/user-attachments/assets/87e4f54c-e09f-4dac-9b83-ed008e7c940f" />
[project_report_7_pages.pdf](https://github.com/user-attachments/files/31919617/project_report_7_pages.pdf)

RAG_Insurance_Assistant_Gemini_REQUIRED_ONLY.ipynb
knowledge_base.json
architecture_diagram.png
project_report_7_pages.pdf

