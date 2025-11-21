# Z-score_new_life
### Comprehensive Assessment of Pronuclear Morphological Pattern Prognostic Value Using Machine Learning Approaches in IVF Programs

This repository contains the full analytical pipeline, code implementation, and supplementary materials for the study:

**Nigmatova N.P., Sergeev S.A., Buyanzhargal Y., Kaldarbekova B.B., Arstanbayeva G.G., Azimkhan M.K., Khonik N.M., Makisheva A.T., Shchigolev V.N.  
_Comprehensive Assessment of Pronuclear Morphological Pattern Prognostic Value Using Machine Learning Approaches in IVF Programs_**

The project evaluates the prognostic value of the classical **Z-score pronuclear morphology system** using a combination of:

- Classical statistical analysis  
- PCA-based dimensionality reduction  
- Unsupervised clustering (K-means)  
- Neural network models (DNN, KAN)  
- Ensemble DNN–KAN metamodel  
- Bayesian refinement  
- IVF KPI benchmarking (Vienna & Istanbul consensus)

The repository is designed to provide transparent, reproducible, open-source computational workflow for embryological data research.

---

# 📌 Objectives of the Study

- To reassess the prognostic value of **Z-score pronuclear morphology** in IVF programs.  
- To compare traditional morphology-based assessment with **machine learning approaches**.  
- To investigate biological heterogeneity of IVF cycles using **PCA + K-means clustering**.  
- To evaluate whether **simplified Z-score grouping** (Z1–Z2 vs. Z3–Z4 vs. Z5) provides more consistent predictive behavior.  
- To integrate neural network methods (DNN, KAN) for **pregnancy probability estimation**.  
- To demonstrate the limitations of static morphological scoring and highlight the need for integrated models combining morphology, morphokinetics, and ML.

---
### Core scientific stack
numpy==1.25.2
pandas==2.1.4
scikit-learn==1.5.0
scipy==1.11.4

### Plotting
matplotlib==3.8.2
seaborn==0.13.2

### Machine Learning – Neural Networks
tensorflow==2.15.0
keras==2.14.0

### Kolmogorov–Arnold Networks
pykan==0.2.1

### Conformal Prediction (Venn-Abers)
venn-abers==1.4.5

### Saving models
joblib==1.3.2

### Notebooks
jupyter==1.0.0
ipykernel==6.29.0

### Utilities
tqdm==4.66.1


---

# 🚀 Quick Start

## 1. Install dependencies
```
pip install -r requirements.txt
```

## 2. Run full analysis pipeline
```
python main.py
```

This script will:

- load the processed IVF dataset  
- perform PCA (3 components)  
- perform K-means clustering (k=3)  
- train DNN and KAN models  
- build an ensemble  
- compute metrics (Accuracy, AUC, Precision, Recall, MCC)  
- save results to `/models` and enriched dataset to `/data/processed`

---

# 🧬 Data Description

The dataset consists of:

### **Cycle-level variables**
- patient age  
- number of OCC (cumulus–oocyte complexes)  
- number of mature MII oocytes  
- fertilization outcomes  
- blastocyst formation and quality metrics  
- clinical pregnancy outcome  

### **Z-score categories**  
Based on PNB (nucleolar precursor bodies) count, symmetry, distribution:
- **Z1** — aligned nucleoli, ≥3 in each PN  
- **Z2** — dispersed nucleoli in both PN  
- **Z3** — dispersed in one PN, aligned in the other  
- **Z4** — only 2 nucleoli in one PN  
- **Z5** — asymmetry >20% OR 1 nucleolus  

### **Target variable**
- Implantation (binary), clinical pregnancy confirmed at 6–8 weeks

---

# 🧠 Machine Learning Pipeline

## 🔹 PCA: dimensionality reduction
- PCA retained **89.88%** of variance in 3 components.
- PC1 captured embryonic yield and developmental capacity  
- PC2 reflected pronuclear morphology gradients  
- PC3 captured patient/stimulation variability  

This created a **biologically interpretable 3D space** for IVF cycles.

## 🔹 Clustering (K-means)
Cluster validation metrics:
- Silhouette → **k=2 optimal**
- Calinski–Harabasz → **k=2 highest**
- Davies–Bouldin → prefers **k=8**
- But biological interpretability → **k=3 chosen**

### Final clusters:
| Cluster | Description | Biological meaning |
|--------|-------------|-------------------|
| **0** | low oocyte/embryo yield | poor responders |
| **1** | high oocyte/embryo yield | good responders |
| **2** | intermediate | moderate responders |

Cluster 1 showed highest Z1/Z2 distribution and highest CPR.

---

# 🔬 Statistical Findings

- ANOVA confirmed differences only in **fertilization rates**, not in blastocyst outcomes.
- Highest correlations with blastocyst development:  
  - **Z2 → r=0.472**  
  - **Z3 → r=0.620**  
- Regression showed **low predictive value** across all Z-scores.
- Severe **multicollinearity (VIF > 10)** across categories → limits classical statistical models.

---

# 🤖 Neural Network Models

### **Deep Neural Network (DNN)**
- 2-layer SimpleRNN model  
- L1/L2 regularization  
- Dropout (0.20)  
- Sigmoid output  

### **Kolmogorov–Arnold Network (KAN)**
- PyKAN implementation  
- 2 layers: [10, 1], grid=5  

### **Ensemble (DNN + KAN)**
- Averaged predictions  
- Calibrated via **Venn–Abers** method  
- Integrated with Bayesian updating based on historical CPR  

---

# 📊 Pregnancy Prediction Results

| Z-score | Actual CPR | Predicted CPR | Interpretation |
|--------|------------|---------------|----------------|
| **Z1** | 27.31% | 32.17% | model overestimates |
| **Z2** | 28.20% | 36.72% | overestimates |
| **Z3** | 29.53% | 39.06% | overestimates |
| **Z4** | 27.48% | 27.59% | accurate |
| **Z5** | 28.57% | 23.64% | model underestimates |

This shows that Z1–Z3 group behaves similarly → justifying grouping.

---

# 🧩 Recommended Simplified Z-Score Grouping

The study concludes that evidence supports **collapsing 5 Z-score categories into 3 biologically meaningful groups**:

- **Group A — favorable:** Z1 + Z2  
- **Group B — intermediate:** Z3 + Z4  
- **Group C — least favorable:** Z5 (1 nucleolus)  

This aligns with Istanbul Consensus recommendations.

---

# 📚 Citation

If you use this repository, please cite:

```
Nigmatova N.P., Sergeev S.A. et al. 
Comprehensive Assessment of Pronuclear Morphological Pattern Prognostic Value 
Using Machine Learning Approaches in IVF Programs, 2025.
```

BibTeX:

```bibtex
@article{Nigmatova2025ZscoreML,
  title={Comprehensive Assessment of Pronuclear Morphological Pattern Prognostic Value Using Machine Learning Approaches in IVF Programs},
  author={Nigmatova, N.P. and Sergeev, S.A. and others},
  year={2025}
}
```

---

# 📄 License

MIT License   

---

# 📨 Contact

For questions or collaboration:

- **Corresponding Author:** N.P. Nigmatova  
- **Technical Contact (ML pipeline):** S.A. Sergeev  
- Emails: *noira.nigmat@gmail.com*, *embryossa@gmail.com*

---

