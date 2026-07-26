# Cohesium sourcing runner

A Claude Code skill for running company sourcing against the Cohesium app.

It exists as its own repository for one reason: Claude Code asks you to pick a
repository when you create an environment, and you should not need access to the
main application repository to run sourcing. Pick this one instead. There is no
application code here — just the skill and this README.

## What it does

Sourcing research is expensive, and most of its cost is wasted re-researching
companies already on file. This skill avoids that:

1. **Shortlist** candidate company names cheaply — no verification yet.
2. **Ask the API** which of them are already known, compared against every
   record held. No cap, no truncation.
3. **Research** only the ones that came back new — the expensive per-company
   work, with a source URL required for every row.
4. **Ingest** the results back into the app as a tracked, gradeable batch.

## Setup

### 1. Get a token

In the app: **Settings → API tokens → Create token**. Copy it immediately — only
its hash is stored, so it cannot be shown again.

A token acts as the person who created it and is limited to three sourcing
endpoints. It cannot reach the rest of the app.

### 2. Point this folder at the app

Copy `.env.example` to `.env` and fill both values:

```bash
cp .env.example .env
```

```
COHESIUM_API_URL=https://your-app.example.com
COHESIUM_API_TOKEN=cin_...
```

`.env` is gitignored. If your Claude Code environment sets environment variables
for you, use that instead — the skill checks the environment first and only
falls back to this file.

### 3. Use it

Open this folder in Claude Code and ask in plain language:

> Source 25 customers in the Richmond VA metro, 20–100 employee professional
> services firms.

> Find 10 customers each for Northwind IT and Cobalt Managed Services.

> Find 25 more providers in Virginia.

The skill runs the whole loop and reports back: how many candidates it
shortlisted, how many were already known (the research it avoided), how many it
researched and imported, and the batch id.

## What happens to the results

They land in the app under **Review & Enrich** as a normal batch, and still have
to pass **Grade** before they can be enriched or used for outreach. The runner
is not a shortcut around review — it produces the same records, reviewed on the
same terms, with a source URL required for every row.

## Notes

- **Keep the token out of version control.** It is a bearer credential with no
  second factor. Share it over something ephemeral, not email or chat.
- **Each person should have their own token**, named for them. Revocation is
  then per person, and every run records which token produced it.
- If a token is lost or exposed, revoke it in **Settings → API tokens** and
  create another. Revocation takes effect immediately.
