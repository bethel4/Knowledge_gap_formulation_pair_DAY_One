# question.md



**Specific question:**
When my agent calls the LLM 60+ times per week with the
same system prompt, are those tokens recomputed each time
or served from KV cache? What do I need to add to my API
call to guarantee prefix caching fires? What conditions
would silently invalidate that cache  for example, if I
inject today's date or the prospect's name into the system
prompt? And what is the real dollar cost difference between
a cache hit and a cache miss at 60 calls per week?

## Grounded in My Work

**1. memo.pdf — Page 1, cost per qualified lead paragraph**
I wrote a cost per qualified lead number but I cannot
break it into components. If my system prompt is being
reprocessed 60 times per week without caching, my cost
number is wrong and I cannot defend it to the Tenacious CFO.

**2. agent/email_handler.py — the API call itself**
I call the API with the full system prompt on every
invocation. I have never added cache_control. I have
never checked cache_read_input_tokens. I do not know
my cache hit rate.

## Why This Generalizes

Any FDE building a multi-prospect outbound system sends
the same system prompt hundreds of times per week.
Understanding when prefix caching fires and what silently
breaks it is the difference between a defensible cost
estimate and one that collapses under a CFO's first question.

