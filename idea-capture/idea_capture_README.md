# Idea Capture → Trello

A single-file HTML tool that captures ideas directly to Trello via the REST API. No middleware, no webhooks, no dependencies. Save it to your desktop, open in a browser, done.

## Why this exists

During sustained LLM collaboration, tangential ideas surface constantly. The model detects them — it says "let's file that for later" — and steers the conversation back. But "let's file that" is a speech act, not an action. The idea lives only in the conversation transcript, which you may never reread and which context compaction may discard.

This tool turns the speech act into an actual act. The model produces a capture card (or you type one), you send it to Trello, the idea has an address. The conversation continues on task.

For the full rationale, see `Idea_Capture_as_LLM_Discipline.md` in this repo.

## Setup (one time, ~5 minutes)

### 1. Get Trello API credentials

1. Go to [trello.com/power-ups/admin](https://trello.com/power-ups/admin)
2. Create a Power-Up (any name, e.g. "Idea Capture")
3. Navigate to the **API Key** tab and generate a key
4. On the same page, click the **Token** link to generate a personal token (click Allow)

### 2. Get your list ID

1. Open your target Trello board in a browser
2. Copy the board's short ID from the URL (e.g. `FUU9O72j` from `trello.com/b/FUU9O72j/idea-capture`)
3. Open this URL in your browser, replacing the placeholders:

```
https://api.trello.com/1/boards/YOUR_BOARD_ID/lists?key=YOUR_API_KEY&token=YOUR_TOKEN
```

4. Find the list you want and copy its `"id"` value (24-character string)

### 3. Configure the HTML file

1. Open `idea_capture.html` in a text editor
2. Find the `CONFIG` section near the bottom:

```javascript
const CONFIG = {
  apiKey: 'YOUR_TRELLO_API_KEY',
  token: 'YOUR_TRELLO_TOKEN',
  listId: 'YOUR_TRELLO_LIST_ID'
};
```

3. Replace the three placeholder values with your credentials
4. Save the file

### 4. Use it

Open the HTML file in any browser. Two modes:

- **Form mode:** Fill in title, description, status, source project, and optional URL. Click Send.
- **JSON mode:** Expand "Paste JSON instead" and paste output from any LLM chat. Click Send.

## JSON schema for LLM capture

Any Claude chat (or any LLM) can produce a capture card on request with a prompt like:

> "Give me a Trello card JSON for this idea."

Expected output:

```json
{
  "title": "Short title of the idea",
  "description": "One or two sentences — what the thread is and why it matters.",
  "status": "Seed",
  "url": "https://claude.ai/project/..."
}
```

Paste this into the JSON textarea and click Send.

## Security note

Your API key and token are stored in the HTML file in plain text. This is fine for a local file on your desktop. Do not host this file on a public server or commit it with your real credentials to a public repo.

## Customization

- **Status options:** Edit the `<select id="status">` element to change or add statuses (e.g. "Parked", "Exploring", "Blocked").
- **Multiple boards:** Duplicate the file with different `listId` values for different projects.
- **Labels:** Trello's API supports `idLabels` in the card creation call — add label IDs to the `params` object in `createCard()` if you want color-coded cards.
