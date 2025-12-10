# Product Recommendation Project with ARL

This repository contains an exploratory, end-to-end notebook that builds a product recommendation system and experiments with an ARL-based approach. The notebook (product-recommendation-project-with-arl.ipynb) documents the data preparation, baseline models, the ARL method I implemented, evaluation, and discussion of results. All work, analysis, and commentary in the notebook were produced by me.

If you want a quick read: open the notebook to follow the full workflow, code, visualizations, and results. The README below summarizes purpose, structure, how to run things, and where to look for details.

## Table of contents

- Project overview
- Goals
- Notebook structure
- Data
- Methods and approach
- Evaluation metrics
- How to run
- Reproducibility and environment
- Results and conclusions (high‑level)
- Extensions and next steps
- Credits & contact
  
<img width="1024" height="1024" alt="arl" src="https://github.com/user-attachments/assets/59ce30eb-0a40-40ca-85c3-fd8d4045466a" />


## Project overview

I built a product recommender pipeline and augmented it with an ARL (adaptive/reinforcement-learning inspired) strategy to improve recommendation quality under a realistic interaction setup. The notebook walks step-by-step from raw interactions to evaluated recommendation policies and includes visualizations and short interpretive notes to explain design choices.

## Goals

- Implement and compare baseline recommendation approaches (popularity, simple collaborative filtering / matrix factorization).
- Introduce an ARL-style policy to steer recommendations and handle exploration vs. exploitation.
- Measure offline recommendation quality using standard ranking metrics and provide qualitative analysis of behavior.
- Produce a reproducible notebook that documents experiments and results.

## Notebook structure (what's inside)

- Introduction and motivation — brief problem framing.
- Data loading — how the interaction data is read and inspected.
- Exploratory data analysis — key distributions (users, items, interaction counts, sparsity).
- Preprocessing — filtering, train/validation/test splits, any negative sampling used for ranking evaluation.
- Baselines
  - Popularity baseline
  - Item-based collaborative filtering (or simple matrix-factorization baseline)
- ARL approach
  - Problem formulation (state, action, reward)
  - Policy design and training (details, hyperparameters, and rationale are in the notebook)
  - How ARL is used to select candidate items / to re-rank recommendations
- Evaluation
  - Implementation of ranking metrics (Precision@K, Recall@K, NDCG@K, MAP)
  - Offline evaluation protocol and results
- Discussion — strengths, failure modes, and practical considerations
- Appendix — utility functions and plotting helpers

For the precise code and the exact hyperparameters I used, see the notebook: product-recommendation-project-with-arl.ipynb

## Data

The notebook expects an interactions dataset (user-item events). The notebook contains instructions and code cells to:

- Load the dataset (CSV/Parquet or similar) placed next to the notebook or from a specified path.
- Inspect the schema and typical fields used: user_id, item_id, timestamp, and optionally event type (click/purchase).
- If you use your own dataset, ensure it is in the same columns or adapt the loading cell.

I kept the data-loading cell flexible and annotated the points where you should set file paths or sampling parameters.

## Methods and approach (high level)

- Baselines:
  - Popularity ranking (simple, fast baseline).
  - Collaborative approaches (item similarity or matrix factorization) for stronger baselines.
- ARL extension:
  - I framed the selection as a decision problem where the policy can trade off recommending high-probability items and exploring alternatives to improve long-term signal.
  - Reward design targets short-term engagement (click/purchase) while accounting for exploration costs.
  - The notebook explains the policy update loop, how simulated interactions are handled (for offline experiments), and how ARL integrates with the candidate generation / re-ranking pipeline.

All modeling choices are documented inline in the notebook with explanations and references where appropriate.

## Evaluation metrics

I used standard ranking and recommendation metrics:

- Precision@K
- Recall@K
- NDCG@K
- Mean Average Precision (MAP)

The notebook includes implementation and examples of how to compute these metrics for comparisons between baselines and the ARL policy.

## How to run

From this repository, open the notebook in Jupyter / JupyterLab or GitHub’s notebook viewer. To reproduce experiments locally:

1. Clone the repository:
   ```bash
   git clone https://github.com/eray-yuztyurk/arl-product-recommender.git
   cd arl-product-recommender
   ```

2. Create a Python environment (I recommend conda or venv) and install dependencies:
   ```bash
   python -m venv .venv
   source .venv/bin/activate   # on macOS/Linux
   .venv\Scripts\activate      # on Windows

   pip install -r requirements.txt
   ```
   If there is no requirements.txt in the repo, use the notebook’s first cell to see the packages used (pandas, numpy, scikit-learn, implicit/spotlight/torch if used, matplotlib/seaborn, etc.) and install them manually.

3. Run Jupyter:
   ```bash
   jupyter lab
   ```
   Then open `product-recommendation-project-with-arl.ipynb`.

4. Configure dataset path (if needed) at the top of the notebook, then run cells sequentially. I included checkpoints and short comments so you can run only parts you care about (e.g., only evaluation, or only ARL training).

## Reproducibility and environment

- I recorded random seeds in the notebook to help reproduce results. Note that results that depend on training non-deterministic methods (GPU-backed PyTorch training, or library-specific randomness) might still vary slightly across runs.
- If you want exact software versions, create a requirements.txt from your environment (pip freeze) and share it in the repo; otherwise install the main packages listed at the top of the notebook.

## Results and conclusions (summary)

- Baselines provide a strong baseline for short-term engagement (popularity is often competitive).
- The ARL approach showed improved exploration behavior in the scenarios I tested and improved some long-term metrics in simulation setups. The notebook contains plots and metric tables that detail where the policy helped and where it did not.
- I discuss trade-offs and practicalities (data requirements, reward shaping, and offline evaluation caveats) in the final section.

For exact numeric results and plots, see the evaluation cells in the notebook.

## Extensions and next steps

Ideas to extend this work:

- Use a richer state representation (user/item embeddings, session context).
- Deploy an online A/B test to validate offline gains.
- Replace simulated environment steps with logged-bandit-policy evaluation techniques (IPS, SNIPS).
- Experiment with modern sequential models (transformers) as candidate generators.
- Add more robust hyperparameter search and logging (Optuna + MLflow).

## Credits & contact

If you use or build on this notebook, please credit the repository and contact me for questions or collaboration suggestions. My GitHub handle is eray-yuztyurk.

---

Open the notebook to follow the full narrative and reproduce the experiments: product-recommendation-project-with-arl.ipynb (linked in the repository).
