# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

University coursework for **Ingeniería en Desarrollo y Gestión de Software (UTEZ)**. Despite the folder name `KNN_SVM`, it holds two unrelated bodies of work:

1. **`latex/`** — A LaTeX report for *Matemáticas para Ingeniería II* on differential equations (Spanish). This is the main, polished deliverable.
2. **`ejemplos_scripts/`** + **`vehicle.csv`** — Standalone scikit-learn teaching notebooks (KNN, SVM, decision trees). These are loose class exercises, not a packaged application.

There is no shared build system tying the two together; treat them as separate projects in one folder.

## LaTeX report (`latex/`)

`main.tex` is the root document. It assembles content via `\input`:
- `portada/title.tex` — title page (institutional data defined as `\newcommand`s: programa, materia, profesor, alumnos).
- `introduccion/`, `unidades/unidad1.tex`–`unidad3.tex`, `conclusion/` — body, one `\section` per file.
- `bibs/referencias.bib` — bibliography, cited with `apalike` style.

Key conventions:
- Spanish (`babel` with `spanish,mexico`), UTF-8 input. Several `.tex` files start with a UTF-8 BOM — preserve it when editing.
- `main.tex` defines custom `tcolorbox` environments used throughout the units: `definicion`, `teorema`, `ejercicio`, `problema`, `resultado`, `nota`, `tecnica`. Reuse these rather than introducing new boxes, and keep the institutional color palette (`azulutez`, `grisinstitucional`, etc.) defined near the top of `main.tex`.

Build (run from `latex/`). The presence of `main.fdb_latexmk` indicates `latexmk` is the toolchain; bibliography requires a bibtex pass:
```sh
latexmk -pdf main.tex
```
Manual equivalent if `latexmk` is unavailable:
```sh
pdflatex main && bibtex main && pdflatex main && pdflatex main
```
Generated artifacts (`.aux`, `.bbl`, `.blg`, `.fls`, `.fdb_latexmk`, `.log`, `.out`, `.toc`, `.synctex.gz`, `.pdf`) are committed in `latex/`; regenerate rather than hand-edit them.

## ML notebooks (`ejemplos_scripts/`)

Jupyter notebooks using `scikit-learn`, `pandas`, `matplotlib`, `seaborn`:
- `K-NN.ipynb` — KNN on inline data, plus an SVM section.
- `U3_Arbol_Decision.ipynb` — decision tree on Titanic; exports `titanic.dot` and shells out to Graphviz (`!dot -Tpng ...`), so a `dot` binary is required for that cell.
- `U3_KNN_SVM_ArbolDecision_Iris.ipynb` — full KNN/SVM/tree comparison on Iris with metrics.

Important: the notebooks read CSVs by relative path that are **not in the repo** (`Iris.csv`, `Social_Network_Ads.csv`, `titanic-train.csv`). To run a notebook, place the expected CSV next to it first. `vehicle.csv` at the repo root (18 numeric features + integer `target`) is a dataset present in the repo but not yet referenced by any notebook.

Run with:
```sh
jupyter notebook   # or: jupyter lab
```
