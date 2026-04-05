# EasyOCR Pipeline: Running Docling with a Second OCR Engine

The last time I worked on this project, I had successfully built and validated a full OCR pipeline using Tesseract.

With that working end-to-end, the next step was to test a second OCR engine.

This time, I used EasyOCR.

---

## Why EasyOCR?

Tesseract works well, but it is not the only OCR engine available.

**EasyOCR is:**
- deep learning-based
- supports multiple languages
- designed to handle complex text more flexibly

**The goal here was simple:**

- run the same Swahili document  
- use a different OCR engine  
- observe how the pipeline behaves  

---

## EasyOCR Installation and Verification

To enable EasyOCR within Docling, I installed the required dependencies in my virtual environment:

```bash
python -m pip install "docling[easyocr]"
python -m pip show easyocr
```
![Docling EasyOCR Installation](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/26-docling-easyocr-installation.png)

The verification output confirmed that EasyOCR was installed correctly in the project environment.

At this point, the pipeline was ready to use EasyOCR as an OCR backend.

## Running the Pipeline (Swahili)

I then ran the same document as before, this time using EasyOCR:

```bash
./venv/Scripts/docling.exe 02-source-docs/Swahili-words-and-phrases-for-travelers.pdf --to md --ocr-engine easyocr --ocr-lang sw
```
![EasyOCR Run](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/25-easyocr-run-and-model-download.png)

### What Happened

During the first run, EasyOCR downloaded its required models:

detection model
recognition model

**The logs showed:**

"Downloading detection model, please wait..."
"Downloading recognition model, please wait..."

**This confirmed that:**

EasyOCR was correctly integrated
the pipeline successfully switched OCR engines
the system was preparing its deep learning models

## Key Observations
### **1. First Run Takes Longer**

Unlike Tesseract, EasyOCR requires model downloads during the first run.

**This means:**

the first execution is slower
subsequent runs will be faster

### 2. Same Pipeline, Different Engine

One of the most interesting parts of this step was that nothing else changed.

same document
same command structure
same output format

Only this changed:

```bash
--ocr-engine easyocr
```
This highlights how flexible Docling is.

### 3. Different OCR Approach

| Feature                  | Tesseract                          | EasyOCR                     |
|--------------------------|-----------------------------------|-----------------------------|
| Approach                 | Rule-based with trained models     | Deep learning-based         |
| Accuracy                 | Varies depending on input quality  | Generally more adaptive     |
| Layout Handling          | Limited                            | Better with complex layouts |
| Multilingual Performance | Moderate                           | Stronger support            |

## Output

The output was generated just like before. A Markdown file was created and saved in the 03-outputs/ folder.

This shows that the pipeline itself stays consistent even when the OCR engine changes.

At this point, I now have two working setups:

Tesseract pipeline
EasyOCR pipeline

With both in place, the next step is to compare the outputs and see how they differ in terms of structure, noise, and overall quality.

That’s where things start to get interesting.

## The next step is to:

compare EasyOCR vs Tesseract outputs
analyze structure, noise, and readability
determine which pipeline performs better

## Final Thought

This step showed that the pipeline is not tied to a single OCR engine.

With minimal changes, the system can switch between engines and still produce usable outputs.

That flexibility will be critical when preparing documents for AI workflows.