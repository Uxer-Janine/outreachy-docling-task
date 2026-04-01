# Docling Setup & Debugging – What Actually Happened

This is a record of how I set up Docling for the Outreachy Fedora project — including what worked, what didn’t, and what I had to figure out along the way.

I’m writing this as honestly as possible, because a lot of the learning came from things *not* working.

---

## Getting Started

![Fedora Account Setup](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/00-fedora-account-setup.png)

The first step was setting up my Fedora account and signing the contributor agreement.

What I thought would be a quick setup step ended up taking much longer than I expected. It took me over three hours just to update my avatar. At the time, I had missed some of the instructions shared by the mentor in the documentation, and I had also overlooked helpful guidance that other contributors were sharing in the public chat. Because of that, I kept trying to figure things out on my own without realizing the answer had already been given.

I also kept saving and deleting my profile several times because I had checked the option that keeps some information private. I was comparing my profile with screenshots from my peers and thinking I was missing an important step somewhere. After so many attempts, I finally realized the issue was not that I had done something wrong completely — it was that my privacy settings were hiding information I expected to see.

That whole experience was frustrating, but it also reminded me that even the simplest setup tasks can take time when you are working in a new ecosystem. It was my first real lesson in slowing down, reading carefully, and not assuming I had failed just because something looked different from what others had.

---

## Realising My Environment Was Messy


![Python Paths](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/01-python-paths-and-runtimes.png)

After getting through the initial setup, I was ready to start running things — or so I thought.

I quickly ran into issues, and I honestly didn’t know where to start debugging from. I tried using code snippets I found on Stack Overflow, but they weren’t working the way I expected. That made things even more confusing because I couldn’t tell whether the issue was with the code itself or something deeper.

At that point, I decided to step back and get some guidance, and that’s when I turned to ChatGPT. I needed help understanding what I might be missing, not just fixing one error at a time.

After several attempts and a bit of back and forth, it started to become clear that the problem might not be the commands I was running — it might be my environment.

That’s when I checked my Python setup more carefully and realized I had multiple Python installations on my machine. Looking back, that explained why things felt inconsistent and unpredictable.

That moment was important for me, because it shifted my thinking from “this code isn’t working” to “something in my setup might be wrong.”

---

## Starting Over (Properly This Time)

![Clean Venv](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/02-clean-venv-python-3.14.png)

Once I realized the issue might be coming from my environment, trying to fix things piece by piece didn’t make sense anymore.

So I decided to start over.

I created a new virtual environment using the correct Python installation and stayed away from Anaconda this time. It felt a bit uncomfortable at first — like I was throwing away time I had already spent — but deep down I knew it was the right move.

Looking back, this was one of the turning points. Instead of forcing things to work, I chose to reset and do it properly.

---

## Installing Docling

![Docling Install](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/03-docling-install-success.png)

After setting up a clean environment, installing Docling went much smoother than before.

This time, there were no strange errors or unexpected behavior. Everything installed the way it was supposed to, and that alone gave me some confidence that I was finally on the right track.

---

## Checking If It Actually Works

![Docling CLI](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/04-docling-cli-available.png)

I didn’t want to assume that installation meant everything was working.

So I checked whether the CLI tools were available and accessible.

Seeing the `docling` commands show up in the terminal was reassuring. It meant I could actually interact with the tool, not just install it and hope for the best.

---

## Fixing What Broke Earlier (PyTorch)

![Torch](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/05-torch-install-verification.png)

Earlier on, PyTorch had caused issues for me — especially with DLL errors — and that contributed to the confusion I was already feeling.

After rebuilding the environment, I tested it again.

This time it worked without issues.

It might seem like a small step, but for me it was confirmation that the reset had actually fixed something real.

---

## Double-Checking Everything

![Docling Version](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/06-docling-install-verification.png)

Before moving forward, I wanted to be sure everything was in place.

So I checked the installed Docling version and confirmed it was accessible from the environment.

It wasn’t the most exciting step, but it helped me trust my setup before going any further.

---

## Running Things From the Right Place

![Project Navigation](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/07-project-navigation-and-env-setup.png)

I also made sure I was working from the correct project directory.

This is one of those small things that’s easy to overlook, but it can lead to confusing errors if you get it wrong.

At this point, I was being more intentional with every step, just to avoid going in circles again.

---

## Following Tooling Hints

![hf_xet](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/08-hf-xet-install-success.png)

At some point during setup, I came across a warning related to Hugging Face downloads.

Earlier, I might have ignored something like that. But this time, I paid attention and followed the suggestion to install `hf_xet`.

It worked, and it made me realize that tools often tell you exactly what they need — you just have to slow down enough to notice.

---

## Making Sure OCR Is Ready

![Tesseract](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/09-tesseract-install-verification.png)

Since the project involves scanned documents, OCR is a key part of the pipeline.

I installed Tesseract and verified that it was accessible from the command line.

At this point, it felt like all the pieces were finally coming together.

---

## The Point Where Things Broke

![Error](https://raw.githubusercontent.com/Uxer-Janine/outreachy-docling-task/master/screenshots/10-docling-winerror1314.png)

Then I ran Docling.

And this is where I hit a real blocker.

I encountered:
- `WinError 1314` (a permission issue related to symlinks)  
- followed by `WinError 32` (a temporary file access issue)  

At first, it was frustrating — especially because everything else had finally started working.

But after taking a step back, I realized something important.

The system had:
- installed correctly  
- loaded dependencies  
- and progressed all the way to downloading models  

So this wasn’t a random failure. It was a very specific, system-level issue.

That changed how I looked at it.

---
## What I Did Next

After encountering the error, I didn’t want to jump into random fixes without understanding what was happening.

From the error message, it seemed like the issue was related to Windows permissions, specifically around symlink creation during model download.

At this point, I decided to pause and document the issue clearly before attempting fixes. My next steps will involve:

- Investigating Windows symlink permissions  
- Trying to run the process with elevated privileges  
- Exploring alternative setups if necessary  

I wanted to approach the fix with intention rather than trial and error.


## What I’m Taking From This

This whole process changed how I think about technical work.

It’s not just about writing or running code. A lot of the real work happens in:
- setting up environments  
- understanding dependencies  
- and figuring out why things break  

I also learned that documenting everything is just as important as fixing things.

Writing things down helped me:
- stay organized  
- avoid repeating mistakes  
- and actually understand what I was doing  

---

## Closing Thought

This wasn’t a smooth setup — and honestly, I’m glad it wasn’t.

Because that’s where most of the learning happened.

I’m starting to understand that things breaking isn’t a sign that you’re doing something wrong — it’s part of the process.

What matters is how you approach it.