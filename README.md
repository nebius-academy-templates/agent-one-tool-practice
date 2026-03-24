# Practice 1 — Build Your First Agent Tool

In this practice you will build one real tool for an AI agent: **`get_ticket`**. It fetches a ticket by ID from a GitHub Issues database and returns structured JSON.

---

## Repository layout

```
agent-one-tool-practice/
├── skills/
│   └── get_ticket/       ← implement your tool here
│       └── get_ticket.py
├── output/
│   └── ticket.json       ← your tool writes here
└── README.md
```

---

## Step 0 — Preparation

1. Confirm this repo (`agent-one-tool-practice`) appears in your GitHub account.
2. Clone it and open it in your editor.
3. Check that you have Python 3.9+ installed: `python --version`
4. Install the GitHub CLI if you don't have it: https://cli.github.com

> **Tickets database** — this repo's GitHub Issues act as the ticket database.
> Browse them at `https://github.com/<your-username>/agent-one-tool-practice/issues`

---

## Step 1 — Implement `get_ticket`

Create `skills/get_ticket/get_ticket.py`. The tool must:

| Requirement | Detail |
|---|---|
| Input | `ticket_id` — a GitHub issue number (string or int) |
| Source | GitHub Issues on **this repo** |
| Output | JSON written to `output/ticket.json` |

### Required JSON shape

```json
{
  "ticket_id": "3",
  "title": "Add Observability",
  "description": "Full issue body text...",
  "comments": ["comment 1 body", "comment 2 body"]
}
```

> Field names must match exactly — the verifier checks them.

### Starter prompt (use this with Claude Code or any AI assistant)

```
Create a Python script called get_ticket.py.
It must define a function get_ticket(ticket_id: str) -> dict.
Use the GitHub REST API (no authentication needed for public repos) to fetch
issue number `ticket_id` from the repo `<your-github-username>/agent-one-tool-practice`.
Also fetch its comments.
Return a dict with keys: ticket_id, title, description, comments (list of comment bodies).
At the bottom of the script, when run directly:
  - Call get_ticket with the ticket_id passed as a CLI argument (sys.argv[1])
  - Write the result as pretty-printed JSON to output/ticket.json
  - Print the JSON to stdout as well
```

Replace `<your-github-username>` with your actual username before running the prompt.

---

## Step 2 — Test the tool

Fetch the ticket titled **"Add Observability"** (issue #3):

```bash
python skills/get_ticket/get_ticket.py 3
```

Verify that `output/ticket.json` was created and looks correct.

---

## Step 3 — Validate locally

```bash
python scripts/verify_output.py output/ticket.json
```

All checks must pass before you submit.

---

## Submission checklist

- [ ] `skills/get_ticket/get_ticket.py` exists
- [ ] `get_ticket(ticket_id)` returns JSON with `ticket_id`, `title`, `description`, `comments`
- [ ] `output/ticket.json` contains the result for ticket **#3 — Add Observability**
- [ ] Local verifier passes
- [ ] Changes committed and pushed to `main`

Then go back to the LMS lesson and click **Submit**.

---

## Tips

- The GitHub Issues API is public for public repos — no token needed.
- `requests` is the easiest HTTP library: `pip install requests`
- If you get rate-limited, set a `GITHUB_TOKEN` env variable and pass it as a Bearer token in the request header.
- Comments live at a separate endpoint: `GET /repos/{owner}/{repo}/issues/{issue_number}/comments`
