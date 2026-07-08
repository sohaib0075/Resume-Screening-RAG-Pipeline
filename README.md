# Resume Screening RAG Pipeline

## Introduction

This research is part of the author's graduating thesis, which aims to present a proof of concept (POC) of an LLM chatbot that can assist hiring managers in the resume screening process. The assistant is a cost-efficient, user-friendly, and more effective alternative to conventional keyword-based screening methods.

Powered by state-of-the-art LLMs, it can handle unstructured and complex natural language data in job descriptions and resumes, while performing high-level tasks as effectively as a human recruiter.

The core design of the assistant involves hybrid retrieval methods to augment the LLM agent with suitable resumes as context:

- **Adaptive Retrieval**
  - *Similarity-based retrieval*: When a job description is provided, the retriever uses RAG/RAG Fusion to search for similar resumes and narrow the pool of applicants to the most relevant profiles.
  - *Keyword-based retrieval*: When applicant information is provided (IDs), the retriever can also retrieve additional information about specified candidates.
- **Generation**: The retrieved resumes are used to augment the LLM generator so it is conditioned on the data of the retrieved applicants. The generator can then be used for downstream tasks like cross-comparisons, analysis, summarization, or decision-making.

---

## Why Resume Screening?

Despite the increasingly large volume of applicants each year, there are limited tools that can assist the screening process effectively and reliably. Existing methods often rely on keyword-based approaches, which cannot accurately handle the complexity of natural language in human-written documents. This creates a clear opportunity to integrate LLM-based methods into this domain — which this project aims to address.

## Why RAG / RAG Fusion?

RAG-like frameworks are effective tools for enhancing the reliability of chatbots. RAG provides an external knowledge base for LLM agents, giving them additional context relevant to user queries. This increases the relevance and accuracy of generated answers, which is especially important in data-intensive environments such as recruitment.

RAG Fusion, in particular, is effective in addressing complex and ambiguous human-written prompts. While the LLM generator can handle this problem well, the retriever may struggle to find relevant documents when presented with multifaceted queries. This technique improves resume retrieval quality when the system receives complex job descriptions — which are common in hiring.

> **Note:** For more info, please refer to the paper: [Google Drive](#)

---

## Demo

- **Chatbot demo interface:** [Streamlit](#)
- **Default synthetic resume dataset used in the demo:** [GitHub](#)
- **Source job description dataset:** [Kaggle](#)

> **⚠️ Warning:** The file uploader is still quite unstable in the Streamlit deployment. Use of the uploader is not recommended.

### Screenshots

**Starting screen**

![Starting screen](assets/starting_screen.png)

**Example response when receiving a job description**

![Job description response](assets/job_description_response.png)

**Example response when receiving specific applicant IDs**

![Applicant ID response](assets/applicant_id_response.png)

---

## System Description

### 1. Chatbot Structure

![Chatbot structure](assets/chatbot_structure.png)

The deployed chatbot uses the following techniques to be more suitable for real-world use cases:

- **Chat history access:** The LLM is fed the entire conversation and the latest retrieved documents for every message, allowing it to perform follow-up tasks.
- **Query classification:** Using function-calling and an adaptive approach, the LLM extracts the necessary information to decide whether to toggle the retrieval process on or off. The system only performs document retrieval when a suitable input query is provided; otherwise, it relies on chat history alone.
- **Small-to-Big retrieval:** Retrieval is performed using text chunks for efficiency. Retrieved chunks are then traced back to their original full-text documents to augment the LLM generator, giving it the complete context of the resumes.

**Tech stack:**

- `langchain`, `openai`, `huggingface` — RAG pipeline and chatbot construction
- `faiss` — Vector indexing and similarity retrieval
- `streamlit` — User interface development

### 2. Under-the-Hood: RAG Pipeline

![RAG pipeline diagram](assets/rag_pipeline_diagram.png)

**Encoder (1)**

The pipeline begins by processing resumes into a vector storage. Upon receiving an input job description query, the LLM agent is prompted to generate sub-queries. The vector storage then performs a retrieval process for each sub-query, returning the top-K most similar documents. The document lists for each sub-query are combined and re-ranked into a new list representing the documents most similar to the original job description.

The LLM then uses the retrieved applicants' information as context to form accurate, relevant, and informative responses that assist hiring managers in matching resumes with job descriptions.

---

## Installation and Setup

To set up the project locally:

1. **Clone the project**

   ```bash
   git clone https://github.com/Hungreeee/Resume-Screening-RAG-Pipeline.git
   ```

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Run the Streamlit demo locally**

   ```bash
   streamlit run demo/interface.py
   ```

---

## Contributions

The design of the demo chatbot is intentionally simple, as it only serves to demonstrate the potential of RAG-like systems in the recruitment domain. The system is still very much a work in progress, and any suggestions, feedback, or contributions are highly appreciated! Please share them via [Issues](#).

## Acknowledgements

Inspired by RAG Fusion.
