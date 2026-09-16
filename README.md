# 3D Induced Polarization Modelling & Inversion

A 3D geophysical modelling and inversion project using **SimPEG** to simulate induced polarization responses from synthetic subsurface structures and recover 3D chargeability distributions through regularized inversion.

The project implements terrain-aware 3D Tree meshes, dipole-dipole survey geometries, synthetic data generation with uncertainties, and weighted least-squares and IRLS inversion methods for subsurface chargeability estimation.

---

## Table of Contents

* [Overview](#overview)
* [Project Objectives](#project-objectives)
* [Methodology](#methodology)
* [Forward Modelling](#forward-modelling)
* [3D Mesh Generation](#3d-mesh-generation)
* [Survey Configuration](#survey-configuration)
* [Synthetic Data Generation](#synthetic-data-generation)
* [3D Chargeability Inversion](#3d-chargeability-inversion)
* [Inversion Methods](#inversion-methods)
* [Sensitivity Weighting & Regularization](#sensitivity-weighting--regularization)
* [Model Evaluation](#model-evaluation)
* [Tools & Technologies](#tools--technologies)
* [Project Structure](#project-structure)
* [How to Run This Project](#how-to-run-this-project)
* [Key Outcomes](#key-outcomes)

---

## Overview

Induced Polarization (IP) is a geophysical method used to characterize subsurface materials based on their ability to temporarily store electrical charge.

This project focuses on **3D IP forward modelling and inversion**. A synthetic subsurface chargeability model is first constructed and used to simulate apparent chargeability measurements. Synthetic observations are then perturbed using data uncertainties and used as input to a 3D inversion workflow.

The objective is to recover the underlying 3D chargeability distribution while accounting for sensitivity, data uncertainty, and model regularization.

---

## Project Objectives

The main objectives of the project were to:

* Develop a 3D IP forward modelling workflow using SimPEG
* Simulate apparent chargeability responses from synthetic subsurface structures
* Construct terrain-aware 3D Tree meshes
* Define dipole-dipole survey geometries
* Generate synthetic IP observations with data uncertainties
* Formulate the 3D chargeability inverse problem
* Apply weighted least-squares inversion
* Apply IRLS inversion for improved model recovery
* Incorporate sensitivity weighting and regularization
* Recover and visualize 3D subsurface chargeability distributions

---

## Methodology

The complete workflow follows:

```text
Synthetic Subsurface Model
          |
          v
Terrain & Mesh Construction
          |
          v
3D Tree Mesh
          |
          v
Dipole-Dipole Survey
          |
          v
IP Forward Modelling
          |
          v
Synthetic Apparent
Chargeability Data
          |
          v
Add Data Uncertainties
          |
          v
3D Chargeability Inversion
          |
       +--+--+
       |     |
       v     v
     WLS    IRLS
       |     |
       +--+--+
          |
          v
Sensitivity Weighting
          |
          v
Regularization
          |
          v
Recovered 3D
Chargeability Model
```

---

## Forward Modelling

The forward modelling stage calculates the expected IP response for a known subsurface chargeability distribution.

A synthetic 3D subsurface model is defined with chargeable structures embedded within the background medium.

The forward modelling workflow is:

```text
True Chargeability Model
          |
          v
    3D Discretization
          |
          v
  Survey Configuration
          |
          v
    Forward Simulation
          |
          v
Apparent Chargeability
       Response
```

SimPEG is used to formulate and solve the forward problem and obtain the predicted IP response at the survey electrodes.

---

## 3D Mesh Generation

A **terrain-aware 3D Tree mesh** is constructed to discretize the subsurface.

The Tree mesh provides adaptive spatial discretization, allowing higher resolution where required while avoiding unnecessary computational cost in regions that do not require fine cells.

The mesh accounts for:

* Surface topography
* Subsurface geometry
* Synthetic anomalous structures
* Survey locations
* Refinement requirements

The resulting mesh forms the computational domain for both forward modelling and inversion.

---

## Survey Configuration

A **dipole-dipole electrode configuration** is used for the synthetic IP survey.

The survey consists of:

* Current electrode pair
* Potential electrode pair
* Electrode locations
* Dipole spacing
* Multiple source-receiver configurations

Conceptually:

```text
Surface

A       B           M       N
|-------|-----------|-------|
Current Dipole      Potential Dipole

<------ Electrode Array ------>
```

Multiple dipole configurations are used to sample the subsurface response across the survey area.

---

## Synthetic Data Generation

Synthetic apparent chargeability observations are generated from the forward model.

The workflow is:

```text
True 3D Model
     |
     v
Forward Simulation
     |
     v
Predicted IP Response
     |
     v
Add Data Uncertainty
     |
     v
Synthetic Observations
```

Data uncertainties are incorporated to simulate measurement uncertainty and create a more realistic inversion problem.

The resulting observations contain:

* Apparent chargeability responses
* Measurement uncertainties
* Survey geometry information

---

## 3D Chargeability Inversion

The inverse problem estimates the subsurface chargeability distribution from the observed apparent chargeability data.

The general inverse problem can be represented as:

```text
Observed IP Data
       |
       v
Forward Model
       |
       v
Predicted IP Data
       |
       v
Data Misfit
       |
       v
Model Update
       |
       v
Convergence
```

The inversion seeks a model that provides a good fit to the observations while avoiding unrealistic model complexity.

---

## Inversion Methods

Two inversion approaches were investigated.

### Weighted Least-Squares Inversion

Weighted least-squares inversion incorporates data uncertainties into the data misfit formulation.

The weighting ensures that observations with different uncertainty levels contribute appropriately to the inversion.

The objective is to minimize a combination of:

* Weighted data misfit
* Model regularization

Conceptually:

```text
Observed Data
      |
      v
Data Weighting
      |
      v
Weighted Data Misfit
      |
      +
      |
Regularization
      |
      v
Model Update
```

---

### IRLS Inversion

An **Iteratively Reweighted Least Squares (IRLS)** approach was applied to modify the regularization weights during the inversion process.

IRLS can be used to promote more focused or block-like model structures compared with conventional smooth least-squares regularization.

The workflow is:

```text
Initial Model
     |
     v
Least-Squares Update
     |
     v
Calculate Model Weights
     |
     v
Update Regularization
     |
     v
Repeat
     |
     v
Converged Model
```

The recovered model is then compared with the known synthetic chargeability distribution.

---

## Sensitivity Weighting & Regularization

### Sensitivity Weighting

Sensitivity information is incorporated to account for variations in the ability of different subsurface cells to influence the measured data.

Sensitivity weighting helps prevent poorly resolved regions from dominating the recovered model.

### Regularization

Regularization constrains the inversion to produce a stable and geologically reasonable model.

The inversion balances:

```text
Data Fit
   +
Model Constraints
   =
Stable Inverted Model
```

Regularization parameters control the trade-off between fitting the observations and maintaining model smoothness or structure.

---

## Model Evaluation

The recovered chargeability model is evaluated by comparing it with the original synthetic model.

Key evaluation aspects include:

### Data Fit

Compare observed and predicted apparent chargeability responses.

```text
Observed Data
      |
      v
Inversion
      |
      v
Recovered Model
      |
      v
Forward Simulation
      |
      v
Predicted Data
      |
      v
Data Fit
```

### Model Recovery

The recovered 3D chargeability distribution is compared with the known synthetic structures.

### Spatial Resolution

The recovered anomaly locations, geometry, and amplitude are examined to assess how effectively the inversion resolves the synthetic subsurface structures.

### Inversion Stability

The effects of sensitivity weighting, regularization, and inversion method are evaluated through the resulting model and data misfit.

---

## Tools & Technologies

* Python
* SimPEG
* NumPy
* SciPy
* Discretize
* Matplotlib
* 3D Geophysical Modelling
* Induced Polarization
* Forward Modelling
* Inverse Modelling
* Tree Meshes
* Dipole-Dipole Surveys
* Weighted Least Squares
* IRLS
* Sensitivity Weighting
* Regularization

---

## Project Structure

```text
3D-IP-Modelling-Inversion/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── forward_modeling.ipynb
│   ├── synthetic_data_generation.ipynb
│   └── chargeability_inversion.ipynb
│
├── src/
│   ├── mesh.py
│   ├── survey.py
│   ├── forward_model.py
│   ├── data_generation.py
│   └── inversion.py
│
├── data/
│   ├── synthetic/
│   └── observations/
│
├── results/
│   ├── forward/
│   └── inversion/
│
└── figures/
    ├── mesh/
    ├── synthetic_model/
    └── inverted_model/
```

The structure above should be modified according to the actual files and folders in the repository.

---

## How to Run This Project

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/<repository-name>.git
cd <repository-name>
```

### 2. Create a Virtual Environment

For Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

For Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Forward Model

Open the forward modelling notebook or script:

```text
notebooks/forward_modeling.ipynb
```

Run the workflow to construct the mesh, define the synthetic model and survey, and simulate the apparent chargeability response.

### 5. Generate Synthetic Observations

Run:

```text
notebooks/synthetic_data_generation.ipynb
```

This generates synthetic IP observations and associated data uncertainties.

### 6. Run the Inversion

Run:

```text
notebooks/chargeability_inversion.ipynb
```

The notebook performs the 3D chargeability inversion using weighted least-squares and IRLS approaches.

### 7. Visualize the Results

Compare:

* True synthetic chargeability model
* Synthetic observations
* Weighted least-squares inversion
* IRLS inversion
* Predicted apparent chargeability
* Data misfit

---

## Key Outcomes

The project demonstrates an end-to-end workflow for 3D induced polarization modelling and inversion.

The analysis focused on:

* Building terrain-aware 3D Tree meshes
* Simulating IP responses from synthetic subsurface structures
* Implementing dipole-dipole survey geometries
* Generating synthetic observations with data uncertainties
* Formulating the 3D chargeability inverse problem
* Applying weighted least-squares inversion
* Applying IRLS inversion
* Incorporating sensitivity weighting and regularization
* Recovering and evaluating 3D chargeability distributions

---

## Author

**Mansi Meena**

Self Project
October 2025 – December 2025
