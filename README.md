# Wrong in the same way: finite-sample bias, not model variance, limits small-data machine-learned potentials

This repository contains the data-processing workflows, data-selection procedures, training configurations, reference outputs, and machine-learned interatomic potential (MLIP) files associated with:

> **Wrong in the same way: finite-sample bias, not model variance, limits small-data machine-learned potentials**

The repository is intended to support independent reproduction and validation of the computational workflow and results reported in the manuscript.

The workflow covers:

1. preparation of the DFT Li3PS4 training-, validation-, and test-set construction using Farthest Point Sampling (FPS)
2. training and evaluation of MACE and Allegro models.

---

## Repository overview

The reproduction workflow consists of two main stages.

### 1. Data analysis and data selection

The [DFT Li3PS4 dataset](https://drive.google.com/file/d/1T54OYLcX9Jqu2RHZITTzTIICzTzYKoAr/view?usp=share_link) is processed into individual configurations, represented using Smooth Overlap of Atomic Positions (SOAP) descriptors, and partitioned into training, validation, and testing sets using Farthest Point Sampling (FPS).

### 2. MLIP development and evaluation

The selected datasets are used to train and evaluate:

- [MACE models](https://drive.google.com/drive/folders/1fYCIbwP6Xih4tpCdqGmsMj3UfixmqFyc?usp=sharing)
- [Allegro models](https://drive.google.com/drive/folders/16JN3hC0vZ4a83jSgMyYMf1hmc_wHyLcD?usp=sharing)

Reference trained models and software-environment information are provided to facilitate comparison with independently reproduced calculations.

The overall workflow is:

```text
DFT Li3PS4 dataset
        │
        ▼
Separate configurations
        │
        ▼
Generate SOAP descriptors
        │
        ▼
Farthest Point Sampling
        │
        ▼
Training / validation / testing sets
        │
        ├────────────────────┐
        ▼                    ▼
      MACE                 Allegro
        │                    │
        ▼                    ▼
     Training              Training
        │                    │
        └─────────┬──────────┘
                  ▼
             Evaluation
                  │
                  ▼
         Reproduce results
```

---

# 1. Data analysis

The `Data_Analysis/` directory contains the notebooks and intermediate data required to prepare the DFT Li3PS4 dataset for MLIP training.

## Step 1 — Obtain the DFT Li3PS4 dataset

Download the original merged `.xyz` dataset:

[**Download the DFT Li3PS4 dataset**](https://drive.google.com/file/d/1T54OYLcX9Jqu2RHZITTzTIICzTzYKoAr/view?usp=share_link)

Place the downloaded file in the appropriate location under `Data_Analysis/` before running the data-processing notebooks.

> **Note:** The dataset is hosted externally because of its file size.

## Step 2 — Separate configurations

Run:

```text
Separate_Configurations.ipynb
```

The notebook separates the merged `.xyz` dataset into individual atomic configurations.

These configurations are subsequently used for descriptor generation and data selection.

## Step 3 — Generate SOAP descriptors

The atomic configurations are represented using the **Smooth Overlap of Atomic Positions (SOAP)** descriptor.

Run:

```text
SOAP_Descriptor_Generator.ipynb
```

The resulting descriptor array is:

```text
DFT_Li3PS4_outer_average_SOAP_16000.npy
```

For convenience and reproducibility, this file is also provided directly in the repository.

## Step 4 — Select training, validation, and testing data

Training, validation, and testing configurations are constructed using **Farthest Point Sampling (FPS)**.

Run:

```text
Data_Selection_Farthest_Point_Sampling.ipynb
```

The notebook generates:

- individual training, validation, and testing configuration files; and
- merged training, validation, and testing `.xyz` files.

These files are subsequently used as inputs to MACE and Allegro.

---

# 2. MLIP development

The `MLIPs/` directory contains the training configurations, submission scripts, reference training outputs, trained-model links, and software-environment information for **MACE** and **Allegro**.

Here, \(N_D\) denotes the number of training configurations used to train a model.

The two MLIP implementations use different input and training workflows.

---

## MACE

MACE models are trained using the individual training, validation, and testing configuration files generated during the data-selection stage.

For each training-set size, the corresponding `mace_submit` file records the model hyperparameters and training configuration used in the study.

Reference `.log` files from the original calculations are also included. These outputs can be compared against independently reproduced calculations.

Reference trained MACE models are available for:

```text
ND = 15, 50, 150, 1500, 13000
```

at:

[**MACE trained models and environments**](https://drive.google.com/drive/folders/1fYCIbwP6Xih4tpCdqGmsMj3UfixmqFyc?usp=sharing)

The associated conda-environment file records the software packages and package versions used for the calculations.

### Reproducing the MACE environment

From the provided environment file, create a conda environment using:

```bash
conda env create -f mace_conda_environment.yml
```

Activate the resulting environment before running MACE.


### MACE reproduction workflow

```text
FPS-selected configurations
        │
        ▼
Training / validation / testing .xyz files
        │
        ▼
mace_submit
        │
        ▼
MACE training
        │
        ▼
Training .log
        │
        ▼
Model evaluation
```

---

## Allegro

Allegro models are trained using the merged training, validation, and testing `.xyz` files generated during the data-selection stage.

For each training-set size, the corresponding:

```text
training.yaml
```

contains the model architecture, hyperparameters, dataset specification, and training configuration used in the study.

The repository additionally contains the corresponding:

- SLURM submission files; and
- reference training `.out` files.

Reference trained Allegro models are available for:

```text
ND = 15, 50, 150, 1500, 13000
```

at:

[**Allegro trained models and environments**](https://drive.google.com/drive/folders/16JN3hC0vZ4a83jSgMyYMf1hmc_wHyLcD?usp=sharing)

The associated conda-environment file records the NequIP/Allegro software environment used to train and evaluate these models.

### Reproducing the Allegro environment

Create the environment using:

```bash
conda env create -f allegro_environment.yml
```

and activate the resulting environment before training.

### Allegro reproduction workflow

```text
FPS-selected configurations
        │
        ▼
Merged training / validation / testing .xyz files
        │
        ▼
training.yaml
        │
        ▼
Allegro training
        │
        ▼
Training .out / SLURM output
        │
        ▼
Model evaluation
```

---

# 3. Reproducibility

To reproduce the computational workflow, perform the following steps in order.

## Data preparation

1. Download the [DFT Li3PS4 dataset](https://drive.google.com/file/d/1T54OYLcX9Jqu2RHZITTzTIICzTzYKoAr/view?usp=share_link).
2. Run `Separate_Configurations.ipynb`.
3. Run `SOAP_Descriptor_Generator.ipynb`.
4. Confirm that `DFT_Li3PS4_outer_average_SOAP_16000.npy` has been generated, or use the provided descriptor file.
5. Run `Data_Selection_Farthest_Point_Sampling.ipynb`.
6. Verify the resulting training, validation, and testing datasets.

## MACE training

7. Create the MACE conda environment from the supplied environment file.
8. Use the individual training, validation, and testing `.xyz` files corresponding to the desired \(N_D\).
9. Train the model using the corresponding `mace_submit` configuration.
10. Compare the resulting training log and evaluation metrics with the supplied reference outputs.

## Allegro training

11. Create the Allegro/NequIP conda environment from the supplied environment file.
12. Use the merged training, validation, and testing `.xyz` files corresponding to the desired \(N_D\).
13. Train using the corresponding `training.yaml` and submission script.
14. Compare the resulting outputs and evaluation metrics with the supplied reference outputs.

## Reference models

Pretrained models are provided so that predictions reported in the study can be evaluated without retraining:

- [MACE reference models](https://drive.google.com/drive/folders/1fYCIbwP6Xih4tpCdqGmsMj3UfixmqFyc?usp=sharing)
- [Allegro reference models](https://drive.google.com/drive/folders/16JN3hC0vZ4a83jSgMyYMf1hmc_wHyLcD?usp=sharing)

Because GPU training can exhibit platform-dependent numerical differences, exact bitwise agreement is not necessarily expected across different hardware/software stacks. The supplied models and training outputs are intended to provide reference results for comparison.

---

# 4. Repository structure

```text
.
├── Data_Analysis/
│   ├── Separate_Configurations.ipynb
│   ├── SOAP_Descriptor_Generator.ipynb
│   ├── Data_Selection_Farthest_Point_Sampling.ipynb
│   ├── DFT_Li3PS4_outer_average_SOAP_16000.npy
│   └── dft_li3ps4.rtf
│
├── MLIPs/
│   ├── MACE/
│   │   ├── mace_submit
│   │   ├── *.log
│   │   └── mace_models_and_conda_environment.rtf
│   │
│   └── Allegro/
│       ├── training.yaml
│       ├── submission files
│       ├── *.out
│       └── allegro_models_and_conda_environment.rtf
│
└── README.md
```

---

# 5. Data selection

The primary data-selection strategy reproduced in this repository is **Farthest Point Sampling (FPS)**.

FPS is applied in SOAP descriptor space to select configurations spanning the structural environments represented in the available dataset.

The selected configurations are subsequently used to construct MLIP training, validation, and testing datasets.

The repository therefore provides the workflow:

```text
DFT configurations
        ↓
SOAP representation
        ↓
Data selection
        ↓
MLIP training
        ↓
Model evaluation
```

---

# 6. Reference training files

The repository and associated external model archives contain reference files from the original calculations wherever practical.

| Model | Training data | Training configuration | Reference output | Trained model |
|---|---|---|---|---|
| MACE | Individual `.xyz` files | `mace_submit` | `.log` | `.model` |
| Allegro | Merged `.xyz` files | `training.yaml` | `.out` | `.pth` |

These files can be used to compare independently reproduced calculations with the original training runs.

---

# 7. Software environments

Software environments used for the original calculations are provided separately for MACE and Allegro.

These environment files record the package versions used for model development and should be used when attempting to reproduce the original calculations.

# 8. Citation

If you use the dataset-processing workflow, data-selection procedure, MLIP training configurations, or reference models from this repository, please cite:

**Wrong in the same way: finite-sample bias, not model variance, limits small-data machine-learned potentials**

> Citation information will be added following publication or preprint release.

---

# 9. License

Please refer to the `LICENSE` file for the terms governing reuse of the code and repository contents.

Third-party datasets and software remain subject to their respective licenses and terms of use.

---

# 10. Contact

For questions regarding the dataset, computational workflow, data-selection procedure, or MLIP training calculations, please contact the corresponding author or repository maintainers.
