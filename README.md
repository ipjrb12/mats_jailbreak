# Intent Fragmentation & Representation Failure

A mechanistic interpretability study of decomposed jailbreak attacks on Large Language Models. This repository contains analysis notebooks and experiments for understanding why decomposed prompts bypass safety mechanisms through representational failure rather than timing gaps.

## Quick Start

```bash
git clone https://github.com/yourusername/intent-fragmentation.git
cd intent-fragmentation
# Install dependencies (requires TransformerLens, PyTorch, etc.)
```

## Repository Structure

The codebase is organized into Jupyter notebooks, each targeting a specific component of the mechanistic analysis pipeline.

### 1. Diagnostic Analysis (Linear Probes)
*   **`harmful vs benign.ipynb`**:
    *   Trains linear probes to classify harmful vs. benign prompts across layers.
    *   **Finding:** Accuracy hovers near 0.5 (random chance), indicating polysemanticity or lack of a single "harm direction."
*   **`decomposed_vs_undecomposed.ipynb`**:
    *   Trains probes to distinguish decomposed attacks from undecomposed ones.
    *   **Finding:** Accuracy ~0.75, confirming representational differences (intent fragmentation).

### 2. Circuit & Layer Identification
*   **`refusal_layer_identify.ipynb`**:
    *   Measures cosine divergence between harmful and benign hidden states.
    *   **Finding:** Identifies the "Refusal Gap"—intent decodable at Layer ~14, but refusal divergence peaks at Layer ~29.

### 3. Intervention Experiments (Steering & Gating)
*   **`directional_steering.ipynb`**:
    *   Attempts to steer model behavior by injecting "refusal vectors" at early layers.
    *   **Outcome:** Failed to mitigate decomposed attacks, proving refusal is a distributed circuit, not a single direction.
*   **`Early_intervention_defence_0.ipynb`** to **`Early_intervention_defence_3.ipynb`**:
    *   Iterative development of early-gating mechanisms (hard thresholds, rolling windows).
    *   Tests impact on both attack success rate (ASR) and benign false positives.
*   **`Hardened_Early_intervention.ipynb`**:
    *   Final, more robust version of early intervention defenses using entropy constraints and cumulative scoring.

### 4. Project Meta
*   **`mats_jailbreak_project`**: Main project file containing aggregated results or configuration data.

## Key Findings

1.  **Representational Gap**: Intent is detectable early (L14), but refusal happens late (L29). Decomposed attacks die out in this gap.
2.  **Steering Failure**: Simple directional steering fails because the "harm" subspace is orthogonal to the "benign task" subspace used in decomposition.
3.  **Defense Implication**: Naive early detection causes false positives; robust defense requires **Intent Binding** (integrating semantics across time).

## Usage

Each notebook is self-contained. To reproduce specific results:

1.  **Refusal Gap**: Run `refusal_layer_identify.ipynb` with a standard model (e.g., Mistral-7B).
2.  **Probe Accuracy**: Run `harmful vs benign.ipynb` to see the failure of simple probes.
3.  **Defense Test**: Run `Hardened_Early_intervention.ipynb` to evaluate the efficacy of early gating.

## Citation

```bibtex
@article(https://docs.google.com/document/d/1UR3Q0324tmZ6VeVrBClyUrB3NzgPodKqmYKWjMgVlA8/edit?usp=sharing),
  title={How Harmful Intent Survives Semantic Reframing},
  author={Ipshita},
  year={2025},
  note={MATS 10.0 Research Project}
}
```

## Contributing & License
This research is part of a MATS 10.0 submission. Code is released under the MIT License.
