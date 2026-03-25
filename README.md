# 30-Day Mortality Rate Prediction for Acute Myocardial Infarction (AMI)

## Project Overview

This project analyzes US CMS (Centers for Medicare & Medicaid Services) hospital data to predict 30-day mortality rates following Acute Myocardial Infarction (heart attack). Using the CRISP-DM methodology, we explore hospital characteristics, quality metrics, and spending patterns to build predictive models that identify factors associated with better or worse patient outcomes.

## Motivation

Understanding factors that influence 30-day AMI mortality rates is crucial for:
- **Healthcare administrators** optimizing hospital operations and resource allocation
- **Policy makers** designing interventions to improve cardiac care quality
- **Hospital quality improvement teams** identifying benchmarks and best practices
- **CMS and insurers** evaluating hospital performance and reimbursement models
- **Patients and families** making informed decisions about where to seek cardiac care

## Business Questions

This analysis addresses five key questions:

1. **Which hospital characteristics most strongly predict 30-day AMI mortality rates?**
2. **How do mortality rates vary by hospital type, ownership, and geographic location?**
3. **Can we accurately predict mortality rates using quality metrics and spending data?**
4. **Which features provide the most actionable insights for hospital improvement?**
5. **What patterns exist in the relationship between hospital spending and mortality outcomes?**

## Data Sources

The project uses four CMS datasets:

1. **Medicare Hospital Spending Per Patient** - Cost metrics across care episodes
2. **Hospital General Information** - Facility characteristics (type, ownership, location)
3. **Unplanned Hospital Visits** - Readmission and emergency department metrics
4. **Complications and Deaths** - Mortality rates for various conditions including AMI

**Target Variable**: `MORT_30_AMI` - 30-day mortality rate for acute myocardial infarction patients

## Libraries Used

- **pandas** - Data manipulation and analysis
- **numpy** - Numerical computations and array operations
- **scikit-learn** - Machine learning models, preprocessing, and evaluation
- **matplotlib** - Data visualization and plotting
- **joblib** - Model serialization and persistence

## Project Structure

```
data_science_project/
├── data/
│   ├── Complications_and_Deaths-Hospital.csv
│   ├── Hospital_General_Information.csv
│   ├── Medicare_Hospital_Spending_Per_Patient-Hospital.csv
│   └── Unplanned_Hospital_Visits-Hospital.csv
├── notebooks/
│   └── Mortality_30Day_AMI_AllSteps.ipynb    # Main analysis notebook
├── pkl/                                        # Saved models and artifacts
│   ├── elasticnet_final.pkl
│   ├── num_imputer.pkl
│   ├── cat_imputer.pkl
│   ├── scaler.pkl
│   └── top200_feature_list.json
├── venv/                                       # Python virtual environment
├── requirements.txt                            # Python dependencies
├── README.md                                   # This file
└── CLAUDE.md                                   # Development guidelines
```

## Installation and Setup

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Setup Steps

1. **Clone or download the repository**

2. **Create and activate virtual environment** (already created):
   ```bash
   # On Windows (Git Bash)
   source venv/Scripts/activate

   # On Windows (Command Prompt)
   venv\Scripts\activate

   # On Windows (PowerShell)
   .\venv\Scripts\Activate.ps1
   ```

