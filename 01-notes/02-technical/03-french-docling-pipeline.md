# Docling Pipeline — French Document (Tesseract OCR)

In the previous step, I had successfully resolved environment issues and confirmed that the Docling pipeline could process a scanned document end-to-end.

To further validate the setup, I ran a second document through the same pipeline — this time in French.

As part of this process, I also recorded my screen while running the pipeline to document the workflow end-to-end. This will be included as a reference once the video is finalized.

---

## Objective

The goal of this step was to:

- run a new document through the existing pipeline  
- verify that the setup works across multiple inputs  
- confirm OCR functionality using a different language (`fra`)  

---

## Initial Error (CLI Syntax Issue)

![CLI Error](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/19-docling-invalid-option-error.png)

While attempting to run the command, I encountered a CLI error caused by incorrect flag formatting.

The issue came from writing:

`--ocr - engine`

instead of:

`--ocr-engine`

This resulted in the error:

`No such option: -e`

To resolve this, I used:

`docling --help`

to confirm the correct command syntax.

---

## Running the Correct Command

`./venv/Scripts/docling.exe 02-source-docs/French-sample.pdf --to md --ocr-engine tesseract --ocr-lang fra`

After correcting the flag, the command executed successfully.

This confirmed that:

- the environment was correctly configured  
- the pipeline could process a second document  
- language switching (`swa` → `fra`) worked as expected  

---

## Output Preview

![Output Preview](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/21-french-output-preview.png)

To verify the output, I previewed the generated Markdown file.

This showed:

- readable extracted text  
- preserved structure  
- correct content flow  

At this stage, it was clear that OCR had successfully processed the French document.

---

## Output File (Saved)

![Output Folder](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/22-french-output-in-folder.png)

The generated file was confirmed inside the `03-outputs/` directory.

This ensured that:

- the file was saved correctly  
- outputs were organized consistently  
- the workflow remained structured  

---

## Full Workflow Overview

![Full Workflow](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/23-french-docling-full-workflow.png)

This captures the full pipeline:

- running the command  
- previewing the output  
- moving the file  
- confirming it in the outputs folder  

---

## Key Observations

- The pipeline is stable across multiple documents  
- OCR works correctly with different languages  
- Output remains structured and usable  
- The workflow is repeatable  

---

## What I Learned

1. Small syntax errors can break execution  
   A single misplaced space caused the command to fail.

2. CLI tools should be verified, not assumed  
   Using `--help` made it easy to identify the issue.

3. Testing multiple inputs is important  
   A pipeline is only reliable if it works beyond a single document.

4. Structure is as important as extraction  
   Clean output is necessary for downstream processing.

---

## Next Step

The next step is to:

- compare outputs across documents  
- evaluate OCR quality and structure  
- prepare the data for downstream tasks such as RAG  
