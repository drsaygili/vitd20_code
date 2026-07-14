# Machine Learning-Driven Prediction of Vitamin D Deficiency Using Non-Laboratory Predictors

## Overview
This repository contains the dataset and source code for the machine learning-driven prediction of Vitamin D deficiency defined as $25\text{-hydroxyvitamin D } [25\text{(OH)D}] < 20\text{ ng/mL}$.

By leveraging simple, non-laboratory (non-invasive) clinical predictors—such as **Age**, **Sex**, **Body Mass Index (BMI)**, **Vitamin D Supplementation**, and the **cyclical representation of the Day of the Year**—we construct an explainable Logistic Regression model and compare its performance against four advanced machine learning algorithms (CatBoost, XGBoost, Random Forest, and Support Vector Machine).

The study and model reporting are structured in strict compliance with the **TRIPOD+AI (Transparent Reporting of a Multivariable Prediction Model for Individual Prognosis or Diagnosis + Artificial Intelligence)** guidelines.

---

## Predictive Variables
The model uses six non-laboratory predictors that are readily available without clinical blood testing:
1. **Age (years)**: Continuous variable (standardised).
2. **Sex**: Binary variable (Female = 1, Male = 0).
3. **Body Mass Index (BMI, kg/m²)**: Continuous variable (standardised).
4. **Vitamin D Supplementation**: Binary variable representing supplement use (Yes = 1, No = 0).
5. **Cyclical Day of the Year (Sine component, $\sin\theta$)**: Continuous variable representing seasonal variation, where $\theta = \frac{2\pi \times \text{Day}}{365}$.
6. **Cyclical Day of the Year (Cosine component, $\cos\theta$)**: Continuous variable representing seasonal variation, where $\theta = \frac{2\pi \times \text{Day}}{365}$.

---

## Repository Structure
- `Vitamin_D_Deficiency_Prediction.ipynb`: Jupyter notebook containing the complete analysis workflow, including environment verification, baseline statistics, train/test splitting, model training/hyperparameter optimization, bootstrapping, calibration analysis (LOESS), Decision Curve Analysis (DCA), and risk stratification.
- `data_for_py.csv`: The complete patient cohort dataset containing predictors and target labels ($y=1$ for deficiency, $y=0$ for sufficiency).

---

## Model Evaluation and Results
Our primary Logistic Regression (LR) model demonstrated strong predictive performance on the independent test set ($n=531$), comparable to complex machine learning models.

### Performance Summary
The following metrics are computed on the independent test set using optimal decision thresholds:

| Model | Cross-Validated AUC [95% CI] | Independent Test AUC [95% CI] | Brier Score | Sensitivity | Specificity | F1 Score |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | 0.824 [0.803–0.844] | 0.832 [0.796–0.865] | 0.165 | 0.733 | 0.771 | 0.697 |
| **CatBoost** | 0.829 [0.808–0.849] | 0.838 [0.803–0.871] | 0.158 | 0.751 | 0.774 | 0.709 |
| **XGBoost** | 0.826 [0.806–0.847] | 0.832 [0.795–0.865] | 0.162 | 0.733 | 0.781 | 0.701 |
| **Random Forest** | 0.823 [0.803–0.844] | 0.829 [0.794–0.862] | 0.162 | 0.760 | 0.749 | 0.695 |
| **SVM** | 0.819 [0.798–0.839] | 0.824 [0.788–0.858] | 0.167 | 0.773 | 0.723 | 0.686 |

*Statistical significance of differences in AUC values was verified using DeLong's test, which showed no statistically significant difference between the explainable Logistic Regression model and the machine learning classifiers.*

### Calibration and Clinical Utility
- **Calibration**: The Logistic Regression model exhibits excellent calibration (Calibration Slope $\approx$ 1.00; Intercept $\approx$ 0.00; low Integrated Calibration Information [ICI]).
- **Clinical Utility**: Decision Curve Analysis (DCA) demonstrates that the Logistic Regression model provides positive net benefit within the probability threshold range of 11% to 71%, outperforming the default strategies of "treat all" (empirical supplementation) and "treat none". In extreme high-risk threshold regions (e.g., above 80%), the net benefit becomes negative, suggesting that model-guided decision-making should be restricted to the moderate risk range (11%–71%), and alternative management strategies should be considered for patients in the high-risk zones.

---

## Clinical Risk Stratification Cutoffs
To facilitate clinical translation, the model outputs were stratified into three clinical decision rules:
- **Low-Risk Rule-Out ($p < 0.20$)**: High sensitivity rule to identify patients highly unlikely to have deficiency.
- **Youden-Optimal Cutoff ($p \approx 0.448$)**: Balanced threshold for screening.
- **Very-High-Risk Rule-In ($p \geq 0.80$)**: High specificity rule to identify patients eligible for empirical supplement initiation without laboratory testing.

---

## Reproduction and Installation

### Run via Google Colab
You can run the notebook directly in Google Colab:
1. Open Google Colab and import `Vitamin_D_Deficiency_Prediction.ipynb`.
2. Upload `data_for_py.csv` into the runtime space when prompted.
3. Run the cells sequentially. The final cell will compress all output tables and figures into `vitamin_d_analysis_outputs.zip`.

### Run Locally
To set up the exact analysis environment on your local machine, run the following:

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/vitd20_code.git
   cd vitd20_code
   ```
2. Install the pinned dependencies:
   ```bash
   pip install numpy==1.26.4 pandas==3.0.3 scipy==1.17.1 matplotlib==3.11.0 scikit-learn==1.9.0 statsmodels==0.14.6 xgboost==3.2.0 catboost==1.2.7
   ```
3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
   Open `Vitamin_D_Deficiency_Prediction.ipynb` and execute the cells.

---

## Citation
If you use this model or dataset in your academic work, please cite our paper:
*(Citation details will be updated upon publication)*
