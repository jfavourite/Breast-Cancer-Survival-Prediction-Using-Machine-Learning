# 🎗️ Breast Cancer Survival Prediction Using Machine Learning
### A Multi-feature Clinical & Genomic ML Pipeline | METABRIC Dataset

---

## 📌 Project Overview

This project builds a machine learning pipeline to predict **breast cancer patient survival outcomes** using the METABRIC (Molecular Taxonomy of Breast Cancer International Consortium) dataset. It combines **clinical features** (age, tumour size, hormone receptor status) with **gene expression and mutation data** to train and compare multiple classification models.

The project demonstrates a complete, research-grade ML workflow — from raw data cleaning to model interpretability using SHAP values.

---

## 🎯 Objectives

- Predict binary survival outcome (`overall_survival`: alive vs deceased)
- Compare performance of classical ML models on clinical + genomic data
- Identify the most predictive biological and clinical features using SHAP interpretability
- Demonstrate model stability and generalisability through cross-validation

---

## 📦 Dataset

**METABRIC — Breast Cancer Gene Expression Profiles**
- Source: [Kaggle — METABRIC Dataset](https://www.kaggle.com/datasets/raghadalharbi/breast-cancer-gene-expression-profiles-metabric)
- Patients: 1,904
- Features: 693 (clinical, gene expression, mutation status)
- Target: `overall_survival` (0 = Alive, 1 = Deceased)

> ⚠️ The dataset file (`METABRIC_RNA_Mutation.csv`) is **not included** in this repository due to size. Download it directly from Kaggle using the link above.

---

## 🗂️ Repository Structure

```
breast_cancer_project/
│
├── breast_cancer_ML.ipynb       # Main Jupyter Notebook (full pipeline)
├── model_results.csv            # Model performance comparison table
├── random_forest_model.pkl      # Saved best model (Random Forest)
├── scaler.pkl                   # Saved StandardScaler object
├── requirements.txt             # Python dependencies
└── README.md                    # Project documentation (this file)
```

---

## 🔬 ML Pipeline

### 1. 📥 Data Loading & Exploration
- Loaded 1,904 patient records with 693 features
- Inspected data types, shapes, and distributions
- Identified target variable: `overall_survival`

### 2. 🧹 Data Cleaning
- Dropped columns with >20% missing values (`tumor_stage`, `3-gene_classifier_subtype` etc.)
- Removed data leakage column: `death_from_cancer`
- Imputed remaining missing values:
  - Categorical → mode imputation
  - Numerical → median imputation

### 3. ⚙️ Feature Engineering & Encoding
- Mutation columns (`_mut`) → binarised (0 = no mutation, 1 = mutated)
- Binary categorical columns → Label Encoding
- Ordinal column (`cellularity`) → manual ordinal mapping (Low=1, Moderate=2, High=3)
- Multi-class columns → One-Hot Encoding

### 4. 📊 Feature Selection
- Removed highly correlated features (threshold > 0.95)
- Applied **Mutual Information** scoring to rank all features
- Selected **top 50 features** for modelling

### 5. ✅ Train/Test Split & Scaling
- Stratified 80/20 train/test split
- Applied `StandardScaler` (fit on train, transform on test)

### 6. 🤖 Model Training
Four models trained and evaluated:

| Model | Accuracy | ROC-AUC |
|---|---|---|
| Logistic Regression | 0.664 | 0.719 |
| Random Forest | **0.688** | **0.747** |
| XGBoost | 0.685 | 0.744 |
| Neural Network | 0.633 | 0.682 |

### 7. 📈 Evaluation
- Metrics: Accuracy, ROC-AUC, Precision, Recall, F1-score
- Confusion matrices for all models
- ROC curve comparison across all models

### 8. 🔍 SHAP Interpretability
- Applied SHAP (SHapley Additive exPlanations) to the best model (Random Forest)
- Generated summary dot plot, bar plot, and single-patient waterfall plot
- Top predictive features identified: `age_at_diagnosis`, `nottingham_prognostic_index`, `brca1`

### 9. ✅ Cross Validation
- 5-Fold Stratified Cross Validation on Random Forest
- **Mean AUC: 0.747 ± 0.013** — confirms model stability and generalisability

---

## 📊 Key Results

- **Best Model:** Random Forest (AUC = 0.747)
- **Cross-Validation AUC:** 0.747 ± 0.013 (stable across all 5 folds)
- **Top Predictive Features (SHAP):**
  - `age_at_diagnosis` — strongest predictor
  - `nottingham_prognostic_index` — validated clinical survival score
  - `brca1` — key breast cancer susceptibility gene
  - Multiple gene expression features validating the multi-modal approach

> The model independently identified clinically validated prognostic factors, suggesting it has learned biologically meaningful patterns rather than spurious correlations.

---

## 🧬 Clinical Relevance

The alignment between model-identified features and established clinical knowledge (NPI, BRCA1, age) validates the approach. The lower recall for deceased patients across models reflects a class imbalance challenge and highlights an important direction for future work — improving sensitivity for high-risk patients is clinically critical.

---

## 🚀 How To Run

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/breast_cancer_project.git
cd breast_cancer_project
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Download the dataset
Download `METABRIC_RNA_Mutation.csv` from [Kaggle](https://www.kaggle.com/datasets/raghadalharbi/breast-cancer-gene-expression-profiles-metabric) and place it in the project folder.

### 4. Launch Jupyter Notebook
```bash
jupyter notebook
```
Open `breast_cancer_ML.ipynb` and run all cells.

---

## 🛠️ Dependencies

```
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
shap
joblib
jupyter
```

Install all at once:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap joblib notebook
```

---

## 🔮 Future Work

- Address class imbalance with SMOTE oversampling
- Hyperparameter tuning with GridSearchCV
- Survival analysis with Cox Proportional Hazards model
- Deep learning with larger datasets (TCGA-BRCA)
- Multi-modal fusion with raw RNA-seq data

---

## 👤 Author

**[Your Name]**
BSc/MSc in [Your Field] | PhD Applicant
[Your Institution]
[Your Email] | [LinkedIn] | [GitHub]

---

## 📄 License

This project is open source under the [MIT License](LICENSE).

---

## 📚 References

- Curtis et al. (2012). The genomic and transcriptomic architecture of 2,000 breast tumours reveals novel subgroups. *Nature*.
- METABRIC Dataset — Kaggle
- SHAP: Lundberg & Lee (2017). A unified approach to interpreting model predictions. *NeurIPS*.
- Scikit-learn: Pedregosa et al. (2011). *JMLR*.
