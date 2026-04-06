
# Document Processing & Semantic Retrieval with Docling

This project started with a simple question:

How do you turn a messy document into something an AI system can actually understand?

What followed was a process of testing, breaking things, fixing them, and gradually building a pipeline that goes from raw PDFs to semantic retrieval.

---

##  Where It Started

The goal was straightforward:

- take a PDF  
- extract the text using OCR  
- convert it into something structured  
- and eventually make it searchable  

To do this, I used **Docling** as the core tool, with different OCR engines behind it.

---

##  Setting Up the Environment

I started by setting up a clean environment to avoid dependency issues later on.

```bash
python -m venv venv
source venv/Scripts/activate
python -m pip install --upgrade pip
pip install "docling[tesseract]"
````

At this point, everything looked fine. The setup was complete, and I was ready to run my first document.

---

## Working in the Terminal

Most of this project was executed directly in the terminal, from running OCR pipelines to managing files and outputs.

![CLI Workflow](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/23-french-docling-full-workflow.png)

This workflow includes:

* navigating project directories
* running Docling commands
* processing multiple documents
* managing outputs

Working in the terminal made it easier to debug issues, rerun commands quickly, and stay aligned with real-world development workflows.

---

##  Running the First Document (Tesseract)

I began with a Swahili document and ran it through Docling using Tesseract:

```bash
./venv/Scripts/docling.exe 02-source-docs/Swahili-words-and-phrases-for-travelers.pdf --to md --ocr-engine tesseract --ocr-lang swa
```

This is where things got interesting.

---

## The First Blocker

Instead of getting output immediately, I ran into an error:

```
OSError: A required privilege is not held by the client
```

At first, it looked like something had gone wrong with the setup. But after reading the error more carefully, it turned out to be a Windows permissions issue.

---

### Fixing It

The problem was that Windows was blocking symlink creation.

The fix was simple:

Enable Developer Mode.

```
Settings → System → For Developers → Developer Mode (ON)
```

![Developer Mode Enabled](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/15-developer-mode-on.png)

Once I made that change, the same command worked.

That moment was important because it showed me that not every error means the whole setup is broken. Sometimes the issue is outside the code.

---

##  First Successful Output

After fixing the issue, the document was successfully converted into Markdown.

![OCR Output Preview](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/17-output-preview.png)

The output was readable and structured, but not perfect.

* some sections were clean
* others had noise or formatting issues

This raised the next question:

Is this good enough, or can it be improved?

---

##  Testing a Second OCR Engine (EasyOCR)

To answer that, I decided to try a different OCR engine: EasyOCR.

First, I installed the required dependencies:

```bash
python -m pip install "docling[easyocr]"
```

Then I ran the same document again, this time switching the OCR engine:

```bash
./venv/Scripts/docling.exe 02-source-docs/Swahili-words-and-phrases-for-travelers.pdf --to md --ocr-engine easyocr --ocr-lang sw
```

![EasyOCR Run](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/25-easyocr-run-and-model-download.png)

---

### What Changed

The pipeline itself didn’t change.

* same document
* same command structure
* same output format

But EasyOCR behaved differently.

* it downloaded models on first run
* it took slightly longer
* and the output formatting was not exactly the same

---

##  Comparing the Outputs

With both pipelines working, I compared the results.

### What stood out:

* Both extracted roughly the same amount of text
* Both struggled with certain sections (especially image-based content)
* Tesseract produced cleaner structure
* EasyOCR occasionally picked up words Tesseract missed

---

## What the Data Shows

To move beyond observation, I visualized some of the differences between the outputs.

---

### Word Count Comparison

![Word Count Comparison](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/01-word-count-comparison.png)

Both engines produced nearly identical word counts, which suggests they extracted a similar amount of content.

However, similar quantity does not necessarily mean similar quality.

---

### Line Length Distribution

![Swahili Line Length](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/02-swahili-line-length.png)

This shows how text is structured across the document.

* shorter lines → structured content
* longer lines → paragraph-style text

---

### What This Confirms

The visualizations reinforce a few key points:

* Similar volume of text does not guarantee similar quality
* Structure and formatting matter just as much as extraction
* OCR performance depends on the nature of the document

---

##  Summary

| Feature    | Tesseract | EasyOCR                              |
| ---------- | --------- | ------------------------------------ |
| Formatting | Better    | Less consistent                      |
| Accuracy   | Good      | Good (slightly better in some cases) |
| Speed      | Faster    | Slower initially                     |
| Setup      | Simple    | Requires model download              |

---

### Decision

For this project, I continued with **Tesseract** because it produced more consistent structured output.

---

## Moving Beyond Extraction

At this point, I had clean Markdown files.

But I wanted to go further.

Instead of just extracting text, I asked:

Can this actually be used in an AI system?

---

##  Building a Retrieval Pipeline

To test that, I built a semantic retrieval pipeline.

The idea was to:

* break the text into chunks
* generate embeddings
* retrieve relevant sections based on a query

![Embedding Setup](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/24-install-sentence-transformers.png)

---

### Result

The system was able to:

- take a question  
- search through the document  
- return relevant sections  

#### Swahili Query Example

![Swahili Retrieval](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/27-rag-query-swahili-results.png)

For a query about greetings in Swahili, the system retrieved relevant phrases such as *“Kwaheri”*, *“Good evening”*, and *“Good night”*, along with similarity scores.

---

#### French Query Example

![French Retrieval](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/28-rag-query-french-results.png)

For a query about French reading concepts, the system retrieved sections discussing *cognates*, *grammar*, and *word similarities*, again ranked by relevance.

---

### What This Shows

- The pipeline is not just storing text  
- It is able to **understand and retrieve meaning based on a query**  
- Results are ranked using similarity scores, making retrieval more useful  

This confirms that the output is not just readable, but usable in downstream AI applications such as semantic search.

##  What I Learned

A few things became very clear during this process:

* Environment issues can block progress just as much as code
* Small CLI mistakes can cause confusion
* OCR outputs require validation
* Different tools behave differently on the same input
* The real value comes after extraction

---

##  Final Pipeline

```
Raw PDF → OCR → Structured Markdown → Analysis → Retrieval
```

Each step builds on the previous one.

---

##  Reflection

![Week 2 Reflection](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/06-images/08-week2-reflection.png)

This week was less about just getting things to work and more about understanding how everything connects.

---

## References

* [OCR Output Analysis Notebook](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/04-notebooks/01-docling-ocr-output-analysis.ipynb)
* [OCR Engine Comparison Notebook](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/04-notebooks/03-tesseract-vs-easyocr-comparison.ipynb)
* [Semantic Retrieval Notebook](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/04-notebooks/02-rag-semantic-retrieval-pipeline.ipynb)
* Technical Notes ([Setup](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/02-docling-environment-setup-and-debugging.md), [Debugging](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/01-windows-symlink-error-and-fix%20(2).md), [EasyOCR](https://github.com/Uxer-Janine/outreachy-docling-task/blob/master/01-notes/02-technical/04-easyocr-pipeline.md))

