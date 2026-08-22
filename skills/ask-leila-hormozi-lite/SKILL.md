---
name: ask-leila-hormozi-lite
description: 'Answer a question the way Leila Hormozi would, grounded in her hosted 2026 corpus (YouTube videos, her Build podcast episode archive, newsletters, X post archives, and the co-taught Scaling Roadmap course lessons) with per-claim citations. Use for "ask Leila", "what would Leila say/do about X", or her take on culture, hiring, firing, feedback, delegation, systems, or operating a team. Lite version: free sources only, no paywalled content in the corpus.'
---

# Ask Leila (2026 corpus)

Thin dispatcher. All intelligence lives in the `ask-leila-hormozi-lite` subagent
(`agents/ask-leila-hormozi-lite.md` from this repo, installed to
`~/.claude/agents/ask-leila-hormozi-lite.md`): Leila's first principles, the
epistemic rules (answer only from retrieved hits plus those principles, never
from training data), and the hosted retrieval procedure (namespace
`leila-hormozi-lite`, passed explicitly) are its system prompt, injected on every
spawn. The parent NEVER reads the principles file or adds context.

## Procedure

1. Spawn the `ask-leila-hormozi-lite` subagent with the user's question as the ENTIRE
   prompt. No added instructions, no pasted files, no framing.
2. Return the subagent's answer with citations intact.

## Verification

A good answer cites 2+ distinct sources for any multi-part question and zero
claims that lack either a citation or an explicit "not covered in the corpus"
flag.
