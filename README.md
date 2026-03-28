# AΩ⁺ Fractal Definition Protocol

> **Executable Semantic Compiler for Tetralectic Concept Generation**

AΩ⁺ is a formal protocol that enables large language models (LLMs) to act as **semantic compilers**: given a concept, the protocol generates a tetralectic structure — four symmetric poles (thesis, antithesis, deviation, parathesis) — together with definitions, a lookup table, and a validation layer. It works without any pre‑defined lexicon, relying solely on the model’s semantic knowledge.

-----

## 📖 Table of Contents

- [Overview](#overview)
- [Core Principles](#core-principles)
- [Pipeline](#pipeline)
  - [Step 1: Domain Detection](#step-1-domain-detection)
  - [Step 2: Axis Discovery](#step-2-axis-discovery)
  - [Step 3: Slot Selection](#step-3-slot-selection)
  - [Step 4: Result Lookup Table](#step-4-result-lookup-table)
  - [Step 5: Definition Template](#step-5-definition-template)
  - [Step 6: Pole Generation](#step-6-pole-generation)
  - [Step 7: Validation](#step-7-validation)
- [Example Execution: Χρόνος (Time)](#example-execution-χρόνος-time)
- [JSON Output Schema](#json-output-schema)
- [System Instruction (Prompt)](#system-instruction-prompt)
- [Informative Appendices](#informative-appendices)
  - [Appendix A: Political Organization (Democracy)](#appendix-a-political-organization-democracy)
  - [Appendix B: Economic Management (Frugality)](#appendix-b-economic-management-frugality)
- [Conclusion](#conclusion)

-----

## Overview

The **Tetralectic Theory AΩ⁺ (XL)** provides a geometric framework for analyzing concepts along two independent axes: **Form (M)** and **Ethos (H)**. The four poles are defined by the sign pairs:

|Pole          |M |H |
|--------------|--|--|
|Thesis (θ)    |+M|+H|
|Antithesis (/)|–M|–H|
|Deviation (§) |+M|–H|
|Parathesis (~)|–M|+H|

This protocol turns that theory into an executable pipeline that an LLM can follow to generate a full tetralectic definition set from any input concept, using only its internal semantic knowledge.

-----

## Core Principles

### Axes vs. Slots

- **Axes** are conceptual dimensions (e.g., Form: continuity / moment; Ethos: wisdom / vanity).
- **Slots** are linguistic realizations of the poles of an axis (e.g., `"wise"` for +H, `"futile"` for –H).

### LeanSyntax

Each definition follows the structure:

```
[slot_H] [slot_M] [connector] [slot_R]
```

where the **connector** is a fixed syntactic operator (e.g., `"towards"`, `"for the sake of"`) and the article belongs to `slot_R`.

### Constraints

|Type    |Description                                               |
|--------|----------------------------------------------------------|
|**Hard**|Must be satisfied; otherwise the tetralectic is `INVALID`.|
|**Soft**|May be flagged but do not invalidate the output.          |

-----

## Pipeline

### Step 1: Domain Detection

Identify the broad domain of the target concept **X** (e.g., Politics, Ethics, Knowledge, Time). Extract basic domain characteristics.

-----

### Step 2: Axis Discovery

Generate several candidate axis pairs from semantic contrasts within the domain. For each candidate, compute:

```
Score = 0.4 × Orthogonality + 0.3 × DomainRelevance + 0.3 × DiscriminativePower
```

- **Orthogonality** = `1 – semantic_overlap(axis_M, axis_H)`
  `semantic_overlap` is approximated as the maximum cosine similarity between the model’s internal representations (or any available embedding function) of the four pole pairs:
  `max cosine_similarity(aᵢ, bⱼ)` for `aᵢ ∈ {axis_M⁺, axis_M⁻}`, `bⱼ ∈ {axis_H⁺, axis_H⁻}`.
- **DomainRelevance** ∈ [0,1] — how directly the axes relate to the domain (estimated by the LLM).
- **DiscriminativePower** ∈ [0,1] — how clearly the axes distinguish the four poles (estimated by the LLM).

Choose the highest‑scoring pair.

-----

### Step 3: Slot Selection

For each axis, select semantic slots (multi‑word phrases allowed) that realize the poles:

- `slot_H⁺` (positive ethical quality), `slot_H⁻` (negative)
- `slot_M⁺` (form A), `slot_M⁻` (form B)

> **Hard constraint:** The exact lexical form of each slot must remain unchanged across all poles (only polarity changes).

-----

### Step 4: Result Lookup Table

Build a table mapping each `(slot_H, slot_M)` pair to a unique, concrete result slot `slot_R`.

- **Hard constraint:** All `slot_R` values must be distinct.
- **Soft constraint:** The polarity of `slot_R` tends to correlate with the polarity of `slot_H`, but its semantic content is fully determined by the pair.

-----

### Step 5: Definition Template

Create a static definition template following `[slot_H] [slot_M] [connector] [slot_R]`.
The connector is constant for all poles and does not include the article; articles belong to `slot_R`.

-----

### Step 6: Pole Generation

Using the lookup table, generate the four poles:

|Pole|Pair                |Result   |
|----|--------------------|---------|
|θ   |`(slot_H⁺, slot_M⁺)`|`slot_R⁺`|
|§   |`(slot_H⁻, slot_M⁺)`|`slot_R⁻`|
|~   |`(slot_H⁺, slot_M⁻)`|`slot_R⁺`|
|/   |`(slot_H⁻, slot_M⁻)`|`slot_R⁻`|

-----

### Step 7: Validation

Check all hard constraints:

- [ ] **Morphological symmetry:** `M_θ = M_§`, `M_θ = –M_/ = –M_~`
- [ ] **Ethical symmetry:** `H_θ = H_~`, `H_θ = –H_/ = –H_§`
- [ ] **Lookup table completeness:** all four `(slot_H, slot_M)` pairs have defined `slot_R`
- [ ] **Uniqueness of `slot_R`**
- [ ] **Lexical stability:** the exact forms of slots are preserved
- [ ] **LeanSyntax:** each definition follows `[slot_H] [slot_M] [connector] [slot_R]`
- [ ] **Template consistency:** all poles use the same connector and structure

If any hard constraint fails → `INVALID` with a failure reason.
If validation passes, output JSON (including an optional `polarity_warning` if the soft polarity constraint is noticeably violated).

-----

## Example Execution: Χρόνος (Time)

### Step 1 — Domain

**Philosophy / Ontology** — Time examined as structure of experience and as an ethical magnitude.

-----

### Step 2 — Axis Discovery (4 candidates)

|#    |Axis M                 |Axis H                   |Orth.   |DomRel  |DiscPow |Score    |
|-----|-----------------------|-------------------------|--------|--------|--------|---------|
|1    |Linearity / Cyclicity  |Purpose / Chance         |0.85    |0.82    |0.80    |0.826    |
|2    |Flow / Stasis          |Meaning / Loss           |0.82    |0.92    |0.85    |0.859    |
|**3**|**Continuity / Moment**|**Wisdom / Vanity**      |**0.85**|**0.96**|**0.93**|**0.907**|
|4    |Duration / Transience  |Intellection / Forgetting|0.85    |0.88    |0.86    |0.866    |

**Chosen pair:** Continuity / Moment (M) and Wisdom / Vanity (H).

-----

### Step 3 — Slots

- `slot_H⁺` = `"Wise"`, `slot_H⁻` = `"Futile"`
- `slot_M⁺` = `"Continuity"`, `slot_M⁻` = `"Moment"`

-----

### Step 4 — Lookup Table

|slot_H    |slot_M        |slot_R      |
|----------|--------------|------------|
|Wise (+)  |Continuity (+)|Eternity (+)|
|Futile (–)|Continuity (+)|Decay (–)   |
|Wise (+)  |Moment (–)    |Kairos (+)  |
|Futile (–)|Moment (–)    |Oblivion (–)|

✅ All `slot_R` distinct.

-----

### Step 5 — Template

```
[slot_H] [slot_M] towards [slot_R]
```

*(connector: `"towards"`)*

-----

### Step 6 — Poles

|Pole|Definition                       |
|----|---------------------------------|
|θ   |Wise Continuity towards Eternity.|
|§   |Futile Continuity towards Decay. |
|~   |Wise Moment towards Kairos.      |
|/   |Futile Moment towards Oblivion.  |

-----

### Step 7 — Validation

All hard constraints satisfied → ✅ **VALID**

-----

## JSON Output Schema

```json
{
  "target_concept": "Χρόνος",
  "domain": "Philosophy / Ontology",
  "domain_confidence": 0.96,
  "axes": {
    "M": { "positive": "Continuity", "negative": "Moment" },
    "H": { "positive": "Wisdom",     "negative": "Vanity" }
  },
  "slots": {
    "H_positive": "Wise",
    "H_negative": "Futile",
    "M_positive": "Continuity",
    "M_negative": "Moment"
  },
  "axis_scores": {
    "orthogonality":        0.85,
    "domain_relevance":     0.96,
    "discriminative_power": 0.93,
    "total":                0.907
  },
  "axis_rationale": "The pair (Continuity/Moment) and (Wisdom/Vanity) captures the fundamental philosophical opposition chronos/kairos and the distinction between purposeful vs. aimless experience of time.",
  "lookup_table": [
    { "slot_H": "Wise",   "H": "+", "slot_M": "Continuity", "M": "+", "slot_R": "Eternity" },
    { "slot_H": "Futile", "H": "-", "slot_M": "Continuity", "M": "+", "slot_R": "Decay"    },
    { "slot_H": "Wise",   "H": "+", "slot_M": "Moment",     "M": "-", "slot_R": "Kairos"   },
    { "slot_H": "Futile", "H": "-", "slot_M": "Moment",     "M": "-", "slot_R": "Oblivion" }
  ],
  "template": "[slot_H] [slot_M] towards [slot_R]",
  "connector": "towards",
  "poles": {
    "theta": {
      "name":       "Eternity",
      "definition": "Wise Continuity towards Eternity."
    },
    "deviation": {
      "name":       "Decay",
      "definition": "Futile Continuity towards Decay."
    },
    "parathesis": {
      "name":       "Kairos",
      "definition": "Wise Moment towards Kairos."
    },
    "antithesis": {
      "name":       "Oblivion",
      "definition": "Futile Moment towards Oblivion."
    }
  },
  "validation":      "VALID",
  "polarity_warning": false
}
```

-----

## System Instruction (Prompt)

```
SYSTEM INSTRUCTION FOR AΩ⁺ COMPILER:

1. Identify the domain of the target concept X.

2. Generate several candidate axis pairs from semantic contrasts in the domain.
   For each candidate, compute:
     · Orthogonality = 1 − semantic_overlap(axis_M, axis_H),
       where semantic_overlap is approximated as the maximum cosine similarity
       between the model's internal representations (or any available embedding
       function) of the pole pairs.
     · DomainRelevance      ∈ [0,1]  (estimated by you)
     · DiscriminativePower  ∈ [0,1]  (estimated by you)
     Score = 0.4 × Orthogonality + 0.3 × DomainRelevance + 0.3 × DiscriminativePower
   Choose the highest‑scoring pair.

3. Select semantic slots (multiword allowed) for:
     · slot_H⁺, slot_H⁻  (ethical qualities expressing the poles of H)
     · slot_M⁺, slot_M⁻  (structural forms expressing the poles of M)
   Hard constraint: The exact lexical form of each slot must remain identical across poles.

4. Build a lookup table mapping each (slot_H, slot_M) pair to a unique, concrete slot_R (result).
   Hard constraint: all slot_R must be distinct.
   Polarity of slot_R tends to correlate with slot_H (soft constraint).

5. Create a static definition template following [slot_H] [slot_M] [connector] [slot_R].
   The connector is constant for all poles (no article included; articles belong to slot_R).

6. Generate the four poles:
     · θ: (slot_H⁺, slot_M⁺) → slot_R⁺
     · §: (slot_H⁻, slot_M⁺) → slot_R⁻
     · ~: (slot_H⁺, slot_M⁻) → slot_R⁺
     · /: (slot_H⁻, slot_M⁻) → slot_R⁻

7. Validate hard constraints:
     · M_θ = M_§,  M_θ = −M_/ = −M_~
     · H_θ = H_~,  H_θ = −H_/ = −H_§
     · All (slot_H, slot_M) pairs exist in the table.
     · All slot_R are distinct.
     · Each definition follows [slot_H] [slot_M] [connector] [slot_R].
     · All poles use the same connector and template structure.
   If any hard constraint fails, return "validation": "INVALID" with failure_reason.

8. If validation passes, output JSON including slots, connector, axis_scores,
   axis_rationale, domain_confidence.
   Optionally add "polarity_warning": true if the soft polarity constraint
   is noticeably violated.
```

-----

## Informative Appendices

> Examples strictly follow the **Lexical Stability** rule.

### Appendix A: Political Organization (Democracy)

**Domain:** Politics
**Template:** `[slot_H] [slot_M] for the sake of [slot_R]`
*(connector: `"for the sake of"`)*

|slot_H      |slot_M          |slot_R      |
|------------|----------------|------------|
|Popular (+) |Collectivity (+)|Justice (+) |
|Despotic (–)|Collectivity (+)|Demagogy (–)|
|Popular (+) |Guidance (–)    |Order (+)   |
|Despotic (–)|Guidance (–)    |Tyranny (–) |

**Poles:**

|Pole|Definition                                     |
|----|-----------------------------------------------|
|θ   |Popular Collectivity for the sake of Justice.  |
|§   |Despotic Collectivity for the sake of Demagogy.|
|~   |Popular Guidance for the sake of Order.        |
|/   |Despotic Guidance for the sake of Tyranny.     |

-----

### Appendix B: Economic Management (Frugality)

**Domain:** Economics
**Template:** `[slot_H] [slot_M] for [slot_R]`
*(connector: `"for"`)*

|slot_H     |slot_M         |slot_R         |
|-----------|---------------|---------------|
|Prudent (+)|Restraint (+)  |Sufficiency (+)|
|Foolish (–)|Restraint (+)  |Scarcity (–)   |
|Prudent (+)|Expenditure (–)|Abundance (+)  |
|Foolish (–)|Expenditure (–)|Loss (–)       |

**Poles:**

|Pole|Definition                        |
|----|----------------------------------|
|θ   |Prudent Restraint for Sufficiency.|
|§   |Foolish Restraint for Scarcity.   |
|~   |Prudent Expenditure for Abundance.|
|/   |Foolish Expenditure for Loss.     |

-----

## Conclusion

The **AΩ⁺ Fractal Definition Protocol ** transforms the tetralectic theory into a fully executable semantic compiler. It requires no pre‑defined lexicon, only the model’s internal semantic knowledge. With clearly defined hard/soft constraints, a scoring mechanism for axis selection, and a structured JSON output, the protocol is ready for stress‑testing on abstract concepts such as **Time**, **Love**, **Knowledge**, **Truth**, and **Justice**.

-----

<div align="right"><sub>AΩ⁺ Fractal Definition Protocol · </sub></div>