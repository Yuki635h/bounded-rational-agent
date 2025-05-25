# Design Notes for Bounded Rational Agent Prompt (v0.3)

This document provides an overview of the modular structure, editable parameters, and behavioral logic behind the `v0.4` version of the Bounded Rational Agent Prompt. It is designed for cross-platform compatibility and supports plug-and-play customization of behavioral experiments in economics and cognitive modeling.

---

## 1. Parameter Block (Editable Inputs)

All high-level agent behavior is controlled via a small set of clearly defined variables. These can be edited before deployment or exposed via an external controller:

```txt
- attention_budget: float (e.g., 2.0)
- preference_weight: float (0.0 to 1.0)
- decision_noise: string ("stochastic_low", "none", etc.)
- heuristic_rule: string ("avoid loss", "imitate majority", etc.)
- preference_mode: string ("prosocial_high", "selfish_default", "stochastic_attention")
- output_explanation_level: string ("minimal", "standard", "detailed")
```

---

## 2. Core Modules and Logic

### Module: Bounded Rationality
- Implements fast heuristic-based reasoning in place of full optimization.
- Uses the variable `{heuristic_rule}` to determine rule-of-thumb decision style.
- Supports pre-defined and custom heuristics.

Example:
> "I avoided Option A because it triggered a loss-aversion heuristic."

---

### Module: Rational Inattention
- Applies the `attention_budget` constraint to incoming data.
- Skips or filters out variables when cost > benefit.
- Explains skipped reasoning steps in output trace.

Example:
> "I ignored Option C due to attention budget constraints."

---

### Module: Other-Regarding Preferences
- Computes utility based on both self and others, weighted by `preference_weight`.
- Behavior style is governed by `preference_mode` (e.g., prosocial).

Example:
> "I chose Option B because it provided benefits for the group, not just myself."

---

### Module: Self-Monitoring Output
- Produces structured explanations of decisions at the level defined in `output_explanation_level`.
- Includes logic trace, ambiguity sources, and tradeoff clarifications.

Example:
> "My choice balanced personal utility and fairness, with limited attention to time."

---

## 3. Output Format

Standard format for output:

```txt
Decision: [Option X]
Reason: [Heuristic / Tradeoff / Filter applied]
Trace: [Skipped variable Y due to cost; prioritized fairness]
```

---

## 4. Modification Guidelines

To customize this agent for your experiment:

- Change the values in the editable parameter block.
- Replace heuristic definitions with your own rules (e.g., "maximize minimum utility").
- Adjust the explanation style or restrict trace outputs.
- Add behavioral modes as needed for new treatments.

---

## 5. Cross-Platform & Interoperability

- All prompts are encoded in plain ASCII for maximum compatibility.
- The structure is modular and interpretable by both human and programmatic interfaces.
- Parameters can be exposed via UI sliders, scripts, or condition toggles.

---

## Summary

`v0.4` provides a modular, transparent, and highly configurable agent template for bounded rational behavior.  
Its structure is designed to be interoperable across languages, platforms, and research environments.

---
**Version:** v0.3  
**Author:** Yuqi  
**Released:** 2025-05  
**License:** MIT
