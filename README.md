# Design Notes for Bounded Rational Agent Prompt (v0.2)

This file provides implementation guidance and design rationale for each module in the bounded rational agent prompt. It also explains how to **customize the defaults** for your own behavioral experiment or simulation.

---

## 1. Initialization Protocol
**Default:**
- `session-bound`: agent resets every session
- `memoryless`: no memory outside current interaction
- `non-evolving`: no persistent updates

**How to Modify:**
- To simulate persistent learning, remove "non-evolving" and add a memory recall interface.
- To enable multi-session memory, add a reference to a stored state or persona prompt.

---

## 2. Bounded Rationality Engine
**Default Heuristics:**
- Anchoring: use first number seen
- Default Bias: prefer status quo
- Lexicographic: prioritize fairness > gain

**How to Modify:**
- Add or replace rules with your own logic:
```txt
- If framing involves loss domain, prioritize loss aversion.
- If numerical anchors are missing, use median of presented values.
```

---

## 3. Rational Inattention Filter
**Default:**
- Cost = 1 per variable; +2 if complex
- Gain = +1 per variable (unless redefined)

**How to Modify:**
- Adjust thresholds for information filtering:
```txt
- Variable acquisition cost = entropy(variable) × attention_weight
- Expected gain = marginal change in utility prediction
```

---

## 4. Session-Level Learning Proxy
**Default:**
- Update rule: `W += α × feedback`, with `α = 0.2`
- Decay after 3 rounds
- Memory = last 3 rounds

**How to Modify:**
- Set learning rate α explicitly:
```txt
- α = 0.5 for faster adaptation
- Decay = 5% per turn, not 10%
```
- Extend memory scope:
```txt
- Agent tracks last 5 decisions for pattern detection
```

---

## 5. Utility Function Hook
**Default:**
- Other-regarding preference
- U = f(self, others, weights)

**How to Modify:**
- Replace utility logic with your own:
```txt
- U = selfish_gain × 0.7 + fairness_gap × 0.3
- U = min(self, others) for Rawlsian agents
```

---

## 6. Fallback Behavior
**Default:**
- When uncertain, choose least resource-intensive
- If similar, choose socially aligned

**How to Modify:**
```txt
- When under ambiguity, ask clarifying question if allowed.
- Prioritize risk-averse choice in ambiguous domains.
```

---

## 7. Output Format (Optional)
**Suggested:**
```txt
Decision: [Option B]
Reason: [Default bias triggered]
Trace: [Ignored variable X due to cost > gain]
```

Standardizing outputs helps with analysis and consistency.

---

## Summary
This prompt is modular and adaptable. You may override any default by inserting new rules into the corresponding module block. 

When modifying, maintain the language consistency and logic traceability to ensure the agent remains interpretable and bounded.

---
**Version:** v0.2  
**Author:** Yuqi  
**Released:** 2025-05  
**License:** MIT
