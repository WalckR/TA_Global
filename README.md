# Global terrestrial testate amoeba diversity reveals a mismatch between richness and endemicity hotspots

This repository contains the complete set of Python scripts (Jupyter Notebooks) developed for our scientific publication to model, optimize, spatially project, and analyze biodiversity metrics (`Richness`, `Diversity`, `Endemicity`) as well as species community assemblages (`Clusters 1, 2, and 3`).

The pipeline integrates the spatial computing power and satellite imagery analysis of **Google Earth Engine (GEE)** via its Python API with the machine learning capabilities of **Scikit-Learn** (Random Forest) for feature selection, hyperparameter tuning, and large-scale predictive modeling.

---

## 📂 Scripts Architecture & Pipeline Workflow

The workflow is strictly structured into 6 chronological steps (`STEP 0` to `STEP 5`) to guarantee full reproducibility of the paper's results:

### 🔹 STEP 0: Formatting & Data Ingestion to Google Earth Engine
These notebooks take cleaned raw field data (geographic coordinates, associated metrics, and indices), clean them, and convert them into spatial vector objects (using `geopandas`) before exporting them automatically as **Assets** to your GEE project.
* `DataTA_STEP0_Prepare_For_GEE_CLUSTERS.ipynb`: Formats and exports species community assemblages (Clusters).
* `DataTA_STEP0_Prepare_For_GEE_DIVERSITY.ipynb`: Formats and exports Diversity data.
* `DataTA_STEP0_Prepare_For_GEE_ENDEMICITY.ipynb`: Formats and exports Endemicity data.
* `DataTA_STEP0_Prepare_For_GEE_RICHNESS.ipynb`: Formats and exports Species Richness data.

### 🔹 STEP 1: Spatial Sampling of Environmental Covariates
Scripts leveraging the GEE API to extract values from dozens of environmental and anthropogenic covariates (bioclimatic variables, soil properties, NDVI/EVI vegetation indices, topography, human modification markers) directly at the geographic coordinates of each field sample. Final matrices are exported as `.csv` files.
* `DataTA_STEP1_Covariable_Sampling_Full_CLUSTERS.ipynb`: Extraction for Cluster points.
* `DataTA_STEP1_Covariable_Sampling_Full_DIVERSITY.ipynb`: Extraction for Diversity points.
* `DataTA_STEP1_Covariable_Sampling_Full_RICHNESS.ipynb`: Extraction for Richness points.
* `DataTA_STEP1_LandCover_Sampling_ENDEMICITY.ipynb`: Extraction of specific land cover and environmental variables for Endemicity.

### 🔹 STEP 2: Model Training & Hyperparameter Tuning
For each biodiversity metric, these notebooks configure a **Random Forest Regressor**. They integrate recursive feature selection (RFECV or model importance thresholds), optimal hyperparameter tuning using `RandomizedSearchCV` (5-fold cross-validation) executed over multiple iterative *Runs*, performance assessment ($R^2$ and $RMSE$), and permutation feature importance calculation.
* `DataTA_STEP2_Model_CLUSTER1_Tuning_Random_Forest_AllVar.ipynb`: Cluster 1 Modeling.
* `DataTA_STEP2_Model_CLUSTER2_Tuning_Random_Forest_AllVar.ipynb`: Cluster 2 Modeling.
* `DataTA_STEP2_Model_CLUSTER3_Tuning_Random_Forest_AllVar.ipynb`: Cluster 3 Modeling.
* `DataTA_STEP2_Model_DIVERSITY_Tuning_Random_Forest_AllVar.ipynb`: Diversity Modeling.
* `DataTA_STEP2_Model_RICHNESS_Tuning_Random_Forest_AllVar.ipynb`: Species Richness Modeling.

### 🔹 STEP 2B: GEE Assets Automated Synchronization
* `DataTA_STEP2B_Delete_and_Import_NewFiles_In_Asset.ipynb`: A utility script to clear out old temporary files from local storage and automatically upload the updated training/validation point sets (`TrainPts`) from each iteration directly into your GEE asset directories.

