# Predicting Bandgaps of Heavy Metal Oxides for EUV Photoresists

**Scientific Question:** Can a composition-based Random Forest model predict the electronic bandgap of heavy metal oxides across different metal families using the Materials Project dataset?

## 📊 Dataset Source & Access
The dataset consists of 7,786 unique, thermodynamically stable heavy metal oxides (containing Sn, Hf, Zr, Ti, Al, Zn, or Ni).
* **Source:** Data is dynamically retrieved from the **Materials Project** database using the `mp-api` python client.
* **Citation:** Jain, A., Ong, S. P., Hautier, G., Chen, W., Richards, W. D., Dacek, S., Cholia, S., Skinner, D., Ceder, G., Persson, K. A. (2013). Commentary: The Materials Project: A materials genome approach to accelerating materials innovation. *APL Materials*, 1(1), 011002.
* **Access Instructions:** You need a free API key from the Materials Project. Register at [https://nextgen.materialsproject.org/api](https://nextgen.materialsproject.org/api) to obtain your key.

## 🛠️ Step-by-step Setup Instructions

To ensure reproducibility, please follow these setup steps on a clean machine:

**1. Clone the repository**
```bash
git clone https://github.com/fightpang/euv-mox-resist.git
cd euv-mox-resist
```


**2. Set up the Conda environment**

Create and activate the environment using the provided environment.yml file:

```bash
conda env create -f environment.yml
conda activate matds_env
```

**3. Set up the Materials Project API Key securely**

To run the data acquisition notebook, you must configure your API key locally.

1. Go to [next.materialsproject.org/api](https://next.materialsproject.org/api) to create a free account and copy your API key.

2. Do not hardcode your API key in notebooks. As per the provided `.env.example` file, you need a `.env` file in the repository root.

3. Create it by copying the example file or running the following command in your terminal:

```bash
# Create a .env file (this file is gitignored — never commit it)
echo "MP_API_KEY=your_actual_key_here" > .env
```

(Note for Windows users: If you are using Windows PowerShell, the echo command may encode the file in UTF-16, causing reading errors in Python. In that case, please create the .env file manually using a text editor and save it with UTF-8 encoding.)


## 🚀 How to Run the Notebooks
To fully reproduce this project, please execute the notebooks in the notebooks/ directory in the following exact order using Kernel → Restart & Run All:

01_data_acquisition.ipynb: Queries the MP API for heavy metal oxides, applies thermodynamic stability (energy_above_hull < 0.1) and density filters, and saves the raw dataset.

02_eda_featurization.ipynb: Computes 132 MAGPIE elemental features, handles missing values, and performs UMAP dimensionality reduction to explore the chemical composition space.

03_modeling.ipynb: Trains and evaluates Random Forest and Ridge Regression models. Uses a prototype-aware GroupKFold cross-validation strategy (grouped by metal family) to strictly prevent data leakage.

04_results_visualization.ipynb: Generates cross-validated parity plots, feature importance analyses, and the Pareto front to identify optimal EUV photoresist candidates.

## 📈 Key Results & Optimal Candidates
The Random Forest model trained exclusively on MAGPIE composition descriptors achieved a test-set MAE of 0.867 eV and an R² of 0.257 under rigorous cross-family GroupKFold validation.

While the model demonstrated strong intra-family performance (R² ~ 0.58–0.78), the severe drop in cross-family generalization reveals a critical insight: composition-only descriptors are insufficient to capture the complex local coordination and crystal field splitting effects that uniquely govern different heavy metal cations.

Despite these limitations, a Pareto optimization approach successfully balanced high density (crucial for EUV absorption) and high bandgap. The top 5 optimal candidates identified by the model are:

HfH₂OF₅ (Density: 5.67 g/cm³, Predicted Bandgap: 6.21 eV)

KH₂OF₅ (Density: 2.85 g/cm³, Predicted Bandgap: 6.18 eV)

BaAlBO₃F₂ (Density: 4.29 g/cm³, Predicted Bandgap: 5.92 eV)

Figure: Pareto front of predicted bandgap vs. density. The red markers highlight the top 5 candidates that offer the best trade-off between EUV absorption capability and wide electronic bandgaps.
