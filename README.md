Here is the comprehensive project description with Markdown formatting.

# **LLM-Powered Legal Contract Analysis Pipeline**

This project provides a comprehensive, end-to-end pipeline for the intelligent analysis of legal contracts using Large Language Models (LLMs). [cite_start]The system is designed to automate the traditionally manual and time-consuming process of contract review [cite: 4][cite_start], offering capabilities for clause extraction [cite: 18][cite_start], summarization[cite: 23], and advanced semantic analysis. [cite_start]The entire solution is implemented in the `submission.ipynb` Jupyter Notebook[cite: 30], using Google's Gemini model as its core analytical engine.

***

### **System Architecture and Workflow**

The pipeline is structured into a series of modular components, ensuring a logical and efficient flow from data ingestion to final output.

#### **1. Data Acquisition and Preprocessing**

The foundation of the pipeline is its ability to handle raw contract documents.

* [cite_start]**Data Source**: The system utilizes the **Contract Understanding Atticus Dataset (CUAD)** [cite: 7][cite_start], a collection of over 13,000 annotated legal clauses from 510 contracts[cite: 10]. [cite_start]For this project, a subset of 50 contracts is processed as required by the assignment[cite: 11]. The dataset is downloaded directly using the `kagglehub` library.
* [cite_start]**PDF Text Extraction**: The pipeline ingests contracts in their original PDF format[cite: 15]. It uses the `PyPDF2` library to extract the full text from each document. The code is built to recursively search through the dataset's subdirectories to locate all contract files efficiently.
* [cite_start]**Text Normalization**: To prepare the data for the LLM, the extracted text undergoes a normalization process[cite: 16]. This step involves using regular expressions to remove excess whitespace and non-standard characters, ensuring the text is clean and optimized for machine analysis.

***

#### **2. The `ContractProcessor` Core Engine**

The central logic is managed by the `ContractProcessor` class, which orchestrates all AI-driven tasks.

* **Dual-Model Integration**: The processor employs a dual-model strategy:
    * **Google Gemini**: Used for all generative and analytical tasks, such as clause extraction and summarization. [cite_start]The assignment allows for the use of any API-based or open-source LLM[cite: 19].
    * **Sentence-Transformers**: The `all-MiniLM-L6-v2` model is used to generate dense vector embeddings for text, enabling semantic understanding and similarity searches.
* **Batch Processing**: The system is designed to handle multiple contracts in a single run. It iterates through the loaded contracts, applies the full analysis pipeline to each, and provides real-time progress updates.

***

#### **3. Prompt Engineering Strategy**

[cite_start]The accuracy and quality of the output depend heavily on the design of the prompts sent to the LLM[cite: 43].

* **Clause Extraction Prompt**: To extract key clauses, the Gemini model is assigned the persona of a "legal document analyzer". [cite_start]It is specifically instructed to identify and return the exact text for three clause types: **Termination Conditions** [cite: 20][cite_start], **Confidentiality Clauses** [cite: 21][cite_start], and **Liability Clauses**[cite: 22]. The prompt strictly requires the output to be formatted as a JSON object, which ensures the data is structured, consistent, and easily parsed by the program. If a clause is not found, the model is instructed to state "Not found".
* **Summarization Prompt**: For contract summaries, the model is tasked with acting as a "legal document summarizer". [cite_start]The prompt mandates a concise 100-150 word summary [cite: 24] [cite_start]that must cover three key areas: the **purpose of the agreement** [cite: 25][cite_start], the **key obligations of each party** [cite: 26][cite_start], and any **notable risks or penalties**[cite: 27].

***

#### **4. Output Generation**

Once the analysis is complete, the results are compiled and stored for downstream use.

* The analysis for each contract—including the ID, summary, and each extracted clause—is organized into a **Pandas DataFrame** for easy viewing and manipulation.
* [cite_start]The final, structured results are saved in two formats: a **CSV file** and a **JSON file**, as specified in the deliverables[cite: 31].

***

### **Fulfillment of Deliverables**

This project successfully meets all specified deliverables as outlined in the assignment.

* [cite_start]**Code**: The complete, runnable Python code is provided in the `submission.ipynb` notebook, which implements the entire document processing pipeline[cite: 30].
* [cite_start]**Output File**: The notebook generates both CSV and JSON files with the specified columns: `contract_id`, `summary`, `termination_clause`, `confidentiality_clause`, and `liability_clause`[cite: 31].
* [cite_start]**Readme**: This comprehensive document serves as the README, providing instructions, an explanation of the approach, and a flow diagram of the solution[cite: 32].

***

### **Features Implemented Beyond Core Requirements**

In addition to the requested deliverables, this project includes several advanced and creative features to enhance its analytical power and demonstrate a more comprehensive solution.

* [cite_start]**Semantic Search**: Fulfilling the optional bonus task[cite: 35], a semantic search function was implemented using sentence embeddings and `FAISS`. This allows a user to search for clauses based on their meaning rather than just keywords.
* **Comparative Clause Analysis**: This feature uses machine learning to compare a specific clause type across all 50 contracts. It groups textually similar clauses using `KMeans` clustering on their embeddings and then prompts the Gemini model to summarize the common themes and key differences within each group. This allows for the rapid identification of standard vs. outlier legal language.
* **Named Entity Recognition (NER)**: A more advanced prompt was engineered to have the LLM perform NER in addition to clause extraction. It identifies and extracts key metadata like **party names**, the contract's **effective date**, and the **governing law**, providing structured data points that are immediately useful for contract management systems.
* **Interactive Q&A with Retrieval-Augmented Generation (RAG)**: A RAG system was built for interactive querying. When a user asks a question, the system first performs a **semantic search** to find the most relevant clauses. It then "augments" a new prompt with this retrieved context and asks the LLM to generate an answer based *only* on that information, ensuring the response is accurate and grounded in the document's content.
* **Streamlit Web Application**: To create a user-friendly interface, the entire pipeline is packaged into an interactive Streamlit web application. The notebook uses `pyngrok` to generate a public URL, allowing users to upload a contract PDF and receive a full analysis in their web browser, demonstrating a practical application of the backend processing.
