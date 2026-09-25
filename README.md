# Wrong in the same way: finite-sample bias, not model variance, limits small-data machine-learned potentials

This repository contains the data-processing workflows, training configurations, analysis scripts, and machine-learned interatomic potential (MLIP) files used in:

> **Wrong in the same way: finite-sample bias, not model variance, limits small-data machine-learned potentials**

The repository is designed to support **reproduction and validation of the results presented in the manuscript**. It includes the workflows used to prepare the training, validation, and testing datasets and to train the MACE and Allegro MLIPs.

---

## Repository Overview

The reproduction workflow consists of two main stages:

1. **Data Analysis and Data Selection**
   Prepare the [DFT Li3PS4 dataset](https://drive.google.com/file/d/1T54OYLcX9Jqu2RHZITTzTIICzTzYKoAr/view?usp=share_link), generate SOAP descriptors, and construct the training, validation, and testing sets using Farthest Point Sampling (FPS).

2. **MLIP Development and Evaluation**
   Train and evaluate MACE and Allegro models using the selected datasets and the provided hyperparameters.

A schematic overview of the workflow is:

```text
DFT Li3PS4 Dataset
        │
        ▼
Separate Configurations
        │
        ▼
SOAP Descriptor Generation
        │
        ▼
Farthest Point Sampling
        │
        ▼
Training / Validation / Testing Sets
        │
        ├───────────────┐
        ▼               ▼
      MACE           Allegro
        │               │
        ▼               ▼
   Training &        Training &
    Evaluation        Evaluation
        │               │
        └───────┬───────┘
                ▼
        Reproduce Results
```

---

# 1. Data Analysis

The `Data_Analysis` folder contains the notebooks and data required to prepare the DFT Li3PS4 dataset for MLIP training.

## Step 1 — Obtain the DFT Li3PS4 Dataset

The original DFT Li3PS4 dataset is provided as a merged `.xyz` file.

To obtain the dataset:

1. Open `dft_li3ps4.rtf`.
2. Follow the Google Drive link provided in the file.
3. Download the [DFT Li3PS4 dataset](https://drive.google.com/file/d/1T54OYLcX9Jqu2RHZITTzTIICzTzYKoAr/view?usp=share_link).
4. Place the downloaded dataset in the appropriate `Data_Analysis` directory.

> **Note:** The dataset is provided externally because of its file size. The repository includes the information necessary to access the dataset.

---

## Step 2 — Separate Configurations

The downloaded dataset contains multiple configurations in a single file.

Use:

```text
Separate_Configurations.ipynb
```

to separate the merged dataset into individual configurations.

The resulting configuration files are used as input for SOAP descriptor generation and data selection.

---

## Step 3 — Generate SOAP Descriptors

The molecular and atomic environments are represented using the **Smooth Overlap of Atomic Positions (SOAP)** descriptor.

Run:

```text
SOAP_Descriptor_Generator.ipynb
```

This notebook generates the SOAP representation of the DFT Li3PS4 configurations.

The resulting descriptor file is:

```text
DFT_Li3PS4_outer_average_SOAP_16000.npy
```

For convenience and reproducibility, this `.npy` file is already provided in the `Data_Analysis` folder.

---

## Step 4 — Select Training, Validation, and Testing Data

The training, validation, and testing configurations are selected using **Farthest Point Sampling (FPS)**.

Run:

```text
Data_Selection_Farthest_Point_Sampling.ipynb
```

The notebook performs the data-selection procedure and generates both:

* individual training, validation, and testing configuration files; and
* merged training/validation/testing configuration files.

These outputs are subsequently used for MACE and Allegro training.

---

# 2. MLIP Development

The `MLIPs` folder contains the model-training workflows, configuration files and links for [**MACE**](https://drive.google.com/drive/folders/1fYCIbwP6Xih4tpCdqGmsMj3UfixmqFyc?usp=sharing) and [**Allegro**](https://drive.google.com/drive/folders/16JN3hC0vZ4a83jSgMyYMf1hmc_wHyLcD?usp=sharing) models and conda environments.

The two MLIP implementations use slightly different input formats.

---

## MACE

MACE models are trained using **individual training, validation, and testing configuration files**.

For each training-set size, the corresponding `mace_submit` file contains the hyperparameters and training configuration used in the study.

There are corresponding training `.log` files. These logs can be used to verify that the reproduced training procedure is consistent with the original calculations.

We also include Google Drive link to access our already trained [MACE models](https://drive.google.com/drive/folders/1fYCIbwP6Xih4tpCdqGmsMj3UfixmqFyc?usp=sharing) at ND={15, 50, 150 and 1500}. You can find the conda environments which show the version of MACE that we used to develop our models.

### MACE reproduction workflow

```text
FPS-selected configurations
        │
        ▼
Training / Validation / Testing .xyz files
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

Allegro models are trained using the **merged training, validation, and testing configuration files** generated during the data-selection stage.

For each training-set size, the corresponding `training.yaml` file contains the hyperparameters and training configuration used in the study.

The repository also includes:

* submission files; and
* training `.out` files.

These files provide the information necessary to reproduce and validate the Allegro training runs.

We also include Google Drive link to access our already trained [Allegro models](https://drive.google.com/drive/folders/16JN3hC0vZ4a83jSgMyYMf1hmc_wHyLcD?usp=sharing) at ND={15, 50, 150 and 1500}. You can find the conda environments which show the version of MACE that we used to develop our models.

### Allegro reproduction workflow

```text
FPS-selected configurations
        │
        ▼
Merged training / validation / testing files
        │
        ▼
training.yaml
        │
        ▼
Allegro training
        │
        ▼
Training .out / submission files
        │
        ▼
Model evaluation
```

---

# 3. Reproducibility

To reproduce the results, follow the workflow below in order.

### Data Preparation

1. Download the DFT Li3PS4 dataset using `dft_li3ps4.rtf`.
2. Run `Separate_Configurations.ipynb`.
3. Run `SOAP_Descriptor_Generator.ipynb`.
4. Verify that `DFT_Li3PS4_outer_average_SOAP_16000.npy` has been generated, or use the provided file.
5. Run `Data_Selection_Farthest_Point_Sampling.ipynb`.

### MLIP Training

6. For **MACE**, use the individual training/validation/testing files and the corresponding `mace_submit` file.
7. For **Allegro**, use the merged configuration files and the corresponding `training.yaml`.
8. Compare the reproduced training logs and outputs with the provided reference files.
9. Evaluate the trained models using the provided analysis workflows.
10. Compare the reproduced models with the provided reference models.

---

# 4. Repository Structure

The repository is organized approximately as follows:

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
│   │   └── *.log
|   |   └── mace_models_and_conda_environment 
│   │
│   └── Allegro/
│       ├── training.yaml
│       ├── submission files
│       └── *.out
|   |   └── allegro_models_and_conda_environment 
│
└── README.md
```

The exact organization may vary depending on the training-set size and computational run.

---

# 5. Data Selection

The primary data-selection strategy reproduced in this repository is **Farthest Point Sampling (FPS)**.

FPS is applied to the SOAP representation of the configurations to select configurations that provide broad coverage of the descriptor space.

The selected configurations are subsequently used to construct the MLIP training, validation, and testing datasets.

The repository is intended to make the complete workflow from **DFT configurations → descriptor representation → data selection → MLIP training → model evaluation** reproducible.

---

# 6. Reference Training Files

To facilitate independent reproduction, the repository includes the files used in the original calculations wherever practical.

These include:

| Model   | Configuration           | Hyperparameters | Training Output           | Trained Models|           
| ------- | ----------------------- | --------------- | ------------------------- |---------------|
| MACE    | Individual `.xyz` files | `mace_submit`   | `.log`                    | `.model`      |
| Allegro | Merged `.xyz` files     | `training.yaml` | `.out` / submission files | `.pth`        |

The provided outputs can be used as reference points when verifying an independent reproduction.

---

# 7. Citation

If you use the data, analysis workflows, or MLIP training files from this repository, please cite the associated manuscript:

**Wrong in the same way: finite-sample bias, not model variance, limits small-data machine-learned potentials**

*Citation information will be added following publication/preprint release.*

---

# 8. Contact

For questions regarding the dataset, analysis workflow, or MLIP training procedure, please refer to the corresponding author or repository maintainers.

