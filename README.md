# 3D-PEDL: Datasets for 3D Fractured Media Inversion

This repository contains the training datasets and reference benchmarks for 3D-PEDL (three-dimensional version of Parameter Estimator with Deep Learning), a deep learning framework designed for 3D fractured media inversion with robustness to limited spatiotemporal observations. 

This dataset accompanies our manuscript submitted to Stochastic Environmental Research and Risk Assessment (SERRA).
---

## Dataset Note

To keep the repository efficient and focused, this repository provides the complete datasets under the Transient Observation with Medium Density configuration for both the **Simple Case** and **Complex Case** described in the paper. This specific spatiotemporal observation strategy provides sufficient dynamic and spatial features to validate the robust inversion capabilities of the 3D-PEDL model.

*(For datasets under other observation densities or steady-state scenarios, please contact the corresponding author upon reasonable request.)*

---

## Dataset Structure

All files are stored in the **NumPy array (`.npy`)** format.

```text
data/
├── simple_case/
│   ├── train_fracture_fields_simple.npy              # 5,000 synthetic 3D hydraulic conductivity fields
│   ├── train_head_transient_medium_simple.npy        # 5,000 transient head observation fields (Medium Density)
│   ├── reference_fracture_field_simple.npy           # The true reference lnK field used for inversion testing
│   └── reference_head_transient_medium_simple.npy     # The actual dynamic heads observed from the reference field
│
└── complex_case/
    ├── train_fracture_fields_complex.npy             # 5,000 synthetic 3D hydraulic conductivity fields
    ├── train_head_transient_medium_complex.npy       # 5,000 transient head observation fields (Medium Density)
    ├── reference_fracture_field_complex.npy          # The true reference lnK field used for inversion testing
    └── reference_head_transient_medium_complex.npy    # The actual dynamic heads observed from the reference field
```

## Data Descriptions

### 1. Training Datasets (5,000 realizations per case)
- **Fracture Fields (`train_fracture_fields_*.npy`)**: 
  - **Dimensions**: `[5000, Nz, Ny, Nx]` representing the full 3D spatial grids of log-hydraulic conductivity ($\ln K$) or binary fracture indicators generated via stochastic fracture network simulation. Nz = 3; Ny = Nx =64
- **Head Observations (`train_head_transient_medium_*.npy`)**: 
  - **Dimensions**: `[5000, C_in, Ny, Nx]`.
  - **Physical Meaning**: Data Characteristics: To maintain spatial consistency for deep learning alignment, these files retain the full 3D grid size (Nx * Ny * Nz). True hydraulic head values are provided only at the specific grid locations corresponding to the observation wells, while all remaining unmonitored grid cells are padded with 0.
  - C_in=Nz * N_inj * N_t, capturing transient head responses to N_inj = 5 individual well injections across Nz=3 layers and N_t=11 time steps. 

### 2. Reference Benchmarks
- **Reference Fields (`reference_fracture_field_*.npy`)**: 
  - The true underlying 3D structural target `[1, Nz, Ny, Nx]` that the trained 3D-PEDL model aims to reconstruct during the validation phase.
- **Reference Observations (`reference_head_transient_medium_*.npy`)**: 
  - **Dimensions**: `[1, C_in, Ny, Nx]`.
  - The actual sparse transient hydraulic heads sampled **exclusively at the observation well locations** from the reference field, serving as the conditioning inputs for inversion. 
