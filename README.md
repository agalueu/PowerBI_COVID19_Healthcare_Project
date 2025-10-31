# PowerBI_COVID19_Healthcare_Project

📊 **COVID-19 Healthcare Dashboard built entirely in Power BI**

This project analyzes synthetic healthcare data from the [Synthea COVID-19 dataset](https://synthea.mitre.org/downloads), focusing on patient outcomes, hospitalization, ICU admissions, and comorbidities.  
It was developed as a full Power BI project to explore the tool’s analytical and visualization capabilities without relying on SQL.

---

## 📌 Overview
The goal of this project was to replicate and expand one of my previous SQL-based analyses — this time using only **Power BI** — to test its performance and analytical flexibility.  

The dashboard provides a clear view of how COVID-19 affected patients by analyzing:
- Mortality and hospitalization rates  
- ICU vs hospitalized ratios  
- COVID-related deaths  
- Comorbidity burden and trends over time  
- Demographic distributions (age, gender)

---

## 🌐 Original Data Source
Dataset generated from **Synthea**, a synthetic patient generator for healthcare research:  
👉 [Synthea GitHub Releases – COVID-19 10K CSV (mirror, ~54 MB)](https://synthea.mitre.org/downloads)

---

## 🛠️ Tools & Technologies
- **Power BI Desktop**
- **DAX (Data Analysis Expressions)**
- **Data Modeling**
- **Data Relationships and Measures**
- **Dynamic Visuals and Parameter Controls**

---

## ❓ Key Business Questions
1. How many total patients were recorded, and what percentage tested positive for COVID-19?  
2. What are the mortality and hospitalization rates for COVID vs non-COVID patients?  
3. What is the ratio of ICU to hospitalized patients?  
4. How do comorbidities vary across patients and how many are most common?  
5. What is the trend of deaths and comorbidities over time?  
6. Are COVID-positive patients more likely to have multiple comorbidities?

---

## 📂 Repository Structure
PowerBI_COVID19_Healthcare_Project/
│
├── COVID_Healthcare_Dashboard.pbix # Power BI dashboard file
├── DAX_Measures.md # All calculated measures with explanations
├── README.md # Project documentation (this file)
└── /images # Screenshots or visuals of the dashboard


---

## 🗄 Database Schema & ERD
**Tables used:**
- `patients` — demographic data (birthdate, gender, deathdate)
- `conditions` — diagnosed conditions per patient
- `encounters` — hospital/ICU admissions and visits
- `medications` — prescribed drugs
- `procedures` — performed medical procedures
- `observations` — recorded metrics during encounters
- `dateTable` — supporting time intelligence for charts

**Relationships:**
- `patients (PATIENT_Id)` → `conditions (PATIENT_Id)` — 1:M  
- `patients (PATIENT_Id)` → `encounters (PATIENT_Id)` — 1:M  
- `patients (PATIENT_Id)` → `medications (PATIENT_Id)` — 1:M  
- `encounters (encounter_Id)` → `observations (ENCOUNTER)` — 1:M  
- `encounters (encounter_Id)` → `procedures (ENCOUNTER)` — 1:M  
- `dateTable (Date)` → `encounters (EncounterDate)` — 1:M

**Entity Relationship Diagram (ERD):**

![ERD](docs/ERD.png)

---

## 🔄 How to Reproduce
1. Download the dataset from [Synthea](https://synthea.mitre.org/downloads)  
2. Extract the CSV files and load them into Power BI  
3. Recreate the relationships as shown in the ERD above  
4. Add the DAX measures from `DAX_Measures.md`  
5. Build visuals using slicers for:
   - Gender  
   - Age  
   - Metric Selector (parameter table)
6. Customize the format (light theme, blue accent, bold KPI headers)

---

## ✅ Key Takeaways
- The population is nearly evenly split between male and female  
- Age distribution ranges from **5 to 106 years** (synthetic data)  
- **Average comorbidities per patient:** 8.58  
- Over **90% of patients have 4 or more comorbidities**  
- Clear correlation between comorbidity count and mortality  
- Power BI handled complex relationships and large data efficiently  

---

## 👤 Author
**Alberto Galue**  
📧 [albertogalue@gmail.com](mailto:albertogalue@gmail.com)  
🔗 [LinkedIn – Alberto Galue](https://www.linkedin.com/in/alberto-galue-u)

---

⭐ *If you found this project useful, consider starring the repository!*
