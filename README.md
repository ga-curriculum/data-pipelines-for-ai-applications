# DATA PIPELINES FOR AI APPLICATIONS

**Session Time:** 120 minutes

---

## Table of Contents

1. Introduction to Data Pipelines (ETL)
2. Data Collection Strategies for AI
3. Preprocessing: Cleaning Data for Models
4. Orchestration with LangChain
5. Lab: Building a Data Ingestion Pipeline
6. Wrap-Up and Reflection
7. Resources

---

## Module Overview

Learners design basic data collection and preprocessing workflows to support AI personalization and analytics. While previous modules focused on getting data *out* of an AI, this module focuses on getting data *in*—preparing user inputs and interactions to improve models or power personalization features.

---

## Learning Objectives

By the end of this module, participants will be able to:

1. **Design and implement simple data pipelines** to feed and store AI-related data for web applications.
2. **Apply preprocessing techniques** such as sanitization, tokenization, and formatting to ensure data quality for AI models.
3. **Utilize LangChain** to create sequential chains that automate the flow of data from user input to storage or further processing.

---

## Session Breakdown

| Segment | Topic | Duration (minutes) |
| --- | --- | --- |
| 1 | Introduction to Data Pipelines (ETL) | 20 |
| 2 | Data Collection Strategies for AI | 20 |
| 3 | Preprocessing: Cleaning Data for Models | 25 |
| 4 | Orchestration with LangChain | 25 |
| 5 | Lab: Building a Data Ingestion Pipeline | 30 |

---

## 1. Introduction to Data Pipelines (ETL)

A **Data Pipeline** is a set of automated processes that move data from one place to another, transforming it along the way. In AI, this is critical because raw data is often messy and unusable by models.

### The ETL Model

* **Extract:** 
  <br>Gathering data from sources (User forms, API logs, databases).
* **Transform:** 
  <br>Cleaning and formatting the data (Removing HTML tags, anonymizing names, vectorizing text).
* **Load:** 
  <br>Storing the clean data in a destination (Vector Database, Data Warehouse).

```mermaid
graph LR
    A[Raw User Input] -->|Extract| B[Node.js Server];
    B -->|Transform| C[Clean & Format];
    C -->|Load| D[Database / AI Model];

```

---

## 2. Data Collection Strategies for AI

To build personalized AI experiences, you need to collect the right signals without violating user privacy.

### Types of AI-Ready Data

* **Explicit Feedback:**
  <br>Thumbs up/down on an AI response. This is gold for "Fine-Tuning."
* **Implicit Signals:** 
  <br>How long a user spends reading a generated article (Dwell time).
* **Contextual Data:**
  <br>The user's location or previous purchase history, which helps the AI generate relevant answers.

### Example: The Feedback Loop

If you are building a chatbot, every interaction is a data point.

1. **User:** 
   <br>"Show me shoes."
2. **AI:** 
   <br>Shows boots.
3. **User:** 
   <br>*Clicks "Next".* (Implicit signal: "I don't want boots").
4. **Pipeline:** 
   <br>Capture this event  Tag "Boots" as negative for this session.

---

## 3. Preprocessing: Cleaning Data for Models

"Garbage In, Garbage Out." If you feed an AI messy data, you get bad predictions. Preprocessing is the "Transform" step where we clean the data.

### Common Preprocessing Steps

1. **Sanitization:** Removing PII (Personally Identifiable Information) like emails or phone numbers to ensure privacy.
2. **Normalization:** Converting text to lowercase or removing special characters to standardize inputs.
3. **Chunking:** Splitting long documents into smaller pieces so they fit within an AI model's "Context Window."

### Code Example: Simple Sanitization

```javascript
function preprocessInput(text) {
    // Remove email addresses for privacy
    const sanitized = text.replace(/[\w-\.]+@([\w-]+\.)+[\w-]{2,4}/g, "[REDACTED]");
    // Trim whitespace and lowercase
    return sanitized.trim().toLowerCase();
}

```

---

## 4. Orchestration with LangChain

**LangChain** is a powerful framework designed to build pipelines specifically for LLMs. It allows you to chain multiple operations together—like reading a file, cleaning it, and then sending it to an AI—using a standardized syntax.

### Why LangChain?

It abstracts the complexity of connecting different data sources (PDFs, Databases, APIs) to AI models. It treats these connections as "Chains."

### Example: A Sequential Chain

Imagine a pipeline that takes a user's complaint, summarizes it, and then detects the sentiment.

```mermaid
graph TD
    Input[User Complaint] --> Chain1[Summarization Chain];
    Chain1 -->|Summary| Chain2[Sentiment Analysis Chain];
    Chain2 -->|Label| Output[Database];

```

