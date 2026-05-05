# Evening Call Summary - Day 1

**Written by:** Samuel | **Confirmed by:** Bethel

## Feedback Bethel Gave on the Explainer

Bethel confirmed the mechanism section landed clearly - she now understands what prefix caching is and why it would matter for her agent. The most useful part was the model-specific table showing that Qwen3-Next does not support prefix caching and why. She had assumed caching was either automatic or irrelevant and did not know the architectural reason it fails on her specific model.

One piece of feedback: the original explainer assumed Claude as the model which was wrong for her setup. After the morning call clarification the explainer was updated to be model-agnostic and to specifically address Qwen3-Next with the three workaround paths.

## What the Writer Revised

Updated the "What You Need to Add" section to check the model first before applying any fix. Added the Qwen3-Next architectural explanation (Gated Delta Net linear attention layers with recurrent state dependencies) with sources from vLLM issue #25874 and xllm issue #1016. Added three concrete paths forward: application-level response caching, self-hosting via vLLM with the FP8 variant, and switching models for production.

## Gap Closure

Bethel signed off that the gap closed. She now understands why her system prompt tokens are recomputed on every call, why prefix caching is not available on her current model, and what her three options are going forward. The grounding commit updates memo.pdf page 1 to note the cost figure was computed without caching and represents the upper bound.
