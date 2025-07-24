# Clinical and Genetic Predictors of Cancer Outcomes: A Survival and Regression Analysis
🔗Dataset: [China Cancer Patient ](https://www.kaggle.com/datasets/ak0212/china-cancer-patient-records)

# Objective
This project aims to analyze clinical cancer data to investigate factors influencing tumor metastasis, identify predictors of patient survival outcomes, explore relationships between tumor characteristics, comorbidities, genetic mutations, and clinical outcomes.

# Methodology

A. Data Source and Management:
Dataset: A de-identified dataset of 10,000 cancer patient records, including demographic information (PatientID, Gender, Age, Province, Ethnicity), clinical characteristics (TumorType, CancerStage, DiagnosisDate, TumorSize, Metastasis, GeneticMutation, Comorbidities), treatment details (TreatmentType, SurgeryDate, ChemotherapySessions, RadiationSessions), and outcomes (SurvivalStatus, FollowUpMonths, SmokingStatus, AlcoholUse).

Software: All data management and statistical analyses were performed using RStudio (version 4.4.1), utilizing packages such as tidyverse (for data manipulation and visualization), survival and survminer (for survival analysis), gtsummary (for summary tables), epitools (for epidemiological measures), and car (for regression diagnostics).

B. Data Cleaning and Preprocessing:
Variable Standardization: Column names were made R-friendly.

Data Type Conversion: Variables were converted to appropriate data types (e.g., factor for categorical, numeric for continuous, Date for dates).

Missing Value Handling:
GeneticMutation: Blanks and "None" were recoded as NA and then Has_GeneticMutation (binary: Yes/No) was created. The original GeneticMutation was converted to a factor for specific mutation analysis.

Comorbidities: Blanks and NA were consolidated to "None." Binary indicator variables (Comorbidity_Diabetes, Comorbidity_Hypertension, Comorbidity_HepatitisB) were created for specific comorbidities using str_detect. A Comorbidity_Count variable was calculated to quantify the number of recorded comorbidities for each patient.

SurgeryDate: N/A values were converted to NA, and a binary Had_Surgery variable was derived.

Feature Engineering/Categorization:
AgeGroup: Patients were categorized into age groups (<=30, 31-50, 51-65, >65).

TumorSize_Category: Tumor size was categorized into tertiles (Small, Medium, Large).

TimeToSurgeryDays: Calculated as the difference between SurgeryDate and DiagnosisDate for relevant cases.

C. Statistical Analysis Plan:

Descriptive Statistics:
Summary tables (mean/SD for continuous, counts/percentages for categorical) were generated using tbl_summary.

Distributions of key variables (Age, TumorSize, TumorType, CancerStage, SurvivalStatus) were visualized using histograms and bar plots (ggplot2).

Bivariate Analyses:
Metastasis and Tumor Size: A two-sample independent t-test was used to compare the mean TumorSize between patients with and without Metastasis. Box plots visualized this relationship.

Survival Analysis:
Kaplan-Meier Survival Curves: Overall survival probabilities were estimated and plotted using the Kaplan-Meier method, stratified by CancerStage. The log-rank test was used to compare survival curves across stages.

Cox Proportional Hazards Regression: A multivariable Cox proportional hazards model was employed to identify independent predictors of overall survival. The model included Age, Gender, TumorSize, Metastasis, CancerStage, TreatmentType, and SmokingStatus. The proportional hazards assumption was assessed using cox.zph function.

Logistic Regression:
Predicting Metastasis: A multivariable logistic regression model was used to assess factors predicting the likelihood of Metastasis. Explanatory variables included TumorSize, Age, TumorType, CancerStage, and GeneticMutation. Odds Ratios (ORs) and 95% Confidence Intervals (CIs) were calculated.

Count Regression (Planned/Considered):
Predicting Chemotherapy Sessions: Given the count nature of ChemotherapySessions, Poisson or Negative Binomial regression was considered to explore factors influencing the number of sessions, depending on the observed overdispersion.

# Findings

1. Association between Metastasis and Tumor Size
A Welch Two-Sample t-test revealed a highly statistically significant difference in mean tumor size between patients with and without metastasis (p < 2.2e-16). Patients with metastasis had consistently larger tumors (mean 7.32 units) compared to those without (mean 5.98 units). This suggests a strong link between increased tumor burden and the presence of metastatic disease.

2. Impact of Cancer Stage on Overall Survival
Kaplan-Meier analysis showed highly significant association between cancer stage at diagnosis and overall survival (p < 0.0001). Patients diagnosed at earlier stages, particularly Stage I, consistently exhibited  higher survival probabilities over time compared to those with advanced disease. This finding highlights the critical role of early detection in improving long-term cancer outcomes for this patient cohort.

3. Independent Predictors of Overall Survival (Cox Model)
The multivariable Cox proportional hazards model showed overall statistical significance for predicting survival (p < 2e-16), and its proportional hazards assumption held. However, coefficients for Cancer Stage were unstable. Other individual covariates in this specific model iteration did not show statistical significance.

4. Factors Predicting Likelihood of Metastasis (Logistic Regression)
The logistic regression model significantly improved upon a null model in predicting metastasis. However, it also suffered from severe numerical instability for Cancer Stage, with inflated odds ratios suggesting quasi-complete separation. This instability prevented reliable interpretation of other covariates, which otherwise did not show individual significance in this model.