### 🔹 STEP 3: Ensemble Predictive Mapping (GEE)
These scripts apply the optimized model architectures determined in STEP 2 to project predictions across the entire regional/global study area in GEE. They predict targets and compute spatial uncertainty (Ensemble model mean rasters and coefficients of variation), exporting final rasters as `.tif` files directly to Google Drive.
* `DataTA_STEP3_Mapping_CLUSTER1_RF.ipynb`
* `DataTA_STEP3_Mapping_CLUSTER2_RF.ipynb`
* `DataTA_STEP3_Mapping_CLUSTER3_RF.ipynb`
* `DataTA_STEP3_Mapping_DIVERSITY_RF.ipynb`
* `DataTA_STEP3_Mapping_RICHNESS_RF.ipynb`

### 🔹 STEP 4: Macroecological Analysis (Latitudinal & Longitudinal Gradients)
Using the `rasterio` library, these notebooks ingest the exported `.tif` predictive rasters. They extract pixel statistical profiles to compile and export macroecological trends across latitudinal and longitudinal bands.
* `DataTA_STEP4_DiversityByLatitude.ipynb`: Latitudinal/longitudinal profile analysis for Diversity.
* `DataTA_STEP4_RichnessByLatitude.ipynb`: Latitudinal/longitudinal profile analysis for Species Richness.

### 🔹 STEP 5: Publication Figures Production
Dedicated scripts for the styling, plot assembly, and generation of high-resolution (`DPI=600`) scientific visualizations.
* `DataTA_STEP5_Figure_Sampling_DiversityRichness.ipynb`: Visualizes field sampling point distribution across regional/global biomes.
* `DataTA_STEP5_FiguresClusters.ipynb`: Generates spatial distribution maps for the 3 species clusters alongside their envelope of variability ($\pm 2\sigma$).
* `DataTA_STEP5_FiguresDiversity.ipynb`: Figures and maps for the final Diversity model and its associated spatial coefficient of variation.
* `DataTA_STEP5_FiguresEndemicity.ipynb`: Visual representations and plots for the Endemicity model.
* `DataTA_STEP5_FiguresRichness.ipynb`: Final mapping layouts and profile plots for Species Richness.

---

## 📊 Data Availability & Intermediate Results

### 🔹 Raw Data Policy
The raw field datasets used to initialize this pipeline are not hosted directly in this repository due to privacy/licensing restrictions. However, they are fully available for academic and replication purposes:
> **Data Availability Statement:** Raw biodiversity data, species counts, and community metrics are available from the corresponding authors upon reasonable request.

### 🔹 Repository Tree & Intermediate Outputs
To guarantee full reproducibility, this repository hosts all intermediate outputs, optimized model configurations, statistical logs, and structural maps generated throughout the pipeline runs. The local directory structure is organized as follows:

```text
├── 📁 RF/                               # Random Forest Intermediate Results
│   ├── 📁 Clusters/
│   │   ├── 📁 Cluster1/
│   │   │   ├── 📄 RF_Ensemblescores.csv       # Iterative performance metrics for Cluster 1
│   │   │   └── 📄 RF_Ensembleparameters.csv   # Best hyperparameters extracted from Tuning
│   │   ├── 📁 Cluster2/
│   │   │   ├── 📄 RF_Ensemblescores.csv       # Iterative performance metrics for Cluster 2
│   │   │   └── 📄 RF_Ensembleparameters.csv   # Best hyperparameters extracted from Tuning
│   │   └── 📁 Cluster3/
│   │       ├── 📄 RF_Ensemblescores.csv       # Iterative performance metrics for Cluster 3
│   │       └── 📄 RF_Ensembleparameters.csv   # Best hyperparameters extracted from Tuning
│   │
│   ├── 📁 Diversity/
│   │   ├── 📄 RF_Ensemblescores.csv           # Iterative performance metrics for Diversity
│   │   ├── 📄 RF_Ensembleparameters.csv       # Best hyperparameters extracted from Tuning
│   │   └── 📄 imageToDrive_meanModel_Diversity_RF.tif  # Exported macroecological Predictive Raster
│   │
│   └── 📁 Richness/
│       ├── 📄 RF_Ensemblescores.csv           # Iterative performance metrics for Species Richness
│       ├── 📄 RF_Ensembleparameters.csv       # Best hyperparameters extracted from Tuning
│       └── 📄 imageToDrive_meanModel_Richness_RF.tif   # Exported macroecological Predictive Raster
│
├── 📄 DataTA_STEP0_...ipynb             # Execution Scripts (Steps 0 to 5)
└── 📄 README.md                         # Project Documentation
