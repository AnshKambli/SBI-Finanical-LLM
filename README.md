# SBI Financial Report Intelligence System

An **LLM-powered financial document intelligence system** that uses Retrieval-Augmented Generation (RAG) to extract, retrieve, validate, and answer questions from the **State Bank of India's FY2023–24 Annual Report**.

The system combines **document processing, financial data extraction, semantic search, vector retrieval, and LLM-based question answering** to provide evidence-grounded responses while refusing questions when sufficient supporting evidence is unavailable.

---

## 🚀 Project Overview

Annual reports contain hundreds of pages of financial statements, tables, ratios, disclosures, and business information. Finding a specific financial metric manually can be slow and difficult.

This project builds an intelligent question-answering pipeline that allows users to ask questions such as:

> **"What was the Gross NPA percentage for Agriculture & allied activities in the Priority Sector in FY2024?"**

The system retrieves relevant information from the annual report and uses **Phi-3** to generate a grounded response.

### Example

**Question**

```text
What was the Gross NPA percentage for Agriculture & allied
activities in the Priority Sector in FY2024?
```

**System Response**

```text
The Gross NPA percentage for Agriculture & allied activities
in the Priority Sector in FY2024 was 9.64%.

Source: PDF page 204
```

The system can also refuse unsupported questions instead of generating an answer without sufficient evidence.

---

# 🧠 System Architecture

```text
                 SBI Annual Report 2023–24
                         │
                         ▼
                  PDF Processing
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
       Text Extraction        Table Extraction
                                     │
                                     ▼
                              Table Normalization
                                     │
                                     ▼
                           Financial Data Extraction
                                     │
                                     ▼
                              Retrieval Chunks
                                     │
                                     ▼
                         Sentence Transformers
                         all-MiniLM-L6-v2
                                     │
                                     ▼
                                FAISS Index
                                     │
                                     ▼
                              Relevant Context
                                     │
                                     ▼
                                  Phi-3
                                     │
                         ┌───────────┴───────────┐
                         ▼                       ▼
                   Grounded Answer          Refusal
                         │
                         ▼
                   Number Validation
                         │
                         ▼
                    Final Response
```

---

# 🔍 How It Works

## 1. Annual Report Processing

The system processes the SBI FY2023–24 Annual Report containing **486 pages**.

The document contains financial statements, tables, business information, asset-quality metrics, and other disclosures.

---

## 2. Table Extraction

The PDF processing pipeline extracted:

* **478 raw tables**
* **478 normalized tables**

The extracted tables are converted into a structured representation that can be processed programmatically.

---

## 3. Financial Data Extraction

Important financial information is converted into structured financial records.

Current V1 extraction contains:

* **74 financial records**

These records form the structured financial knowledge layer used by the question-answering system.

---

## 4. Retrieval Chunk Creation

Relevant information is transformed into smaller retrieval units.

Current V1 contains:

* **85 retrieval chunks**

Chunking allows the system to search for specific information rather than sending the complete annual report to the LLM.

---

## 5. Embeddings

Each retrieval chunk is converted into a numerical vector using:

```text
all-MiniLM-L6-v2
```

The embeddings represent the semantic meaning of the financial information.

This allows questions to be matched with relevant content based on meaning rather than exact keyword matching.

---

## 6. Vector Search with FAISS

The generated embeddings are stored in a **FAISS vector index**.

Current index:

```text
Retrieval chunks: 85
Vectors:          85
```

When a user asks a question:

```text
Question
   ↓
Question Embedding
   ↓
FAISS Similarity Search
   ↓
Relevant Financial Context
```

The retrieved context is then passed to Phi-3.

---

## 7. LLM-Based Answer Generation

The retrieved evidence is provided to **Phi-3**, which generates the final response.

The model is instructed to rely on retrieved evidence rather than inventing information.

For example:

```text
Question
   +
Retrieved financial evidence
   ↓
Phi-3
   ↓
Grounded answer
```

---

## 8. Evidence-Based Refusal

The system includes a refusal mechanism.

If sufficient evidence cannot be found in the available financial data, the system does not attempt to guess the answer.

Example:

```text
Question:
What were SBI's total assets as of March 31, 2024?

Response:
I couldn't find sufficient evidence for that in the
extracted financial data.
```

