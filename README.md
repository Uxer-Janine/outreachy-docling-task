## Table of Contents

- [Document Processing \& Semantic Retrieval with Docling](#document-processing--semantic-retrieval-with-docling)
  - [Overview](#overview)
  - [What is Docling?](#what-is-docling)
  - [OCR Engines Used](#ocr-engines-used)
    - [Why These OCR Engines?](#why-these-ocr-engines)
    - [Evaluation Criteria (Key Metrics)](#evaluation-criteria-key-metrics)
  - [Project Structure](#project-structure)
    - [Project Structure (Overview)](#project-structure-overview)
  - [Setup](#setup)
  - [Challenges \& Debugging](#challenges--debugging)
  - [OCR Processing](#ocr-processing)
  - [Data Analysis Insights](#data-analysis-insights)
  - [OCR Engine Comparison](#ocr-engine-comparison)
  - [Supported Language Codes](#supported-language-codes)
    - [Tesseract / RapidOCR](#tesseract--rapidocr)
    - [EasyOCR](#easyocr)
  - [Semantic Retrieval](#semantic-retrieval)
  - [Notebooks](#notebooks)
  - [Key Findings](#key-findings)
  - [Presentation](#presentation)
  - [Dev Notes](#dev-notes)
  - [Conclusion](#conclusion)
  - [AI Assistance Disclosure](#ai-assistance-disclosure)

# Document Processing & Semantic Retrieval with Docling

This project demonstrates how scanned documents can be transformed into structured, AI-ready data using OCR and then used for semantic retrieval.

Pipeline:
Raw PDF → OCR → Markdown → Analysis → Retrieval 

---

## Overview

The objective is to:
- extract text from scanned documents  
- evaluate OCR quality across multiple engines  
- analyze structure and noise  
- test usability in a semantic retrieval (RAG-style) pipeline  

---

## What is Docling?

Docling is an open-source document processing tool that converts complex documents into structured formats such as Markdown or JSON, enabling downstream AI workflows.

<img src="https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/04-Docling%20explained.png" width="600"/>

---

## OCR Engines Used

### Why These OCR Engines?

Three OCR engines were selected to represent different approaches to text extraction and to enable a meaningful comparison:

- **Tesseract** → a widely-used, traditional OCR engine known for reliability and strong structure preservation. It performs well on structured documents and produces consistent, clean layouts.

- **EasyOCR** → a deep learning-based engine designed for flexibility and multilingual support. It handles varied and complex text better, but may introduce formatting inconsistencies.

- **RapidOCR** → an ONNX-based pipeline optimized for speed and efficiency. It is lightweight, fast, and easy to run, making it suitable for scalable workflows.

Each engine offers a different balance of:
- speed  
- accuracy  
- formatting consistency  
___

### Evaluation Criteria (Key Metrics)

Using all three makes it possible to evaluate not just which extracts text, but which produces the most usable output for downstream AI tasks.

It allows comparison of:
- extraction accuracy  
- structure preservation  
- noise levels  
- suitability for downstream tasks like semantic retrieval   

---

## Project Structure

```
outreachy-docling-task/
├── 01-notes/
├── 02-scanned-source-docs/
├── 03-outputs/
│   ├── tesseract_outputs/
│   ├── easyocr_outputs/
│   ├── rapidocr_outputs/
├── 04-notebooks/
├── 05-screenshots/
├── 06-images/
├── 07-presentation/
├── README.md
```

### Project Structure (Overview)


The project is organized to reflect the full pipeline:

- **01-notes/** → technical and non-technical reflections
- **02-scanned-source-docs/** → raw input PDFs   
- **03-outputs/** → OCR results grouped by engine  
- **04-notebooks/** → analysis, comparison, and retrieval workflows  
- **05-screenshots/** → setup, debugging, and execution evidence  
- **06-images/** → visual outputs (plots and diagrams) 
- **07-presentation/** → Non-technical Presentation

This structure follows a clear flow:
**raw data → OCR → analysis → retrieval**

---

## Setup

A clean virtual environment was created to ensure consistent dependency management and avoid conflicts between different OCR engines.

```bash
python -m venv venv
source venv/Scripts/activate
pip install "docling[tesseract]"
pip install "docling[easyocr]"
pip install rapidocr onnxruntime
```

This setup installs Docling along with the required OCR engines and their dependencies:

- Tesseract support via docling[tesseract]
- EasyOCR integration via docling[easyocr]
- RapidOCR + ONNX runtime for fast inference

This prepares a controlled environment for running OCR pipelines, testing multiple engines, and ensuring reproducible results across the project.

---

## Challenges & Debugging

During setup and execution, several issues surfaced that affected the OCR pipeline.

- **Windows permissions (WinError 1314)**  
  Symlink creation was blocked during model downloads.  
  This was resolved by enabling Developer Mode.

- **DLL errors with PyTorch**  
  Conflicts from multiple Python installations (Anaconda vs venv) caused runtime failures.  
  This was fixed by rebuilding the environment using a clean virtual environment.

- **Multiple Python environments**  
  Inconsistent behavior across commands revealed overlapping Python installations.  
  Standardizing on a single environment resolved the issue.

- **OCR model setup delays**  
  EasyOCR required downloading models on first run, increasing execution time.

These challenges highlighted that environment configuration is a critical part of building reliable OCR pipelines, not just an initial setup step.

---

## OCR Processing

Example (Tesseract):

```bash
./venv/Scripts/docling.exe 02-scanned-source-docs/Swahili-words-and-phrases-for-travelers.pdf --to md --ocr-engine tesseract --ocr-lang swa
```

The same document was processed using:

- Tesseract
- EasyOCR
- RapidOCR

Each run used the same input but a different OCR engine.

The outputs were stored separately and used for comparison and analysis in later stages. 

---

## Data Analysis Insights

<p align="center">
  <img src="https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/01-word-count-comparison.png" width="30%"/>
  <img src="https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/02-swahili-line-length.png" width="30%"/>
  <img src="https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/03-french-line-length.png" width="30%"/>
</p>

These visualizations highlight how document structure influences OCR output.

**Insights:**
- Similar word counts ≠ similar quality  
- Structured documents produce shorter, segmented lines  
- Paragraph-based documents produce longer, continuous lines  
- OCR noise varies depending on layout and engine behavior  

---

## OCR Engine Comparison

| Feature                | Tesseract               | EasyOCR                    | RapidOCR              |
| ---------------------- | ----------------------- | -------------------------- | --------------------- |
| Engine Type            | Traditional OCR         | Deep learning-based        | ONNX-based            |
| Speed                  | Fast                    | Slower (first run)         | Fast                  |
| Accuracy               | Good                    | Good (slightly variable)   | Good                  |
| Structure Preservation | Strong                  | Moderate                   | Good                  |
| Formatting Consistency | High                    | Lower                      | Moderate              |
| Noise Level            | Low                     | Slightly higher            | Moderate              |
| Setup Complexity       | Medium (language packs) | Easy but model download    | Simple + ONNX runtime |
| Best Use Case          | Structured documents    | Flexible multilingual text | Efficient pipelines   |

---

## Supported Language Codes

<table align="center">
<tr>
<td align="center" width="50%">

### Tesseract / RapidOCR

| Language | Code |
|----------|------|
| English  | eng  |
| French   | fra  |
| Swahili  | swa  |
| Arabic   | ara  |

</td>

<td align="center" width="50%">

### EasyOCR

| Language | Code |
|----------|------|
| English  | en   |
| French   | fr   |
| Swahili  | sw   |
| Arabic   | ar   |

</td>
</tr>
</table>

---

## Semantic Retrieval

OCR outputs were converted into embeddings using SentenceTransformers, enabling meaning-based search instead of keyword matching.

**Process**
- clean text  
- split into chunks  
- generate embeddings  
- retrieve relevant chunks using similarity  

**What This Enables**
- semantic search  
- RAG-style workflows  

**Example:**  
A query like *“How do I greet someone in Swahili?”* returns the most relevant phrases from the document.

--- 

## Notebooks
- [OCR Output Analysis](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/04-notebooks/01-docling-ocr-output-analysis.ipynb)
- [OCR Engine Comparison](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/04-notebooks/02-rag-semantic-retrieval-pipeline.ipynb)  
- [Semantic Retrieval Pipeline](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/04-notebooks/02-rag-semantic-retrieval-pipeline.ipynb)  

---

## Key Findings

All three OCR engines produced usable outputs from the same document, but with noticeable differences in quality and consistency.

- **Tesseract** delivered the most stable and well-structured output, making it easier to work with in downstream tasks  
- **EasyOCR** showed flexibility and strong text recognition, but introduced more formatting noise  
- **RapidOCR** provided a strong balance between speed and output quality, making it efficient for practical workflows  

**Most importantly:**

- The quality of OCR output directly affects downstream AI performance.  
- Cleaner, well-structured text leads to better results in tasks like chunking, embedding generation, and semantic retrieval.

---

## Presentation

* [Download Presentation (PDF)](./07-presentation/docling-project-presentation.pdf)
* [Download Presentation (PPTX)](./07-presentation/docling-project-presentation.pptx)

---

## Dev Notes

**Technical Reflections:**

- [windows symlink error and fix](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/01-windows-symlink-error-and-fix.md)
- [Docling environment and setup](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/02-docling-environment-setup-and-debugging.md)
- [Docling tesseract-ocr](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/03-docling-tesseract-ocr.md)
- [Docling easy-ocr](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/04-docling-easyocr.md)
- [Docling rapid-ocr](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/05-docling-rapidocr.md)
  

**Non-Technical Reflections:**

- [week1-reflection](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/03-dev-notes/01-weeek-1.md)
- [week2-reflection](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/03-dev-notes/02-week-2.md)
- [week3-reflection](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/03-dev-notes/03-week-3.md)

---

## Conclusion

This project shows that OCR is not just about extracting text, but about producing structured, high-quality data that can be effectively used in downstream AI systems.

The choice of OCR engine directly impacts the reliability of analysis and retrieval tasks, making evaluation and comparison a critical part of the pipeline.

---

## AI Assistance Disclosure

AI tools were used throughout this project to:
- help debug environment and setup issues  
- assist in structuring parts of the code  
- refine explanations and improve documentation clarity  

The core ideas, experimentation, and analysis were carried out independently, with AI serving as a supporting tool rather than a substitute for the work itself.

