# thread.md

---

**Tweet 1 — The question (hook + stakes)**

Our constrained sales agent prompt costs 73% more per task than the baseline.

We couldn't explain why.

The prompt was longer. The outputs were longer. Both changed at once.

Here's the decomposition — and why the answer surprised me. 🧵

---

**Tweet 2 — The mechanism (load-bearing)**

Every LLM call has two phases:

→ PREFILL: processes your prompt in parallel. Cheap.
→ DECODE: generates tokens one at a time. Expensive.

Claude Sonnet pricing:
- Input: $0.25 / 1M tokens
- Output: $3.00 / 1M tokens

Output costs 12× more. This is architecture, not pricing strategy.

---

**Tweet 3 — The compounding effect (the surprising part)**

Here's what I didn't know:

A bigger input doesn't just cost more upfront. It makes every output token more expensive too.

Larger KV cache → each generated token attends over more memory → higher compute per token.

Solovyeva & Castor (2026) measured this: prefill amplifies decode cost by 1.3% to 51.8% depending on the model.

---

**Tweet 4 — The actual breakdown**

So where does the 73% actually come from?

~10–15% → longer system prompt (prefill)
~50–60% → more output tokens (decode volume)
~10–20% → higher cost per output token (compounding)

The constrained prompt didn't just add rules.
It induced verbosity.
Verbosity is the cost.

---

**Tweet 5 — The fix (babbling suppression)**

The highest-leverage fix has a name: babbling suppression.

3 out of 10 models in Solovyeva & Castor kept generating after the task was done. No accuracy gain. Pure cost.

Suppressing it saved 44–89% of energy.

The fix: detect task completion mid-stream and stop.

```python
for token in stream():
    generated += token
    if is_complete(generated):
        break
```

Make the model say less. Not read less.

---

**Tweet 6 — Link + takeaway**

Full breakdown with KV cache mechanics, the SimPO Pareto claim explained, and 4 concrete cost fixes:

→ [link to blog post]

Sources: Kwon et al. (2023) PagedAttention paper + Solovyeva & Castor (2026) Green AI paper. Both linked in the post.
