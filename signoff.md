# Signoff: Gap-Closure Judgment

## Judgment
**Closed**

Before this pass, I did not have a reliable way to tell whether repeated outbound-generation calls were paying full prompt cost each time or reusing work, and I could not quantify that in a defensible cost-per-qualified-lead narrative. I now understand two distinct layers clearly: provider-side prefix caching (model-dependent, not guaranteed from our current payload alone) and application-level response caching (now implemented in `agent/openrouter_client.py` for identical prompt tuples). I also now have direct observability, because each call emits a structured `LLM_USAGE` record with cache-hit status, model name, and input-token figures (plus optional estimated cost when `LLM_COST_PER_M_TOKENS` is set). The key new understanding is that our memo risk was not only “missing caching,” but “missing measurement.” With telemetry and deterministic cache behavior in place, I can now support cost claims with actual run data instead of assumptions.
