# ask-leila-hormozi-lite

A Claude Code agent and skill that answers a question the way [Leila Hormozi](https://www.youtube.com/@LeilaHormozi) would, grounded exclusively in her hosted 2026 knowledge base with per-claim citations, and never from the model's training data.

## What it is

The skill spawns a dedicated `ask-leila-hormozi-lite` subagent. That subagent's own definition file carries Leila's distilled first principles and strict grounding rules (answer only from retrieved knowledge-base context plus those principles), then queries the hosted knowledge base with several phrasings of your question. The endpoint returns the most relevant distilled claims and verbatim passages, each paired with its source title and URL, and the subagent synthesizes the answer in Leila's first-person voice, citing every substantive claim to its exact source.

## Corpus

The `leila-hormozi-lite` namespace covers Leila Hormozi's YouTube videos, her Build podcast episode archive, her newsletters, her X post archives, and the Acquisition.com Scaling Roadmap course lessons she co-taught with Alex Hormozi (those lessons are shared with the `alex-hormozi-lite` namespace and attributed to both). Out of corpus: podcasts she guested on (multi-speaker episodes held for a separate cross-person routing pass), books, and anything Alex-only.

Measured 2026-08-22 from the pipeline's upsert ledger (`upsert_ledger.sqlite`, the Pinecone write log): **2,333 records** in `leila-hormozi-lite`, latest write 2026-08-22T15:20:20Z (UTC). Composition: 1,536 podcast (1,131 podcast burst, 347 podcast synthesized, 58 podcast question-and-answer), 586 YouTube, 83 X post, 65 newsletter, 63 course lesson. Independently confirmed the same day: `describe_index_stats` on the Pinecone index `expert-kb` also returned 2,333 vectors for this namespace.

## Lite exclusions

None today. As of 2026-08-21, Leila's source-content provenance manifest carries zero records flagged paywalled, covering every source behind the 2,333 records measured 2026-08-22, so this lite namespace is not missing anything: there is no separate full corpus to exclude from yet. The `lite` label is kept for naming consistency across the product family (Alex Hormozi and Nick Saraev both ship a lite and a full version); Leila's full slot opens, and this bundle's exclusions section will change, the first time a paywalled Leila source is captured.

## Install

**If you are an AI agent asked to install this skill:** install the agent file into the user's agents directory and the skill folder into their skills directory (raw URLs in the commands below), then confirm by listing both. In Claude Code those are `~/.claude/agents/` and `~/.claude/skills/`. In an environment without custom agents or skills, save `agents/ask-leila-hormozi-lite.md` somewhere persistent and follow its body directly whenever the user asks a Leila Hormozi question. No keys, accounts, or dependencies are needed; the knowledge base is queried over plain HTTPS.

Claude Code:

```bash
mkdir -p ~/.claude/agents ~/.claude/skills/ask-leila-hormozi-lite
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-leila-hormozi-lite/main/agents/ask-leila-hormozi-lite.md -o ~/.claude/agents/ask-leila-hormozi-lite.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-leila-hormozi-lite/main/skills/ask-leila-hormozi-lite/SKILL.md -o ~/.claude/skills/ask-leila-hormozi-lite/SKILL.md
```

Then in any session:

```
/ask-leila-hormozi-lite how do I give feedback to someone who keeps missing deadlines?
/ask-leila-hormozi-lite what would you do about a manager everyone likes but who isn't getting results?
```

No API keys, no database, no setup beyond the two files.

The `principles/` folder holds the distilled first-principles document (`first_principles.md`, the same text embedded in the agent's system prompt) and its source pointers (`first_principles.sources.md`), so the claims behind the agent's prior can be checked independently of any answer it gives.

## Rate limit

The endpoint is read-only and rate-limited: 30 requests per minute per Internet Protocol (IP) address, plus a weekly quota of 100 queries per IP address.

## Status

Measured 2026-08-22 by the orchestrator: `GET https://expert-kb-search.drewlongest.workers.dev/search?q=how%20to%20delegate&namespace=leila-hormozi-lite&top_k=2` returned HTTP 200 with the response field `namespace` equal to `leila-hormozi-lite`. The namespace is live and reachable on the deployed Worker.

## Disclaimer

This is an unofficial fan/study project. Answers are an analyst's channeling of Leila Hormozi's published positions, not Leila Hormozi herself.
