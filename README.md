# Project
## Overview
This repo contains the data and code used for the study presented in the following paper:

[*Bayesian modelling of the latent human influence field in archaeological urban sediments at Koh Ker, Cambodia*]()

## Abstract

Reconstructing the spatiotemporal trajectories of massive ancient cities remains difficult because archaeological excavation offers temporal depth but limited spatial coverage, while lidar and other survey methods map urban form without chronological depth. Moreover, ancient urban dynamics were multidimensional and indicated by many different variables. Integrating archaeological and environmental proxies indicative of these variables into a single spatiotemporal picture (the urban trajectory) is complicated by heterogeneity in data resolution, distributional form, and measurement structure. To address these limitations, we propose a new framework centred on mapping a theoretical “latent human influence field” (LHIF). This field represents the intensity of past human activity and its effects on the proxies in the sediment archive, which can be estimated by fusing proxy data in rapidly excavated, low-impact trenches. Given sufficient sampling and temporal coregistration, an estimated LHIF promises a coherent multidimensional spacetime view of ancient urbanism. It would fill the inferential gaps between lidar, survey, and excavation by estimating not only where archaeological traces occur, but how strongly human activity shaped local sediments and landscapes — a continuous field of human activity intensity and influence. To explore the potential of this framework, we conducted a pilot study at Koh Ker (Cambodia) using magnetic susceptibility and ceramic abundance data from eight test trenches. We paired continuous magnetic susceptibility scans with ceramic abundance data and fitted a Bayesian hierarchical latent-variable model that treats both proxies as noisy observations of the shared LHIF. This model synthesizes heterogeneous data channels within a unified causal framework and is fully extensible to additional sedimentary proxies, with potential application across archaeological site types and regions.

## Software
The Python scripts contained in this repository are intended for replication efforts and to improve the transparency of research. They are, of course, provided without warranty or technical support.

## Replicating the Analysis

The main analysis is a Jupyter notebook:

```text
Src/kohker_pp24.ipynb
```

The intended replication workflow is:

1. download, clone, or fork this repository;
2. recreate the Python environment from `environment.yml`;
3. run `Src/kohker_pp24.ipynb` from top to bottom without changing the internal paths.

The notebook uses paths that are relative to the repository layout. In particular, it expects the input files in `Data/` and writes derived tables and figures to `Output/`. Please do not move the notebook or change the folder names unless you also update the paths inside the notebook.

### 1. Get the Repository

Use whichever method is most familiar:

- download the repository as a ZIP file and unzip it;
- clone it with Git;
- fork it on GitHub and then clone your fork.

If using Git from the terminal, the command will look like:

```bash
git clone <repository-url>
cd kkpp24
```

Replace `<repository-url>` with the URL for this repository or your fork. After this step, your terminal should be in the repository root, the folder that contains `README.md`, `Data/`, `Output/`, `Src/`, and `environment.yml`.

### 2. Install Conda

This project uses conda to recreate the Python software environment. If conda is not already installed, install either Miniconda, Anaconda, or Mambaforge/Miniforge following the official instructions for your operating system.

After installation, open a new terminal and confirm that conda is available:

```bash
conda --version
```

Any recent conda installation should work. The commands below assume `conda`; users who prefer `mamba` can usually replace `conda` with `mamba`.

### 3. Recreate the Environment

From the repository root, create the environment:

```bash
conda env create -f environment.yml
```

This creates an environment named `kkpp24`. Activate it with:

```bash
conda activate kkpp24
```

Then register the environment as a Jupyter kernel:

```bash
python -m ipykernel install --user --name kkpp24 --display-name "Python (kkpp24)"
```

This makes the environment selectable from notebook interfaces such as JupyterLab, classic Jupyter Notebook, or VS Code.

### 4. Run the Notebook Interactively

For users who want to watch progress cell by cell, an interactive notebook interface is recommended.

One terminal-based route is:

```bash
conda activate kkpp24
jupyter lab
```

Then open:

```text
Src/kohker_pp24.ipynb
```

Select the `Python (kkpp24)` kernel if prompted, then run the notebook from top to bottom.

The same basic procedure works in VS Code:

1. open the repository folder;
2. open `Src/kohker_pp24.ipynb`;
3. select the `Python (kkpp24)` kernel;
4. run all cells.

If the notebook cannot find files such as `../Data/MagSusMap.xlsx`, check the working directory used by your notebook interface. The notebook is written with paths relative to `Src/`, so `../Data/` should point to the repository's `Data/` folder and `../Output/` should point to the repository's `Output/` folder. Running the notebook in place from `Src/kohker_pp24.ipynb` should preserve this structure.

### 5. Run the Notebook from the Terminal

The notebook can also be executed non-interactively from the repository root:

```bash
conda activate kkpp24
jupyter nbconvert \
  --to notebook \
  --execute Src/kohker_pp24.ipynb \
  --inplace \
  --ExecutePreprocessor.kernel_name=kkpp24 \
  --ExecutePreprocessor.timeout=-1
```

This updates the notebook in place. It is useful for fully scripted replication, but it does not show live cell-by-cell progress in an editor. If you want to monitor progress visually, use JupyterLab, Jupyter Notebook, or VS Code instead.

### 6. Expected Outputs

The notebook writes processed data and figures to `Output/`, including files such as:

- `Output/magsus_scans.csv`
- `Output/c14_data.csv`
- `Output/posterior_summary_sim_full.csv`
- `Output/posterior_summary_sim_mag.csv`
- `Output/posterior_summary_kkpp24_full.csv`
- `Output/posterior_summary_kkpp24_mag.csv`
- posterior, diagnostic, and comparison plots as `.png` and `.pdf` files

Running the notebook may overwrite existing files in `Output/`. This is expected.

### Practical Notes and Common Problems

- The notebook includes Bayesian model fitting with PyMC. A full top-to-bottom run can take a long time, especially on machines with fewer CPU cores.
- The notebook currently runs four model fits: two simulated-data fits and two Koh Ker data fits. These are the slowest cells.
- The first notebook cell sets `PYTENSOR_FLAGS` before importing PyMC/PyTensor. Run the notebook from a fresh kernel so this setting is applied before PyMC is imported.
- The environment file installs `chronocluster` from `https://github.com/wccarleton/chronocluster` because the radiocarbon calibration step imports `chronocluster.distributions`. This requires internet access when the environment is first created.
- If package solving is slow, try `mamba env create -f environment.yml` instead of `conda env create -f environment.yml`.
- If you rerun the notebook after a failed or interrupted attempt, restart the kernel and run from the first cell.
- The notebook writes outputs using the existing repository structure. Do not change the internal paths unless you are intentionally modifying the analysis.

## License

Shield: [![CC BY 4.0][cc-by-shield]][cc-by]

This work is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
