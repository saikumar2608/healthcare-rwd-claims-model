# Medicare Part D High-Cost Patient Prediction (RWD Project)

This project predicts **high-cost Medicare Part D (prescription drug)** patients using real-world PDE (Prescription Drug Event) data and Beneficiary Summary Files.  
It replicates the workflow used in **payer analytics, PBMs, HEOR, and clinical informatics** teams at organizations like Optum, CVS Health, Elevance, Cigna, and Humana.

The project includes:
- Prescription-level → Patient-level transformation  
- Feature engineering for pharmacy utilization  
- Modeling with Logistic Regression, Random Forest, and XGBoost  
- SHAP explainability  
- Insights into cost drivers and intervention opportunities  

---

## 🔍 **1. Project Goal**

To identify members likely to become **high-cost Part D patients** so that health plans can intervene early with:
- Medication Therapy Management (MTM)
- Generic substitution programs
- High-risk pharmacy reviews
- Care coordination

---

## 📚 **2. Data Used**

**1. PDE (Prescription Drug Event) File**  
Contains every prescription filled by a Medicare beneficiary:  
- Drug code (NDC)  
- Fill date  
- Quantity  
- Days supply  
- Brand/Generic indicator  
- Out-of-pocket cost  
- Total drug cost  

**2. Beneficiary Summary File**  
Includes patient-level features:  
- Age  
- Sex  
- Race  
- ZIP code  
- ESRD indicator  
- Dual-eligibility status  
- HMO/Buy-in coverage months  

Raw data is **not uploaded to GitHub** for compliance reasons.

---

## 🔧 **3. Methodology Overview**

### **A. Data Cleaning & Preparation**
- Read PDE and Beneficiary files
- Split pipe-delimited columns
- Convert date formats
- Remove missing or malformed entries
- Merge PDE + Beneficiary on `BENE_ID`

### **B. Patient-Level Feature Engineering**
Created clinically meaningful variables:
- `TOTAL_FILLS`
- `TOTAL_QUANTITY`
- `TOTAL_DAYS_SUPPLY`
- `UNIQUE_DRUG_COUNT` (polypharmacy)
- `TOTAL_GENERIC_CLAIMS`
- `GENERIC_PERCENT`
- `TOTAL_OOP` (PTNT_PAY + TROOP)
- `TOTAL_DRUG_COST`
- `AVG_DAYS_SUPPLY`
- `IS_GENERIC` (flag)
- `SRVC_YEAR` / `SRVC_MONTH`

### **C. Defining High-Cost Patients**
Patients in the **top 10% of total drug spending** were labeled as:

---

## 🤖 **4. Machine Learning Models**

We trained 3 models:

### **1. Logistic Regression**
- Baseline interpretable model  
- AUC ≈ **0.95**

### **2. Random Forest**
- Captures nonlinear patterns  
- AUC ≈ **0.977**  

### **3. XGBoost (Final Model)**
- Highest recall  
- AUC ≈ **0.974**  
- Best real-world performance for risk detection  

---

## 📊 **5. Key Results**

### **Model Performance Comparison**

| Model | Accuracy | Precision | Recall | F1 Score | AUC |
|-------|----------|-----------|--------|----------|------|
| Logistic Regression | ~0.96 | ~0.82 | ~0.77 | ~0.79 | ~0.95 |
| Random Forest | ~0.96 | ~0.78 | ~0.78 | ~0.78 | **0.977** |
| XGBoost | ~0.96 | ~0.77 | **0.83** | **0.80** | **0.974** |

---

## 🔍 **6. SHAP Explainability (XGBoost)**

SHAP revealed the strongest drivers of high-cost predictions:

1. **Total prescription fills** (largest contributor)
2. **Unique drug count** (polypharmacy)
3. **Low generic percentage**
4. **High quantity & days supply**
5. **ESRD indicator**

These findings align with clinical and pharmacy cost literature.

Visualizations (saved under `/docs/images/`):
- SHAP summary plot  
- SHAP bar plot  
- Waterfall plot for single patient  

---

## 💡 **7. Insights & Interpretation**

### **High-fill patients = high cost**  
Frequent pharmacy activity is the strongest risk signal.

### **Polypharmacy drives spending**  
The more unique medications a patient uses, the higher their total cost.

### **Generic use reduces risk**  
Low generic utilization correlates with costly brand-name drugs.

### **Chronic conditions matter**  
ESRD and long-term therapy patterns significantly increase drug spending.

---

## 🏥 **8. Real-World Impact**

Health plans can use this model to:

- Flag high-risk Part D members early  
- Prioritize MTM and pharmacist interventions  
- Encourage generic substitution  
- Improve formulary decisions  
- Reduce preventable drug spending  

This directly maps to real workflows in PBMs and payer organizations.

---

## 📁 **9. Repository Structure**

/notebooks/
Medicare_PartD_RWD_Project.ipynb

/docs/
images/ # SHAP plots, feature importance charts
summary/ # optional summary documents

/src/
utils.py # helper functions (optional)

LICENSE
README.md

---

## 🚀 **10. Future Enhancements**

- Add multi-year PDE data  
- Sequence modeling (LSTM for longitudinal drug patterns)  
- Fairness analysis  
- Cost bucket prediction instead of binary label  
- Integrate diagnosis & medical claims  

---

## 🙌 **11. Author**

**Sai Kumar Chary Sripathi**  
Health Data Science | RWE | Clinical Analytics  
LinkedIn: *your LinkedIn URL here*

---

## 📜 **12. License**

This project is licensed under the **MIT License**.

