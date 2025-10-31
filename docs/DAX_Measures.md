# Power BI COVID-19 Healthcare Dashboard – DAX Measures

This file documents all the measures, their purpose, and logic used in the dashboard.

---

### 1. COVID Patients
```DAX
COVID patients =
CALCULATE(
    DISTINCTCOUNT(conditions[PATIENT_id]),
    conditions[DESCRIPTION] = "COVID-19"
)
```
Result: Counts unique patients diagnosed with COVID-19.

### 2. COVID Dead Patients
```DAX
COVID Dead patients =
CALCULATE(
    DISTINCTCOUNT(patients[PATIENT_Id]),
    NOT(ISBLANK(patients[DEATHDATE])),
    FILTER(conditions, conditions[DESCRIPTION] = "COVID-19")
)
```
Result: Counts patients who died and had COVID-19 as a condition.

### 3. Dead Patients
```DAX
Dead patients =
CALCULATE(
    DISTINCTCOUNT(patients[PATIENT_Id]),
    NOT(ISBLANK(patients[DEATHDATE]))
)
```
Result: Counts all patients with a recorded death date.

### 4. Deaths Over Time
```DAX
Deaths Over Time =
CALCULATE(
    DISTINCTCOUNT(Patients[PATIENT_ID]),
    FILTER(
        Patients,
        NOT(ISBLANK(Patients[DeathDate])) &&
        Patients[DeathDate] >= MIN(DateTable[Date]) &&
        Patients[DeathDate] <= MAX(DateTable[Date])
    )
)
```
Result: Tracks patient deaths over time using the date table.

### 5. Hospitalized COVID Patients
```DAX
Hospitalized COVID Patients =
CALCULATE(
    DISTINCTCOUNT(encounters[PATIENT_ID]),
    encounters[ENCOUNTERCLASS] = "inpatient",
    FILTER(conditions, conditions[DESCRIPTION] = "COVID-19")
)
```
Result: Counts inpatients diagnosed with COVID-19.

### 6. ICU Admissions
```DAX
ICU Admissions =
CALCULATE(
    DISTINCTCOUNT(encounters[PATIENT_Id]),
    FILTER(
        ALL(encounters[DESCRIPTION]),
        CONTAINSSTRING(encounters[DESCRIPTION], "intensive care")
    )
)
```

### 7. Selected Metric Value
```DAX
Selected Metric Value =
SWITCH(
    SELECTEDVALUE('Metric Selector'[Metric]),
    "Total patients", [Total Patients],
    "COVID patients", [COVID Patients],
    "Hospitalized patients", [Hospitalized Patients],
    "Hospitalized COVID Patients", [Hospitalized COVID Patients],
    "ICU Patients", [ICU Patients],
    "Dead Patients", [Dead patients],
    "COVID Dead Patients", [COVID Dead patients],
    "Dead ICU Patients", [Dead ICU Patients],
    BLANK()
)
```
Result: Allows dynamic KPI switching based on the selected metric parameter.

### 8. Dynamic Patients Title
```DAX
Dynamic patients Title =
SELECTEDVALUE('Metric Selector'[Metric], "Blank") &
" distribution " &
" - Gender: " & SELECTEDVALUE(patients[GENDER], " All")
```
Result: Generates dynamic chart titles based on selected metric and gender.

### 9. Total Comorbidities
```DAX
Total Comorbidities =
CALCULATE(
    DISTINCTCOUNT(conditions[DESCRIPTION]),
    conditions[DESCRIPTION] <> "COVID-19"
)
```
Result: Counts unique comorbidity types, excluding COVID-19.

### 10. Comorbidities Per Patient
```DAX
Comorbidities Per Patient =
VAR pid = patients[PATIENT_Id]
RETURN
CALCULATE(
    DISTINCTCOUNT(conditions[DESCRIPTION]),
    conditions[PATIENT_Id] = pid,
    conditions[DESCRIPTION] <> "COVID-19"
)
```
Result: Calculates the number of comorbidities each patient has.

### 11. Comorbidity Bucket
```DAX
Comorbidity Bucket =
SWITCH(
    TRUE(),
    patients[Comorbidities Per Patient] = 0, "0",
    patients[Comorbidities Per Patient] = 1, "1",
    patients[Comorbidities Per Patient] = 2, "2",
    patients[Comorbidities Per Patient] = 3, "3",
    patients[Comorbidities Per Patient] >= 4 && patients[Comorbidities Per Patient] <= 5, "4-5",
    patients[Comorbidities Per Patient] >= 6 && patients[Comorbidities Per Patient] <= 7, "6-7",
    patients[Comorbidities Per Patient] >= 8, "8+",
    "Unknown"
)
```
Result: Groups patients by comorbidity count for easier visualization.

### 12. Metric Selector Table
```DAX
Metric Selector =
DATATABLE(
    "Metric", STRING,
    {
        {"Total patients"},
        {"Covid patients"},
        {"Hospitalized Patients"},
        {"Hospitalized COVID Patients"},
        {"ICU Patients"},
        {"Dead Patients"},
        {"COVID Dead Patients"},
        {"Dead ICU Patients"}
    }
)
```
Result: Creates a parameter table to switch between different KPIs in visuals.
