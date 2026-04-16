---
type: graveyard
origin: avprickning
created: 2026-04-16T12:00:00Z
confidence: 0.90
observations: 4
context:
  domain: ai-validation
  constraints: production-ready
  environment: Node.js, OpenRouter API
tags:
  - openrouter
  - error-handling
  - api-integration
  - silent-failure
---

# OpenRouter returns HTTP 200 with error body instead of proper error status

## Summary

OpenRouter can return HTTP 200 OK responses that contain an error object
instead of the expected `choices` array. Standard SDK error handling
(try/catch on API calls) does NOT catch these — the request "succeeds"
but the response lacks the data your code expects.

## The failure pattern

1. Send a valid-looking request to OpenRouter
2. Upstream provider (e.g. Anthropic) rejects the request
3. OpenRouter returns `{ error: { message: "...", code: ... } }` with HTTP 200
4. SDK does not throw an exception
5. Code accesses `response.choices[0].message` and crashes with TypeError

## Why this is dangerous

- Breaks the HTTP contract — errors should be 4xx/5xx
- SDK error handling (try/catch) is bypassed entirely
- The crash happens far from the actual error, making debugging hard
- Intermittent: only happens when upstream rejects, not on every call

## The fix

1. ALWAYS use optional chaining before array index: `response.choices?.[0]`
2. Check for error body explicitly: `if (response.error || !response.choices)`
3. Log the full raw response on failure, not just the parsed fields
4. Consider a wrapper function that normalizes OpenRouter responses

## Observations

- Confirmed with Anthropic, Google, and Meta models via OpenRouter
- Direct Anthropic SDK does NOT have this issue (proper HTTP errors)
- The pattern is especially dangerous in batch processing where one bad
  response kills the entire pipeline