3. **Install required packages**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Launch Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```

5. **Open the analysis notebook**: Navigate to `notebooks/Mortality_30Day_AMI_AllSteps.ipynb`

## Methodology: CRISP-DM Process

This project follows the **CRISP-DM** (Cross-Industry Standard Process for Data Mining) framework:

### 1. Business Understanding
- Define prediction objective: 30-day AMI mortality rates
- Identify stakeholders and use cases
- Formulate 5 business questions

### 2. Data Understanding
- Explore 4 CMS hospital datasets
- Analyze feature distributions and correlations
- Identify missing data patterns
- Examine target variable characteristics

### 3. Data Preparation
- Clean and standardize facility IDs across datasets
- Pivot time-series data to latest measurements
- Join datasets into unified analytical table
- Handle missing values (>50% missing → drop, else impute)
- Encode categorical variables (hospital type, ownership, state)
- Scale numerical features using StandardScaler

### 4. Modeling
- Baseline: Linear Regression
- Advanced models: ElasticNet, Random Forest, Gradient Boosting
- Hyperparameter tuning with validation set
- Feature selection (top 200 by correlation)

### 5. Evaluation
- Metrics: RMSE, MAE, R² Score
- Residual analysis
- Actual vs Predicted scatter plots
- Feature importance analysis
- Model comparison and selection

### 6. Deployment
- Save best model and preprocessing artifacts
- Generate predictions for new data
- Document findings and recommendations

## Key Features

### Data Processing
- **Automated data cleaning** with standardized facility ID matching
- **Intelligent missing value handling** using median/mode imputation
- **Feature engineering** with one-hot encoding for categorical variables
- **Dimensionality reduction** via correlation-based feature selection

### Modeling Approach
- **Multiple algorithm comparison** (Linear, ElasticNet, Random Forest, GBM)
- **Systematic hyperparameter tuning** with validation set
- **Reproducible results** with fixed random seeds
- **Model persistence** for deployment

### Analysis Components
- **Exploratory Data Analysis** with comprehensive visualizations
- **Correlation analysis** identifying key predictive features
- **Statistical summaries** of target variable distribution
- **Geographic and categorical comparisons**

## Summary of Results

### Model Performance

**Best Model: ElasticNet Regression**
- **RMSE**: 1.002 (approximately 1 percentage point error)
- **MAE**: 0.779 (average absolute error)
- **R² Score**: 0.262 (26.2% of variance explained)

**Model Comparison:**
| Model | RMSE | MAE | R² |
|-------|------|-----|-----|
| ElasticNet | 1.002 | 0.779 | 0.262 |
| Random Forest | 1.023 | 0.786 | 0.231 |
| Gradient Boosting | 1.039 | 0.800 | 0.206 |
| Linear Regression | 1.009 | 0.781 | 0.252 |

### Key Findings

1. **Mortality rates cluster around 12%** with relatively low variance (mean: 12.1%, std: 1.2%)

2. **Strong predictors include:**
   - Other mortality metrics (MORT_30_PN, MORT_30_HF, MORT_30_COPD)
   - Patient safety indicators (PSI scores)
   - Spending efficiency metrics (Hybrid_HWM)

3. **Data limitations:**
   - Target variable available for only 36% of hospitals (1,953 of 5,418)
   - High missingness in surgical metrics (CABG, hip/knee)
   - Model explains only 26% of variance, suggesting mortality depends on factors not captured in administrative data

4. **Geographic and structural patterns:**
   - Mortality varies by state and hospital ownership type
   - Emergency services capability not strongly predictive
   - Hospital type shows limited direct correlation

### Recommendations

**For Hospital Administrators:**
1. Focus on reducing complications (PSI metrics) as they correlate with mortality
2. Monitor pneumonia and heart failure mortality rates as leading indicators
3. Implement spending efficiency programs (Hybrid metrics)

**For Policy Makers:**
1. Invest in data collection for missing hospital segments
2. Consider patient case-mix adjustments in quality metrics
3. Explore unmeasured factors (staffing ratios, technology, protocols)

**For Model Improvement:**
1. Incorporate patient-level data (age, comorbidities, severity)
2. Add temporal features (trends over time)
3. Include staffing and technology variables
4. Consider ensemble methods combining predictions

### Limitations

1. **Missing data**: 64% of hospitals lack target variable
2. **Ecological fallacy**: Hospital-level analysis may not reflect patient-level patterns
3. **Unmeasured confounders**: Patient demographics, case severity, clinical protocols not included
4. **Temporal dynamics**: Cross-sectional analysis doesn't capture trends
5. **Model interpretability**: 74% of variance unexplained suggests complex causality

## Usage

To reproduce the analysis:

1. Ensure all dependencies are installed (see Setup Steps)
2. Verify data files are in the `data/` directory
3. Open `notebooks/Mortality_30Day_AMI_AllSteps.ipynb`
4. Run all cells sequentially (Cell → Run All)
5. Review outputs, visualizations, and model results

## Model Artifacts

Trained models and preprocessing objects are saved in `pkl/`:
- `elasticnet_final.pkl` - Final trained ElasticNet model
- `num_imputer.pkl` - Numerical feature imputer
- `cat_imputer.pkl` - Categorical feature imputer
- `scaler.pkl` - StandardScaler for numerical features
- `top200_feature_list.json` - Selected feature names

To load and use the model:
```python
import joblib
import pandas as pd

# Load artifacts
model = joblib.load('pkl/elasticnet_final.pkl')
scaler = joblib.load('pkl/scaler.pkl')
num_imputer = joblib.load('pkl/num_imputer.pkl')

# Prepare new data and predict
# (Follow preprocessing steps in notebook)
```

## Code Quality Standards

This project adheres to:
- **PEP 8** style guidelines for Python code
- **DRY principles** (Don't Repeat Yourself) with reusable functions
- **Comprehensive documentation** with docstrings for all functions
- **Clear variable naming** using descriptive snake_case
- **Modular code structure** with logical separation of concerns

## Acknowledgments

- **Data Source**: CMS Hospital Compare Data (https://data.cms.gov/)
- **Methodology**: CRISP-DM Framework
- **Python Ecosystem**: pandas, scikit-learn, matplotlib development teams

## License

This project is for educational purposes as part of a data science course.

## Contact

For questions or feedback about this analysis, please refer to the project documentation or course materials.

---

**Note**: This is an academic project for learning data science and machine learning techniques. Model predictions should be interpreted in an educational context and validated with domain expertise before any clinical application.
