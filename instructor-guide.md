# Instructor Guide: Data Pipelines for AI Applications

## Module Overview

In this module, learners design basic data collection and preprocessing workflows to support AI personalization and analytics. The goal is to shift their mindset from "Consuming AI" (asking questions) to "Feeding AI" (preparing data).

## Learning Objectives

By the end of this session, students will be able to:

* **Design and implement simple data pipelines** to feed and store AI-related data for web applications.
* **Apply preprocessing techniques** to clean and format raw user data.
* **Utilize LangChain** to orchestrate the flow of data between inputs, models, and storage.

## Session Pacing Guide (120 Minutes total)

| Duration | Segment | Instructor Action |
| --- | --- | --- |
| **20 min** | **Intro to Pipelines (ETL)** | Use the "Water Treatment Plant" analogy. Raw water (User Data)  Filter (Preprocessing)  Storage (Database). |
| **20 min** | **Data Collection Strategy** | Discuss "Garbage In, Garbage Out." Ask students: "If we train an AI on Reddit comments, what kind of personality will it have?" (Bias/Toxic). |
| **25 min** | **Preprocessing Techniques** | Live Code: Write a simple Regex to remove emails. Explain why we do this *before* the AI sees it (Privacy/GDPR). |
| **25 min** | **Intro to LangChain** | Explain that LangChain is the "Glue" code. It creates a standard interface for different models. Show a simple "Prompt Template" example. |
| **30 min** | **Lab: Ingestion Pipeline** | Facilitate the lab. Help students connect the output of their cleaning function to the input of the LangChain chain. |

---

## Delivery Notes & Teaching Tips

### 1. The "Water Treatment" Analogy (ETL)

* **Raw Source:** A river (User Input). It has fish, mud, and trash.
* **Filtration Plant (Transform):** Screens and chemicals that remove the bad stuff. This is your **Node.js Preprocessing** script.
* **Bottled Water (Load):** The clean product ready for drinking. This is the **Vector Database** or clean JSON file ready for the AI.

### 2. Why LangChain?

* **Concept:** Students might ask, "Why not just use the OpenAI SDK?"
* **Answer:** LangChain is model-agnostic. If you build a pipeline with LangChain, you can swap OpenAI for Claude or a local Llama model later with very few code changes. It also handles "Chaining" (Step 1 output becomes Step 2 input) much better than nested callbacks.

### 3. Privacy First

* **Critical Teaching Point:** Emphasize that **Data Pipelines are the first line of defense for privacy.**
* **Scenario:** "A user accidentally pastes their password into the support chat. Your pipeline must catch that and redact it before it gets saved to the database or sent to OpenAI."

---

## Lab Guide: Building a Data Ingestion Pipeline

**Lab Objective:** Build a small data ingestion pipeline to store user input for model improvement.

### Setup

* **Tools:** Node.js, VS Code.
* **Libraries:** `langchain`, `openai`, `dotenv`.

### Common Student Issues & Solutions

* **Issue:** `Regex is too hard.`
* *Solution:* Don't make them write complex regex from scratch. Provide the email regex snippet. The focus is on the *pipeline flow*, not regex mastery.


* **Issue:** "LangChain isn't working/import errors."
* *Solution:* LangChain updates frequently. Ensure they are using the documentation for the specific version installed, or stick to the basic `LLMChain` and `PromptTemplate` imports shown in the slides.


* **Issue:** "Where is the data going?"
* *Solution:* Since we are using a local `fs` (file system) write, show them the `data.json` file appearing in their project folder. Open it to verify the data is there.



### Debrief Questions

At the end of the lab, ask:

1. "If we wanted to analyze thousands of these feedbacks, would a JSON file still work?" (No, we'd need a real database).
2. "What other things should we 'clean' besides emails?" (Profanity, HTML tags, credit card numbers).

---

## Resources for the Instructor

* **Reference:** [LangChain Conceptual Documentation](https://www.google.com/search?q=https://docs.langchain.com/docs/)
* **Reference:** [Regex101 (for testing patterns)](https://regex101.com/)
* **AI Prompt Bank:**
* *"Write a Node.js function that removes all credit card numbers from a string."*
* *"Explain the difference between ETL and ELT to a beginner."*