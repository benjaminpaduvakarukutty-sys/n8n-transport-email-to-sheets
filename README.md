# Transportanfrage → KI → Google Sheets (n8n)

**EN:** A small n8n workflow for dispatch: it reads transport request emails, uses AI (Google Gemini) to extract the key details, and writes them as a new row into Google Sheets – no manual typing.

**DE:** Ein kleiner n8n-Workflow für die Disposition: Er liest E-Mails mit Transportanfragen, zieht mit KI (Google Gemini) die wichtigsten Angaben heraus und schreibt sie automatisch als neue Zeile in Google Sheets – ohne Abtippen.

```
📧 Gmail Trigger  →  🤖 Information Extractor (Gemini)  →  📋 Google Sheets (Append row)
```

## How it works / So funktioniert es
1. **Gmail Trigger** – checks the inbox every minute, only emails with `Transportanfrage` in the subject.
2. **Information Extractor + Gemini** – reads the email text and returns 4 fields:

| Field | Meaning |
|---|---|
| `VON` | Ort, wo der Lkw startet (start location) |
| `nach` | Ort, wo der Lkw ankommt (destination) |
| `ankunftszeit` | Uhrzeit der Ankunft (arrival time) |
| `fahrer` | Name des Fahrers (driver) |

3. **Google Sheets** – appends one row: `von | nach | Ankunftszeit | fahrer`.

## Example / Beispiel
**Email (Subject: Transportanfrage):**

> Guten Tag, unser Lkw startet in Hamburg und fährt nach Berlin. Er kommt gegen 18:15 Uhr an. Der Fahrer heißt Ali Yilmaz.

**New row in the sheet:**

| von | nach | Ankunftszeit | fahrer |
|---|---|---|---|
| Hamburg | Berlin | 18:15 Uhr | Ali Yilmaz |

## Setup
1. In n8n: **Import from File** → `workflow.json`
2. Connect your own credentials: Gmail, Google Gemini (or n8n AI credits), Google Sheets
3. Create a Google Sheet with the headers `von | nach | Ankunftszeit | fahrer` in row 1 and select it in the Sheets node
4. Send a test email with the subject `Transportanfrage` → **Execute workflow**

No credentials or private IDs are included in this repository.

## Ideas for next steps
- Let the AI first decide *"is this a transport request?"* instead of relying on the subject line
- Add date, cargo and weight fields
- Send a Telegram/Slack message to the dispatcher for urgent requests
- Human-in-the-loop: a person confirms the row before it is used

---
Built by **Benjamin Paduva** · [milosquirrel.de](https://milosquirrel.de)
