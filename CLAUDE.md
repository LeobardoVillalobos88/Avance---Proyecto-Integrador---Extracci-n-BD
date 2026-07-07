# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

University coursework (UTEZ) for **Extracción del conocimiento en Bases de datos**. It is a collection of scikit-learn classification exercises, each paired with a LaTeX report that documents the notebook's methodology and results. There is no shared build system or package — treat each task folder as an independent deliverable.

Author/context: Leobardo Daniel Bertadillo Villalobos, group 9C. The `KNN_SVM/` exercises are individual work; the `Integradora_IA_Estudiantes/` deliverable is a **team** activity (5 members, listed on its report cover). Everything is in **Spanish** (prose, notebook comments, LaTeX). Notebook comments are intentionally written without accents and must stay simple (no emojis, no heavy jargon); the LaTeX prose uses full accents.

The published copy of this repo lives at the GitHub remote *Avance---Proyecto-Integrador---Extracci-n-BD* (owner `LeobardoVillalobos88`), shared so teammates can clone it.

## Layout

- `Integradora_IA_Estudiantes/` — **the main team deliverable** (KDD study, see its own section below). Self-contained: notebook, its own copy of the dataset, and a `latex/` report.
- `ai_student_impact_dataset.csv` — the 50k-row source dataset at the repo root (also copied inside `Integradora_IA_Estudiantes/` so the notebook can load it by bare filename).
- `KNN_SVM/T2_KNN_SVM_vehiculos.ipynb` — **T2**: KNN vs. SVM on `vehicle.csv` (Statlog vehicle-silhouette dataset, 18 numeric features + integer `target`). Metrics are computed **by hand from the confusion matrix**, not via `sklearn.metrics` — this is the point of the exercise, so preserve that approach.
- `KNN_SVM/Optimización de modelos/T4_Optimizacion_modelos.ipynb` — **T4**: hyperparameter optimization (KNN, SVM, DecisionTree) on `Iris.csv` using `GridSearchCV` / `cross_val_score`, comparing base vs. optimized models.
- `KNN_SVM/ejemplos_scripts/` — loose teaching-example notebooks (KNN, decision tree on Titanic, Iris comparison). These reference CSVs by relative path that are **not in the repo** (`Social_Network_Ads.csv`, `titanic-train.csv`); place the CSV next to the notebook before running. `U3_Arbol_Decision.ipynb` shells out to Graphviz (`!dot -Tpng ...`), requiring a `dot` binary.
- `latex/` and `Optimización de modelos/latex/` — one LaTeX report per task. Each has a `main.tex` root, an `img/` folder of figures **exported manually from the corresponding notebook** (the notebooks do not `savefig`), and `bibs/referencias.bib`.

Each notebook loads its CSV by bare relative filename (`pd.read_csv("vehicle.csv")`, `pd.read_csv("Iris.csv")`, `pd.read_csv("ai_student_impact_dataset.csv")`), so **run Jupyter from the folder that contains the CSV**, not from the repo root.

## The integradora deliverable (`Integradora_IA_Estudiantes/`)

Team KDD study: predict a student's **`Burnout_Risk_Level`** (Low/Medium/High — 3-class classification) from AI-usage and study habits, comparing **Decision Tree, KNN, and SVM**. The report is organized by the five KDD phases (selección, preprocesamiento, transformación, minería, evaluación).

Key facts and decisions baked into the code and report — keep them consistent if you touch either:
- **Single global 80/20 stratified split** (40k train / 10k test). Decision Tree and KNN train on the full 40k; **SVM tunes/trains on a stratified 10k subsample** of the train set (RBF SVM on 40k takes ~4 min per fit, so a full grid search is infeasible). All three are evaluated on the **same 10k test set** for a fair comparison. This subsample choice is stated explicitly in the report.
- Honest headline: accuracy tops out around **~52%** across all three models (vs. 33% random). The dataset only partially predicts burnout; `Weekly_GenAI_Hours` carries ~80% of the tree's feature importance. Do not inflate these numbers.
- Best configs found: tree `max_depth=6, min_samples_leaf=50`; KNN `k=101`; SVM `kernel=rbf, C=1, gamma=scale`. Everything uses `random_state=42`, so results are deterministic and the notebook's numbers match the report's tables exactly — **if you change the pipeline, re-run and update both**.
- Metrics (precision/recall/specificity/F1) are computed **by hand from the confusion matrix** (one-vs-rest), same as the KNN_SVM exercises — preserve that.
- Report style rules (user-mandated, differ from the KNN_SVM reports): **no em dashes (`---`), no italics (`\textit`/`\emph`), no bibliography/citations**, and the whole document is set in an Arial-like sans font via `\usepackage[scale=0.94]{tgheros}` + `\familydefault=\sfdefault`. Math-mode symbols staying italic is fine.
- Figures in `latex/img/` are produced by the notebook's plotting cells (same code, saved as PNG). Regenerate them from the notebook if a figure changes.

### Python environment gotcha (important)

This machine has **two Python installs**: **3.14** (`python` on PATH — has `scikit-learn`, `pandas`, `seaborn`) and **3.12** (has the `jupyter`/`nbconvert` CLIs but **no** sklearn). To run the notebook headlessly you must use the 3.14 kernel, registered as **`py314-integradora`**:

```sh
cd Integradora_IA_Estudiantes
python -m nbconvert --to notebook --execute --inplace \
  --ExecutePreprocessor.kernel_name=py314-integradora \
  --ExecutePreprocessor.timeout=1200 Integradora_IA_Estudiantes.ipynb
```

The default `python3` kernelspec points at 3.12 and will fail with `ModuleNotFoundError: sklearn`. A full execution takes ~6–8 min (the SVM grid search is the slow part). The notebook's saved `kernelspec` is the generic `python3` so it stays portable for teammates.

## Running the notebooks

Stack: `scikit-learn`, `pandas`, `numpy`, `matplotlib`, `seaborn`.

```sh
cd KNN_SVM            # or "KNN_SVM/Optimización de modelos"
jupyter lab           # or: jupyter notebook
```

Both task notebooks follow the same pipeline: load → inspect classes → `train_test_split` → `StandardScaler` (KNN and SVM are distance-based, so scaling is required) → train → confusion matrix → metrics. When editing, keep this ordering and the numbered `## N.` markdown section headers intact, since the LaTeX reports cross-reference them.

## Building the LaTeX reports

Run from inside the report's own `latex/` directory (the `.fdb_latexmk` files indicate `latexmk` is the toolchain; the bibliography needs a bibtex pass):

```sh
latexmk -pdf main.tex
# manual equivalent:
pdflatex main && bibtex main && pdflatex main && pdflatex main
```

Conventions:
- Spanish babel (`spanish,mexico`), UTF-8. Some `.tex` files carry a UTF-8 BOM — preserve it.
- `main.tex` defines the institutional color palette and custom `tcolorbox` environments near the top; reuse the existing boxes and colors rather than introducing new ones.
- Build artifacts (`.aux`, `.bbl`, `.blg`, `.fls`, `.fdb_latexmk`, `.log`, `.out`, `.toc`, `.pdf`) are checked in — regenerate them, don't hand-edit.
- Figures live in `img/`. If a report figure needs updating, re-export it from the notebook cell that produced it (there is no automated figure pipeline).

## Note on the stale file

`KNN_SVM/CLAUDE.md` describes an old differential-equations report and no longer matches the repository — prefer this root file.
