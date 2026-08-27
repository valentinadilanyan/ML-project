# ML-project
# USDA Foods Exploratory Data Analysis & Quality Assessment
---

## 📌 Project Overview

This project focuses on auditing and refining large-scale nutritional data provided by the USDA. Raw food databases often contain unit inconsistencies, extreme outliers, missing values, and unstandardized serving sizes. This pipeline cleans, standardizes (to 100g base reference), validates macro and micro-nutrient caps, and produces clean data ready for downstream nutritional modeling or machine learning classification.

### Key Highlights
- **Dataset Size:** 40,000 rows × 24 features
- **Initial Data Quality Completeness:** 86.26%
- **Unit Standardization:** Conversion of serving units (`g`, `ml`, `grm`, `mlt`, `gm`) to standardized metric units (`g`, `ml`).
- **Portion Normalization:** Rescaling all macronutrients and micronutrients to a standardized **100g portion**.
- **Physical Boundary Validation:** Filtering impossible physiological values (e.g., >100g of a nutrient in a 100g sample).
- **Nutrient Unit Normalization:** Standardized micronutrients (`calcium`, `iron`, `sodium`, `vitamin_c`, `cholesterol`) from milligrams (`mg`) to grams (`g`).

---

## 📁 Repository Structure

```text
.
├── EDA_usda_foods.ipynb        # Primary Analysis Notebook (by Valentina Dilanyan)
├── comprehensive_foods_usda.csv # Raw USDA Dataset (Input)
├── README.md                    # Project Documentation
└── requirements.txt             # Python Dependencies
```

---

## 📊 Dataset Schema

The raw dataset consists of 24 attributes detailing food identification, branding, serving sizes, and nutrient breakdowns:

| Feature Category | Column Name | Type | Description |
| :--- | :--- | :--- | :--- |
| **Metadata** | `fdc_id` | Integer | Unique Food Data Central ID |
| | `food_name` | String | Commercial or raw food description |
| | `data_type` | String | USDA data type (e.g., SR Legacy, Branded) |
| | `food_category` | String | USDA broad food categorization |
| | `food_type` | String | High-level food grouping (e.g., Fruits, Meat & Poultry) |
| | `brand_owner`, `brand_name` | String | Commercial brand information |
| | `ingredients` | String | Text listing of ingredients |
| **Portioning** | `serving_size`, `serving_unit` | Float/String | Raw serving size amount and unit |
| | `household_serving` | String | Household unit description (e.g., 1 cup) |
| **Macronutrients** | `calories`, `carbs_g`, `fat_g` | Float | Energy (kcal), Carbs, Fat |
| | `protein_g`, `saturated_fat_g` | Float | Protein, Saturated Fat |
| | `fiber_g`, `sugar_g` | Float | Dietary Fiber, Total Sugars |
| **Micronutrients** | `calcium_mg`, `iron_mg` | Float | Calcium, Iron |
| | `sodium_mg`, `vitamin_c_mg` | Float | Sodium, Vitamin C |
| | `cholesterol_mg` | Float | Cholesterol |
| **Target / Score** | `health_score` | Integer | Calculated health assessment score |

---

## 🧹 Data Cleaning & Preprocessing Pipeline

1. **Feature Pruning:**
   Dropped non-nutritional/sparse meta-columns: `['data_type', 'household_serving', 'brand_name', 'brand_owner']`.

2. **Unit Standardisation:**
   - Standardized unit string values: `GRM`, `gm` ➔ `g` and `MLT` ➔ `ml`.
   - Filtered out non-gram/milliliter non-standard units (`IU`, `MC`, etc.).

3. **Portion Normalization (100g Scale):**
   - Filtered out invalid/missing serving sizes (`serving_size < 1`).
   - Rescaled macronutrients to 100g reference:
     $$\text{Nutrient}_{100g} = \frac{\text{Nutrient}_{raw}}{\text{serving\_size}} \times 100$$
   - Converted micronutrients from milligrams (`mg`) to grams (`g`) and rescaled to 100g reference.

4. **Physical Sanity Filtering:**
   - Applied strict physical upper bounds: Any macronutrient value exceeding 100g per 100g food mass (`carbs_g`, `fat_g`, `protein_g`, `sugar_g`, `fiber_g`, `saturated_fat_g` > 100) was removed as measurement/data entry error.

---

## 🛠️ Tech Stack & Requirements

- **Python 3.8+**
- **Pandas** & **NumPy** — Data manipulation & numerical operations
- **Matplotlib** & **Seaborn** — Visualizations and exploratory plots
- **Scikit-Learn** & **Imbalanced-Learn** — Machine learning (KNN, Decision Trees, SMOTE)

### Installation

```bash
# Install required dependencies
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn
```

---

## 👤 Author & Acknowledgments

- **Author:** Valentina Dilanyan
- **Specialization:** Applied Statistics & Data Science
- **Data Source:** USDA FoodData Central
