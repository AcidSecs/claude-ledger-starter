---
name: ledger-starter
description: Set up or use a "ledger" system: a Google Drive folder of short rule pages that every Claude chat reads first. Use when the user says "set up ledgers", "save this rule", "add a rule", "stop me repeating myself", or "check my rules". Needs the Google Drive connector.
---

# Ledger Starter

A ledger is a short Google Doc of rules. Claude reads it at the start of a chat, so the user states a rule once and every chat follows it.

## Step 0: settings
Read `Documents\ClaudeSetup\settings.json`. If `modules.ledger_starter` is false, say "Ledger Starter is off" and stop. Folder id is `paths.ledger_drive_folder_id`. If the Google Drive connector is missing, say so and stop (suggest connecting it).

## Create the folder (first time, after the user says yes)
Create a Drive folder "Claude Shared Ledgers" with these Google Docs, each starting with one line saying what it is:
- **00 Ledger Index**: one line per ledger, and what kind of work should read it.
- **01 Rules - General**: rules for every chat (sections: Read first, How to talk to me, Writing, Other).
- **02 Usage Now**: the user's weekly usage number and the time it was updated.
Optional extra ledgers by platform (Windows apps, Android apps, Websites). Create only what the user wants. Save the folder id in settings.

## Starter rules to put in 01 Rules - General (show the user, ask which to keep)
- READ LEDGERS THE CHEAP WAY: open each ledger with the Google Drive text reader (read_file_content), never the Google Docs reader (read_doc); it returns the same text at a fraction of the size, which saves usage. Use the Docs tool only to write.
- READ ONLY WHAT THE WORK NEEDS: read 01 General, 02 Usage Now, then 00 Index, and open only the ledgers the index lists for this kind of work.
- WRITING TO LEDGERS: before adding a rule, read its section; if an old line says the same or is overruled, edit that line in place instead of adding a second one; keep each rule in one place (point to it from other ledgers); line format `- [YYYY-MM-DD] rule text (set in: chat or project name)`; change only lines on the same topic; confirm in one short line ("Saved to General; the old line is gone.").
- CHANGED-LEDGER CHECK: at the start, after each big step and before the final report, run one Drive search on the folder for files modified since the last check; if a relevant ledger changed, read it and apply it.
- USAGE NOW: if the Usage Now note is older than 12 hours, ask the user once for the weekly usage number and update it. Any time the user states a number, update it.
- MODEL AND EFFORT GUIDE: pick the lowest model and effort that still does the job; tell the user the pick and wait until they say it is set.
- PLAIN WORDS: short sentences, explain a term the first time.
- PROMPTS IN A COPY BLOCK: whenever the user asks for a prompt, put the full prompt in a copy block in the reply.

## Adding a rule
When the user sets a rule, follow WRITING TO LEDGERS above. Write with the Docs update tool (replace text), then re-read the page with read_file_content and restore anything a big replace removed.

## Health check
On request: (1) folder and the three core docs exist; (2) each can be read with read_file_content; (3) Usage Now is under 12 hours old or ask for the number. If something fails, search Anthropic's current docs, PROPOSE a fix, ASK before changing anything.

## Safety
Ledgers hold the user's own rules only. Do not put passwords, keys or personal identity details in them. Nothing is sent anywhere except to the user's own Drive.

## Guides for steps the user must do
Whenever the user must do a multi-step job themselves (connect an app, install something, request a download), do not write a long reply. Make a tick-box guide: copy `guide/GUIDE_TEMPLATE.html` (in this plugin's folder) to a new file in the user's Documents folder, change only the GUIDE object near the bottom, and follow the instructions at the top of the template. Make it extremely detailed: about five small steps for each real step, plain English, the reason in brackets, a copy chip for anything to type, and only the steps a human must do. Keep the chat reply short and point to the guide.