This is intentional.

The goal is to prioritize **grounded responses over hallucinated responses**.

---

# 📊 Current Results

| Component                    | Result |
| ---------------------------- | -----: |
| Annual Report Pages          |    486 |
| Raw Tables Extracted         |    478 |
| Normalized Tables            |    478 |
| Structured Financial Records |     74 |
| Retrieval Chunks             |     85 |
| Vectors                      |     85 |
| Validation Pass Rate         |   100% |
| Evaluation Answer Accuracy   |   100% |
| Evaluation Refusal Accuracy  |   100% |

> **Note:** Accuracy results are based on the project's defined evaluation dataset and should not be interpreted as a guarantee of 100% accuracy across the entire annual report.

---

# 🧪 Example Questions

The system has been tested with questions including:

### Asset Quality

```text
What was the Gross NPA percentage for Agriculture & allied
activities in the Priority Sector in FY2024?
```

### Year-over-Year Analysis

```text
How did Agriculture & allied activities in the Priority
Sector perform between FY2023 and FY2024?
```

### Sector Comparison

```text
Which sector had the highest Gross NPA ratio in FY2024?
```

### Unsupported Information

```text
What were SBI's total assets as of March 31, 2024?
```

When the required information is not present in the current extracted financial knowledge base, the system refuses to answer rather than fabricate a value.

---

# 🛠️ Tech Stack

### LLM

* **Phi-3**

### Embeddings

* **Sentence Transformers**
* **all-MiniLM-L6-v2**

### Vector Database / Search

* **FAISS**

### Data Processing

* **Python**
* **Pandas**
* PDF and table extraction tools

### Development Environment

* **Jupyter Notebook**

---

# 📁 Project Structure

```text
SBI-Financial-Report-Intelligence/
│
├── SBI_Financial_Report_Intelligence.ipynb
├── README.md
└── .gitignore
```

The main implementation is contained in the Jupyter Notebook.

---

# ⚙️ Pipeline

The complete pipeline can be summarized as:

```python
PDF
 ↓
Extract text and tables
 ↓
Normalize tables
 ↓
Extract financial records
 ↓
Create retrieval chunks
 ↓
Generate embeddings
 ↓
Build FAISS index
 ↓
Retrieve relevant evidence
 ↓
Pass context to Phi-3
 ↓
Generate answer
 ↓
Validate numerical response
 ↓
Return answer or refusal
```

---

# 🎯 Key Learning Outcomes

This project demonstrates practical experience with:

* Large Language Model integration
* Retrieval-Augmented Generation (RAG)
* Semantic search
* Text embeddings
* Vector databases
* FAISS similarity search
* Financial document processing
* PDF and table extraction
* Structured financial data extraction
* Prompt-based grounded generation
* Numerical answer validation
* LLM evaluation
* Hallucination/refusal handling

---

# 🔮 Future Improvements

Planned improvements include:

* Expand financial record extraction across the complete annual report
* Improve table understanding for complex financial statements
* Increase retrieval coverage
* Add page-level citations for retrieved evidence
* Improve financial calculation validation
* Add automated evaluation datasets
* Test retrieval precision and recall
* Support multiple annual reports
* Compare financial performance across years
* Extend the system to multiple listed companies
* Add structured financial analytics on top of the RAG system

---

# 📌 Why This Project?

Financial reports contain valuable information but are difficult to search and analyze manually.

This project explores how **LLMs + RAG + structured financial data + vector search** can transform long financial documents into an interactive analytical knowledge system.

The focus is not simply on generating text, but on building a pipeline that:

```text
Extracts → Structures → Retrieves → Validates → Answers
```

while reducing unsupported responses.

---

# 👨‍💻 Author

**Ansh Kambli**

Data Analyst | Business Intelligence | Data Science | AI/LLM

GitHub: [AnshKambli](https://github.com/AnshKambli)

---

## ⭐ Project Status

**V1 — Working**

The core LLM/RAG pipeline, retrieval system, financial extraction workflow, answer validation, and refusal mechanism are implemented and evaluated.

Future versions will expand financial-data coverage and retrieval capabilities across the complete annual report.
