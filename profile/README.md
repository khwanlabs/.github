## Khwan — the memory layer for AI agents

Khwan is not a model and does not replace one. It wraps the agent you already run
and gives it memory that survives the session, rules distilled from the times you
corrected it, and a gate that can refuse an answer before it ships.

The whole contract is three calls:

```
POST /prepare   → recalled memory + your constitution + a coherence gate   (no model call)
      your model answers                                                    (Khwan is absent)
POST /record    → persist the turn, and learn from it                       (no model call)
```

**Khwan never runs your model and never holds your provider key.** There is no
hosted chat path in the engine — it produces context, not answers.

### Works with

| Host | Getting started |
| --- | --- |
| Claude Code, over MCP | `claude mcp add khwan` · [docs](https://docs.khwan.ai/claude-code) |
| ChatGPT, as a Custom GPT | [docs](https://docs.khwan.ai/chatgpt) — reaches free accounts, no Plus required |
| Hermes Agent | [docs](https://docs.khwan.ai/hermes) — the answer-gate exposed as a tool the model can call on itself |
| CrewAI | [docs](https://docs.khwan.ai/crewai) — prepare in a Flow step, verify before the send, record after |
| Anything else | [Connect your agent](https://docs.khwan.ai/connect-your-agent) — the pattern, and the rules an integration has to follow |

### Repositories here

| | |
| --- | --- |
| [khwan-client-python](https://github.com/khwanlabs/khwan-client-python) | Python SDK — `pip install khwan` |
| [khwan-client-ts](https://github.com/khwanlabs/khwan-client-ts) | TypeScript SDK — `npm i @khwan/client` |
| [khwan-mcp](https://github.com/khwanlabs/khwan-mcp) | MCP server — `uvx khwan-mcp` |
| [khwan-chat](https://github.com/khwanlabs/khwan-chat) | A reference chat app built on the loop |

All MIT. The hosted engine is closed; everything that runs on your machine and
touches your data is not.

### Memory you can audit

Synthesis distils your corrections into standing rules. Retrieval only ever
*reinforces* a rule — a wrong one that stays relevant would be renewed forever —
so every rule is listable, traceable to the turns it came from, and deletable:

```python
for l in kw.lessons():
    print(l["response_text"], "←", l["source_link"])
kw.delete_lesson(bad_id)
```

### What it is good at, and what it is not

Retrieval is by meaning, ranked by confidence. That makes Khwan strong on
**preferences, standing rules, and project context that has to survive the
session** — per person, per repo, per customer.

It is weaker than a temporal knowledge graph at tracking an entity whose state
keeps changing, and it has no point-in-time index. The
[architecture page](https://docs.khwan.ai/architecture) says which is which,
including a section on where it is weak.

---

[khwan.ai](https://khwan.ai) · [docs](https://docs.khwan.ai) · [dashboard](https://app.khwan.ai)
