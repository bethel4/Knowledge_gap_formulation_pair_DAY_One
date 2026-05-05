# Morning Call Summary - Day 1

**Written by:** Samuel | **Confirmed by:** Bethel

## What Was Ambiguous in the Original Drafts

Bethel's original draft asked broadly about KV cache and API costs without being pinned to a specific file or a specific failure condition. My original draft pointed at inference cost variance without naming the exact mechanism I did not understand.

## How the Questions Were Sharpened

During the morning call I asked Bethel which model she was using in her agent. She confirmed Qwen3-Next-80B-A3B via OpenRouter. This was a critical clarification because it changed the answer entirely - Qwen3-Next does not support prefix caching at all due to its hybrid linear attention architecture, meaning the fix is not just adding cache_control but requires a different approach entirely.

Bethel pushed back on my question asking whether I was confused about the cost model or the latency model. That forced me to narrow from general inference cost variance to the specific prefill vs decode cost split that underlies the 73% cost delta claim in ablation_results.json.

## What Changed

Bethel's question moved from "explain KV cache and costs" to a specific named file (agent/openrouter_client.py), a specific missing implementation, and a specific model (Qwen3-Next) whose architectural constraints change the answer. My question moved from "explain inference cost" to the specific cost delta between constrained and baseline conditions in ablation_results.json and which phase (prefill or decode) drives it.

## Partner Attestation

Both partners confirmed their final questions were unambiguous and resolvable in one explainer. ✓
