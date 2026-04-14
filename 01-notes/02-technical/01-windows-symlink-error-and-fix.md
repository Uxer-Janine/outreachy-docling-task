

# Resolving Docling WinError 1314 on Windows (Symlink Permission Issue)

After successfully setting up my environment and installing Docling with its dependencies, I ran my first real OCR task on a scanned document.

This is when I encountered my first major blocker.

---

## The Command

I ran the following command from the project root directory to convert a scanned Swahili PDF into Markdown using Tesseract OCR:

```bash
./venv/Scripts/docling.exe 02-scanned-source-docs/Swahili-words-and-phrases-for-travelers.pdf --to md --ocr-engine tesseract --ocr-lang swa
```
This command:

Takes a scanned Swahili PDF
Applies OCR using Tesseract
Converts the document into Markdown format

---

## The Error

![WinError 1314](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/10-winerror1314-traceback.png)

*Docling failed during execution with `WinError 1314`, followed by a temporary file access error.*

The key message was:

> WinError 1314: A required privilege is not held by the client

At first glance, this looked like a major failure. Since I had already spent a lot of time setting up the environment, my first instinct was to wonder whether something had gone wrong with the installation.

But after slowing down and reading the traceback more carefully, I realized the problem was not with Docling itself.

---

## What the Error Actually Meant

This error meant that Windows had blocked Docling from doing something because my system had not given it enough permission.

From the traceback, the issue happened during model download, when the system tried to create a **symlink**.

A symlink is like a shortcut or pointer to a file instead of creating a full copy of it.

That detail mattered because it helped me understand that:

- Docling had already started running
- the dependencies were installed correctly
- the process had reached the model download stage
- the real issue was system permissions, not package installation

So this was not a random failure, and it was not proof that my setup was completely broken. It was a Windows-specific permission issue.

---

## Root Cause

I did some research and learned that on Windows, creating symlinks usually requires one of the following:

- Developer Mode enabled
- administrator privileges

Since Developer Mode was turned off on my machine, Windows blocked the symlink operation and raised `WinError 1314`.

That was the root cause of the problem.

---

## How I Investigated It

Once I understood that the issue was related to Windows permissions, I moved away from the terminal and into system settings.

### Step 1: Open Windows Settings

![Settings Home](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/11-settings-home.png)

*Opening Windows Settings to begin investigating the permission issue.*

---

### Step 2: Navigate to System settings

![System Advanced](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/12-system-advanced.png)

*Moving through system settings to find the developer-related options.*

---

### Step 3: Check whether Developer Mode is enabled

![Developer Mode Off](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/13-developer-mode-off.png)

*Developer Mode was turned off, which explained why Windows blocked symlink creation.*

At this point, things became much clearer. The setup was not the issue. The problem was that my system was not allowing the operation Docling needed in order to continue.

---

## The Fix

The fix was to enable **Developer Mode** in Windows.

### Step 4: Confirm enabling Developer Mode

![Developer Mode Confirmation](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/14-developer-mode-confirmation.png)

*Windows confirmation prompt shown before enabling Developer Mode.*

---

### Step 5: Turn Developer Mode on

![Developer Mode On](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/15-developer-mode-on.png)

*Developer Mode successfully enabled.*

Once Developer Mode was enabled, Windows now allowed symlink creation.

That was the main fix for the `WinError 1314` issue.

---

## Re-running the Command

After enabling Developer Mode, I returned to the project directory and ran the same Docling command again.

![Docling Success](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/16-docling-success.png)

*Re-running the Docling OCR command after enabling Developer Mode.*

This time, the command completed successfully.

That confirmed that:
- the environment was working
- the OCR pipeline was working
- the issue had really been the Windows setting

---

## Output File

After the command completed successfully, the generated Markdown file was moved into the `03-outputs/` folder.

![Output File](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/18-output-file-in-outputs-folder.png)

*Generated Markdown file successfully saved in the `03-outputs/` folder.*

---

## Output Preview

![Output Preview](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/05-screenshots/windows-screenshots/17-output-preview.png)

*Preview of the generated Markdown output after the permission issue was resolved.*

The output was not perfect, but it clearly showed that:
- OCR had run
- Docling had processed the Swahili document
- the pipeline was now working end-to-end

---

## What I Learned

### 1. Not every error means the whole setup failed

At first, the traceback looked like everything had broken. But the system had already progressed quite far before it failed. The issue was specific, not catastrophic.

### 2. Environment matters just as much as code

Even when the commands and packages are correct, system-level settings can still block a workflow.

### 3. Reading the traceback carefully saves time

The traceback pointed to symlink creation during model download. That helped me understand the actual problem instead of reinstalling everything blindly.

### 4. Debugging is part of the real work

This experience reminded me that technical work is not just about running tools successfully. It is also about understanding why they fail and how the system around them behaves.

### 5. Small changes can unlock major progress

Enabling Developer Mode was a simple change, but it unblocked the entire pipeline.

---

## Next Steps

Now that the pipeline is working, the next technical markdown will focus on what happened after the fix.

The next step is to:
- analyze the quality of the OCR output
- identify what Docling extracted well and what it struggled with
- compare structure, readability, and accuracy
- begin thinking about how this output could be prepared for RAG workflows

---

## Final Thought

This issue looked intimidating at first, but it ended up being one of the most useful parts of the project so far.

It taught me that sometimes the problem is not the code.

Sometimes, the system just needs permission to let the code run.