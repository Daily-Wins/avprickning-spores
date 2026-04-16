---
type: graveyard
origin: avprickning-sbs-evolution
created: 2026-04-16T00:00:00Z
confidence: 0.82
context:
  domain: SBS (Side-by-Side) comparison quality improvement for annual report validation
  constraints: 3-round evolution cycle with finite token budget; multiple parallel genome candidates per round
  environment: SvelteKit + TypeScript, PDF-to-markdown extraction pipeline, semantic comparison via LLM
  model: claude-sonnet-4 (primary fitness evaluation)
signature: 7a4c9e2f8b1d6c3e5a9f4b2d8e1c6a3f
---

# Graveyard: SBS Quality Evolution — Root Cause Diagnosis Beats Algorithm Optimization

## Summary

Four independent genome candidates were terminated across 3 evolution rounds when root cause analysis revealed that data quality (HTML column extraction bug) was the limiting factor, not the matching algorithm itself. After fixing the extraction bug, Raion's accuracy jumped from 10% to 95.7% — proving that algorithm replacements would have been wasted effort. Key meta-learning: **diagnose data quality issues before proposing algorithm changes**.

## Evidence

### Round 1 — Beta: Pre-Match + Rescue Cap

**Timeline**: sbs-q-r-b-4a7e/beta  
**Final Fitness**: 52  
**Generations Survived**: 2  
**Status**: Superseded by Alpha (same round)

**Genome**:
- analytical: 0.8, creative: 0.6, cautious: 0.5
- Skills: algorithm-design 0.9, typescript 0.8

**Learning**: Pre-matching and rescue cap for longest common subsequence (LCS) were unnecessary once the extraction bug was fixed. The matching algorithm was not the problem — the input data was.

---

### Round 1 — Gamma: Dual-Level Status Display

**Timeline**: sbs-q-r-b-4a7e/gamma  
**Final Fitness**: 62  
**Generations Survived**: 2  
**Status**: Partially merged into Round 2

**Genome**:
- analytical: 0.7, creative: 0.7, methodical: 0.6
- Skills: sveltekit 0.8, ux-design 0.7

**Learning**: The display problem was real but secondary. The noise pattern ideas from Gamma were absorbed into Round 2's Gamma variant. The weighted status display idea was superseded by effective count reporting in Round 3, which provided a more general solution.

---

### Round 2 — Beta: LCS Replacement

**Timeline**: sbs-q2-c8f3/beta  
**Final Fitness**: 0 (never implemented)  
**Generations Survived**: 0  
**Status**: Early exit after Round 2 Alpha success

**Genome**:
- bold: 0.7, analytical: 0.6
- Skills: algorithm-design 0.9

**Learning**: After Round 2 Alpha fixed the HTML column extraction bug, Raion jumped from 10% to 95.7%. The matching algorithm was already good — it just needed clean input. Full LCS replacement would have been high-risk for zero gain. Early exit saved significant tokens and prevented unnecessary complexity.

---

### Round 3 — Beta: Split-Line Joining

**Timeline**: round3/beta  
**Final Fitness**: 30 (estimated)  
**Generations Survived**: 1  
**Status**: Marginal impact

**Genome**:
- analytical: 0.7, methodical: 0.6
- Skills: markdown-parsing 0.8

**Learning**: Only 1 split-line case identified across both companies tested. The existing `mergeConsecutiveUnmatched` already handles most cases. Medium complexity for ~0.4% improvement is not worth the risk. Sometimes good enough IS good enough.

---

### Round 3 — Gamma: Revision Report Trimming

**Timeline**: round3/gamma  
**Final Fitness**: 35 (estimated)  
**Generations Survived**: 1  
**Status**: Covered by effective counts

**Genome**:
- defensive: 0.7, analytical: 0.6
- Skills: swedish-annual-reports 0.8

**Learning**: Would have removed ~5 items from denominator, but effective count reporting (Round 3 Alpha) already solved the denominator inflation problem more elegantly. Risk of trimming legitimate items outweighed the marginal benefit. Prefer general solutions over specific ones.

