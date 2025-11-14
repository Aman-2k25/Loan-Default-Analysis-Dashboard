
# 📘 Loan Default Analysis & Financial Risk Dashboard

### *     End-to-End Data Analysis | Power BI Dashboard | DAX Measures | Business Insights*

## 📌 Project Overview

This project presents a comprehensive **Loan Default Analysis** using interactive dashboards built in **Power BI**.
The goal is to understand loan applicant demographics, financial behavior, credit score patterns, and default risks.

The dashboard is powered by a dataset sourced from **Microsoft Dataflow**, enriched with DAX-based calculated columns and measures, and structured into visually intuitive report pages.

This project helps financial institutions make better decisions regarding loan approvals, risk identification, and portfolio evaluation.

---

## 📂 Dataset

**Source:** Microsoft **Dataflow**
**Filename:** `Loan_default.csv`

The dataset includes:

* Applicant demographics
* Employment type
* Income & education details
* Loan amount, loan purpose, loan history
* Credit score & risk classification
* Loan default status
* Time features (Loan Date, Year)


## 🏗️ Data Preparation

### ✔️ Calculated Columns (DAX)

```DAX
-- Age Group
Age Group =
IF(Loan_default[Age]<=19,"Teen",
IF(Loan_default[Age]<=39,"Adults",
IF(Loan_default[Age]<=59,"Middle Age Adults","Senior Citizens")))

-- Year
Year = YEAR(Loan_default[Loan_Date_DD_MM_YYYY].[Date])

-- Credit Score Bins
Credit Score Bins =
IF(Loan_default[CreditScore]<=400,"Very Low",
IF(Loan_default[CreditScore]<=450,"Low",
IF(Loan_default[CreditScore]<=650,"Medium","High")))

-- Income Bracket
Income Bracket =
SWITCH(TRUE(),
Loan_default[Income] < 30000, "Low Income",
Loan_default[Income] >= 30000 && Loan_default[Income] < 60000, "Medium Income",
Loan_default[Income] >= 60000, "High Income")
```

---

## 📐 DAX Measures

### **Page 1 – Applicant Demographics & Financial Profile**

```DAX
Loan Amt by Purpose =
SUMX(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanAmount]))),
'Loan_default'[LoanAmount])

Avg Income By Employment Type =
CALCULATE(AVERAGE(Loan_default[Income]),
ALLEXCEPT('Loan_default',Loan_default[EmploymentType]))

Default Rate By Employment Type =
VAR totalrecords = COUNTROWS(ALL('Loan_default'))
VAR defaultcases = COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE()))
RETURN
CALCULATE(DIVIDE(defaultcases,totalrecords)*100,
ALLEXCEPT('Loan_default','Loan_default'[employmentType]))

Average Loan By Age Group =
AVERAGEX(VALUES(Loan_default[Age Group]),AVERAGE('Loan_default'[LoanAmount]))

Default Rate by Year =
VAR totalrows = CALCULATE(COUNTROWS('Loan_default'),
ALLEXCEPT('Loan_default',Loan_default[Year]))
VAR totaldefault =
CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
ALLEXCEPT('Loan_default',Loan_default[Year]))
RETURN DIVIDE(totaldefault,totalrows)*100
```

---

### **Page 2 – Credit Score & Education Analysis**

```DAX
Median By Credit Score Bins =
MEDIANX('Loan_default','Loan_default'[LoanAmount])

Average Loan Amount (High Credit) =
AVERAGEX(FILTER('Loan_default',Loan_default[Credit Score Bins]="High"),
Loan_default[LoanAmount])

Total Loan (Credit Bins)=
CALCULATE(SUM('Loan_default'[LoanAmount]),
'Loan_default'[Age Group]="Adult",
ALLEXCEPT(Loan_default,Loan_default[Age],Loan_default[CreditScore],
Loan_default[Credit Score Bins]))

Total Loan (Middle Aged Adults) =
SUMX(FILTER('Loan_default',Loan_default[Age Group]="Middle Aged Adults"),
'Loan_default'[LoanAmount])

Loans by Education Type =
COUNTROWS(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanID]))))
```

---

### **Page 3 – Loan Performance & Risk Metrics**

```DAX
YOY Loan Amt Change =
DIVIDE(
CALCULATE(SUM('Loan_default'[LoanAmount]),
'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))) -
CALCULATE(SUM('Loan_default'[LoanAmount]),
'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),
CALCULATE(SUM('Loan_default'[LoanAmount]),
'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),
0)

YOY Default Loan Change =
DIVIDE(
CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))) -
CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),
CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),0)

YTD Loan Amount =
CALCULATE(SUM('Loan_default'[LoanAmount]),
DATESYTD('Loan_default'[Loan_Date_DD_MM_YYYY]),
ALLEXCEPT('Loan_default',Loan_default[MaritalStatus],
Loan_default[Credit Score Bins]))
```

---

## 📊 Dashboard Overview

### **Page 1 – Applicant Demographics & Financial Profile**

![image alt](https://github.com/Aman-2k25/Loan-Default-Analysis-Dashboard/blob/main/Applicant%20Demographics%20&%20Financial%20Profile.png?raw=true)

* Median loan amount by credit score
* Loan distribution by age group
* Loan amount vs marital status
* Number of loans by education type

### **Page 2 – Financial Risk Metrics**

![image alt](https://github.com/Aman-2k25/Loan-Default-Analysis-Dashboard/blob/1bdc5858cedd990caa614d17b2f9954724cce9df/Financial%20Risk%20Metrics.png)

* YOY loan amount change
* YOY default rate change
* YTD loan amount flow chart
* Income bracket funnel

### **Page 3 – Loan Default & Overview**

![image alt](https://github.com/Aman-2k25/Loan-Default-Analysis-Dashboard/blob/main/Loan%20Default%20%26%20Overview.png)

* Loan distribution by purpose
* Default rate by employment type
* Loan amount by age group
* Default rate over years


## 🌐 Live Dashboard

🔗 **View Power BI Report:**
[https://app.powerbi.com/links/FaWzT2G7oD](https://app.powerbi.com/links/FaWzT2G7oD)

---

## 📈 Key Insights

### **🔹 Credit Score & Loan Patterns**

* Median loan amount decreases sharply from high to low credit score categories.
* Applicants with “Very Low” credit score take significantly smaller loans.

### **🔹 Employment Type & Default Risk**

* **Unemployed applicants** have the highest default rate.
* **Full-time employees** show lowest default percentage and highest average income.

### **🔹 Demographics**

* Adults and middle-aged adults form the largest loan applicant segment.
* Bachelor's and high school graduates are the largest education groups applying for loans.

### **🔹 YOY & YTD Trends**

* Peak default rates occurred around 2015–2016.
* Loan amounts show noticeable year-over-year fluctuation.

---

## 🛠️ Tools & Technologies

* **Power BI**
* **DAX**
* **Dataflow**
* **Data Modeling**
* **Dashboarding & Visualization**
* **Data Transformation**
