# Docling + RapidOCR Pipeline (Execution and Observations)

This document focuses on using Docling with RapidOCR to process scanned documents.

---

## Running Docling with RapidOCR

I used the following command to process a scanned Swahili document:

```bash
./venv/Scripts/docling.exe 02-scanned-source-docs/Swahili-words-and-phrases-for-travelers.pdf --to md --ocr-engine rapidocr --ocr-lang swa
```
---

## Execution Output

The logs show that RapidOCR successfully initialized and executed using the ONNX runtime backend.

![RapidOCR Execution](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/29-rapidocr-swahili-and-french-processing-success.png)


## What Happened During Execution

From the output logs, the following steps occurred:

1. RapidOCR initialized using the ONNX runtime engine
2. Required OCR models were located and validated:
3. detection model
4. classification model
5. recognition model
6. No model downloads were required because they already existed locally
7. The OCR pipeline executed without errors

This indicates that the environment and dependencies were correctly configured.

---
## Initial Error Encounter

While processing the French document, I initially ran:
```bash
./venv/Scripts/docling.exe 02-source-docs/French-sample.pdf --to md --ocr-engine rapidocr --ocr-lang fr
```
**This resulted in:**

```bash
Error: The input file 02-source-docs/French-sample.pdf does not exist.
```
The issue was caused by an incorrect folder name.

**Fix**

I corrected the path to match the current project structure:

```bash
./venv/Scripts/docling.exe 02-scanned-source-docs/French-sample.pdf --to md --ocr-engine rapidocr --ocr-lang fr
```
After this correction, the command executed successfully.

---
### Observations
1. RapidOCR integrates smoothly with Docling
2. Model loading is efficient when files are already cached
3. The ONNX runtime backend enables fast execution
4. Errors encountered were related to file paths, not the OCR engine itself
---

### Output

The processed files were successfully generated and saved in the 03-outputs/ directory.

## Key Takeaways
1. Correct file paths are critical when working with CLI tools
2. RapidOCR provides a stable and efficient OCR pipeline
3. Once models are downloaded, subsequent runs are faster
4. Most issues encountered were environmental rather than tool-related

## Next Steps
1. Compare RapidOCR output with Tesseract and EasyOCR
2. Evaluate differences in accuracy and structure
3. Identify strengths and weaknesses of each OCR engine

