---
type: graveyard
origin: avprickning
created: 2026-04-16T12:00:00Z
confidence: 0.95
observations: 5
context:
  domain: ai-validation
  constraints: production-ready
  environment: Node.js, SvelteKit, OpenRouter, Anthropic Claude
tags:
  - image-processing
  - mime-type
  - anthropic-api
  - vision-mode
  - silent-failure
---

# MIME type mismatch silently blocks AI vision processing

## Summary

When sending images to Anthropic Claude via OpenRouter for vision-based analysis,
the MIME type in the API payload MUST match the actual image format. A mismatch
(e.g. declaring `image/jpeg` when sending PNG bytes) causes Anthropic to return
a 400 "Could not process image" error — but OpenRouter wraps this in a 200 OK
response with an error body instead of the expected `choices` array.

## The failure pattern

1. Pipeline generates PNG images (e.g. via sharp `.png()`)
2. API payload declares `mimeType: 'image/jpeg'` (copy-paste from earlier JPEG code)
3. Anthropic rejects the image but OpenRouter returns HTTP 200
4. SDK does not throw — `response.choices` is undefined
5. Code crashes on `response.choices[0]` with cryptic TypeError

## Why this is dangerous

- The error is **not** at the HTTP level — no 4xx status code to catch
- The error message ("Could not process image") doesn't mention MIME types
- If the pipeline format changes (JPEG to PNG), the MIME declaration is often forgotten
- The crash blocks ALL subsequent reanalysis, not just the affected document

## The fix

1. Always verify MIME type matches actual output format in the image pipeline
2. Use optional chaining: `response.choices?.[0]` (guard BEFORE array index)
3. When image format changes anywhere in pipeline, grep for ALL MIME type declarations
4. Add a validation step: check magic bytes match declared MIME type before API call

## Observations

- Encountered 3 times across different pipeline changes
- Each time, the root cause was a format change without updating MIME declarations
- Anthropic is strict about MIME/content match; other providers may be more lenient
- OpenRouter's 200-wrapping of errors is a separate but compounding issue