**Conceptual LangChain Code:**

```javascript
import { OpenAI } from "langchain/llms/openai";
import { PromptTemplate } from "langchain/prompts";
import { LLMChain } from "langchain/chains";

// 1. Define the model
const model = new OpenAI({ temperature: 0 });

// 2. Define the prompt template (The Transformation)
const template = "Summarize this text in one sentence: {inputText}";
const prompt = new PromptTemplate({ template, inputVariables: ["inputText"] });

// 3. Create the Chain (The Pipeline)
const chain = new LLMChain({ llm: model, prompt: prompt });

// 4. Run the Pipeline
const res = await chain.call({ inputText: "Long user complaint..." });

```

---

## 5. Simple data pipelines for AI based web applications.

Integrating an AI model is not just about making a single API call; it requires managing the flow of data. If a user uploads a 50-page PDF and wants to "chat" with it, you cannot send the entire document to the AI API in one request due to size limits and latency. 

Instead, you must build a pipeline on your Node.js backend to handle this. We use **LangChain**, a powerful framework designed to chain together various data processing steps for Large Language Models. 

A standard AI data pipeline involves three main steps:
1.  **Ingestion (Loading):** Reading the raw unstructured data (e.g., extracting text from a PDF or scraping a webpage).
2.  **Transformation (Chunking):** Splitting the massive text into smaller, manageable chunks.
3.  **Vectorization and Storage:** Converting the text chunks into vector numbers using an Embeddings API and storing them in a database for quick retrieval later.

**Practical Example: Using Node.js and LangChain**
Here is how an engineer might use LangChain on a Node.js server to process a text document into smaller pieces suitable for AI ingestion:

```javascript
// Node.js Backend Pipeline Example using LangChain
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { Document } from "langchain/document";

async function processDataPipeline(rawText) {
  // Step 1: Ingestion - Wrap the raw text in a LangChain Document format
  const doc = new Document({ pageContent: rawText });

  // Step 2: Transformation - Configure the text splitter to break the document into chunks
  const textSplitter = new RecursiveCharacterTextSplitter({
    chunkSize: 1000, // Maximum characters per chunk
    chunkOverlap: 200, // Overlap ensures sentences aren't cut off abruptly
  });

  // Execute the split
  const splitDocs = await textSplitter.splitDocuments([doc]);
  
  console.log(`Pipeline output: Split into ${splitDocs.length} chunks.`);
  
  // Step 3 (Next Phase): Send splitDocs to an Embeddings API
  return splitDocs;
}

```

---

## 6. Architectural Flow of an AI Data Pipeline

To visualize this integration, we can map the data flow from the moment the user uploads a file to the moment the processed data is safely stored and ready for AI inference.

```mermaid
graph TD
    subgraph Client-Side [Browser / React]
        A[User Uploads Document]
    end

    subgraph Server-Side [Node.js Backend]
        B[Receive File in Express]
        C[LangChain: Document Loader]
        D[LangChain: Text Splitter]
        E[OpenAI Embeddings API]
    end

    subgraph Storage & Inference [Database / AI]
        F[(Vector Database)]
    end

    %% Pipeline Flow
    A -->|Raw Unstructured Data| B
    B -->|Ingestion| C
    C -->|Transformation| D
    D -->|Calls Embeddings API| E
    E -->|Returns Vector Numbers| F
    
    classDef react fill:#61dafb,stroke:#333,stroke-width:2px,color:#000;
    classDef node fill:#339933,stroke:#333,stroke-width:2px,color:#fff;
    classDef db fill:#ff9900,stroke:#333,stroke-width:2px,color:#000;
    
    class A react;
    class B,C,D,E node;
    class F db;

```

---

## 7. Lab: Building a Data Ingestion Pipeline

**Objective:** Build a small data ingestion pipeline to store user input for model improvement.

**Refer:** [**Building a Data Ingestion Pipeline**](https://git.generalassemb.ly/build-ai-powered-web-apps/Lab7_Building-a-Data-Ingestion-Pipeline)

---

## 8. Wrap-Up and Reflection

**Reflection Questions:**

1. Why is it important to scrub PII (emails/names) *before* sending data to an external AI provider like OpenAI?
2. How does LangChain help manage complex pipelines compared to writing raw nested functions?

---

## 9. Resources

* [LangChain JS Documentation](https://js.langchain.com/docs/)
* [Node.js File System API](https://nodejs.org/api/fs.html)
* [The ETL Data Pipeline Explained](https://www.ibm.com/topics/etl)