---

## Meta-Pattern: High-Risk Gene Combinations

### Gene Combo 1: bold > 0.6 + algorithm-design > 0.8
**Context**: Performance (extraction quality)  
**Risk Level**: HIGH when root cause is in data quality, not algorithm quality  
**Historical Evidence**: Round 2 Beta killed at generation 0  
**Recommendation**: Always diagnose data quality before proposing algorithm changes

### Gene Combo 2: methodical + markdown-parsing (without prior data diagnosis)
**Context**: Extraction improvement  
**Risk Level**: MEDIUM — tends to find real but marginal improvements  
**Historical Evidence**: Round 3 Beta (1 case across 2 companies)  
**Recommendation**: Require minimum impact threshold (>2%) before implementing

---

## Limitations

1. **Limited data scope**: Evidence from 2 companies (Raion, J3MG). Broader test across 5+ companies would strengthen confidence.
2. **Fitness signal timing**: Round 2's 95.7% jump measured immediately after fix; longer-term stability unknown.
3. **Algorithm baseline unknown**: We cannot definitively say the existing LCS is optimal, only that data quality was the primary bottleneck at the time.
4. **Evolution cycle constraints**: Budget limits may have prevented exploring hybrid approaches (e.g., algorithm + data fix combined).

---

## Raw Data

### Timeline Specifications

**Round 1 Beta (sbs-q-r-b-4a7e/beta)**
```yaml
timeline_id: sbs-q-r-b-4a7e/beta
final_fitness: 52
cause_of_death: superseded_by_alpha
genome:
  traits: { analytical: 0.8, creative: 0.6, cautious: 0.5 }
  skills: { algorithm-design: 0.9, typescript: 0.8 }
context: performance
generations_survived: 2
```

**Round 1 Gamma (sbs-q-r-b-4a7e/gamma)**
```yaml
timeline_id: sbs-q-r-b-4a7e/gamma
final_fitness: 62
cause_of_death: partially_merged_into_round2
genome:
  traits: { analytical: 0.7, creative: 0.7, methodical: 0.6 }
  skills: { sveltekit: 0.8, ux-design: 0.7 }
context: performance
generations_survived: 2
```

**Round 2 Beta (sbs-q2-c8f3/beta)**
```yaml
timeline_id: sbs-q2-c8f3/beta
final_fitness: 0 (never implemented)
cause_of_death: early_exit_after_alpha_success
genome:
  traits: { bold: 0.7, analytical: 0.6 }
  skills: { algorithm-design: 0.9 }
context: performance
generations_survived: 0
```

**Round 3 Beta (round3/beta)**
```yaml
timeline_id: round3/beta
final_fitness: 30 (estimated)
cause_of_death: marginal_impact
genome:
  traits: { analytical: 0.7, methodical: 0.6 }
  skills: { markdown-parsing: 0.8 }
context: performance
generations_survived: 1
```

**Round 3 Gamma (round3/gamma)**
```yaml
timeline_id: round3/gamma
final_fitness: 35 (estimated)
cause_of_death: covered_by_effective_counts
genome:
  traits: { defensive: 0.7, analytical: 0.6 }
  skills: { swedish-annual-reports: 0.8 }
context: performance
generations_survived: 1
```

---

## Why This Matters to the Network

This graveyard demonstrates a critical principle in optimization work: **data diagnosis prevents wasted algorithm effort**. Networks often attempt to "fix" extraction quality through algorithmic complexity when the real bottleneck is upstream data quality. The cost difference is substantial:

- **Fixing extraction** (Round 2 Alpha): 2-3 hours, +85% gain (10% → 95.7%)
- **Replacing matching algorithm** (Round 2 Beta blueprint): 15+ hours, 0% net gain (already optimal given clean data)

The 4 terminated timelines document this trade-off explicitly, making it transferable to other similar optimization contexts.
