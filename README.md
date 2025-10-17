# ExpertEdge Consulting Performance Insights

### 📊 Power BI | Excel | Data Analysis | Business Intelligence

This project analyzes consultant utilization, project performance, and revenue insights for **ExpertEdge Consulting Group**, a management consulting firm.  
The goal was to identify trends in project profitability, consultant performance, and client revenue distribution using real-world-style datasets.

---

## 🧠 Project Overview
ExpertEdge Consulting operates in a competitive industry where maximizing consultant utilization and project profitability is crucial.  
Our analysis focused on answering key management questions:

1. Which consultants generate the most revenue for the firm?  
2. What is the average billing rate by expertise?  
3. Are projects being completed within the allocated budget and timeframe?  
4. How are projects distributed by status (Ongoing, Completed, Pending)?  
5. How is project workload changing over time?  

---

## 🗂️ Datasets Used
We worked with **three datasets**, cleaned and merged in Excel before analysis in Power BI.

| Dataset | Description | Key Columns |
|----------|--------------|--------------|
| **Consultants.csv** | Details about consultants’ expertise, billing rates, and experience. | `ConsultantID`, `Expertise`, `HourlyRate`, `YearsExperience`, `EmploymentType` |
| **Projects.csv** | Project-level data including budget, revenue, and status. | `ProjectID`, `Client`, `ProjectBudget`, `ActualRevenue`, `Status`, `ServiceType` |
| **ProjectAssignments.csv** | Assignment-level data connecting consultants to projects. | `AssignmentID`, `ProjectID`, `ConsultantID`, `HoursWorked`, `BilledAmount` |

---

## ⚙️ Data Cleaning & Preparation
Data cleaning was done in **Excel** following best practices:
- Removed duplicates based on IDs.
- Handled missing values using mean/median imputation.
- Estimated missing budgets and billed amounts using formulae:

- ProjectBudget = HourlyRate × HoursWorked
BilledAmount = HourlyRate × HoursWorked

- Merged datasets using **VLOOKUP** on `ConsultantID` and `ProjectID`.
- Created pivot tables for early insights (revenue by service, top clients, utilization).

---

### 🧹 Data Cleaning Process (Excel)

| Step | Task | Formula / Method | Description |
|------|------|------------------|--------------|
| 1 | Replace missing **EmploymentType** values | `Find & Replace` | Missing employment types were replaced with “Contract” (most common type). |
| 2 | Handle missing **ActualRevenue** (Projects) | `IF + AND` formula | Replaced blanks in *ActualRevenue* where `Status = "Pending"` with `"TBD"`. Alternative: filter out pending projects and fill manually. |
| 3 | Merge datasets | `VLOOKUP` | Merged `ProjectAssignments.csv` and `Projects.csv` using `ProjectID`, then merged `Consultants.csv` using `ConsultantID`. |
| 4 | Fill missing **HoursWorked** values | `IF + AVERAGEIF` | Created helper column → `=IF([@[HoursWorked]]="",AVERAGEIF(ProjectData[ServiceType],[@[ServiceType]],ProjectData[HoursWorked]),[@[HoursWorked]])` → copied and replaced values. |
| 5 | Complete **BilledAmount** column | `IF + HourlyRate * HoursWorked` | Calculated billed amount per record using hourly rate and total hours worked. |
| 6 | Complete **BilledHours** column | `IF + SUMIF` | Assumed billed hours = total consultant hours for each project → `=SUMIF(ProjectData[ProjectID], [@[ProjectID]], ProjectData[HoursWorked])`. |
| 7 | Complete **ActualRevenue** column | `IF + SUMIF` | Assumed revenue equals total billed amount per project → `=SUMIF(ProjectData[ProjectID], [@[ProjectID]], ProjectData[BilledAmount])`. |
| 8 | Check for remaining blanks | `Ctrl + G → Special → Blanks` | Verified that all missing values were appropriately handled. |

---

## 📈 Power BI Dashboard
An **interactive Power BI dashboard** was developed to visualize:
- **Revenue vs Budget performance**
- **Consultant utilization rates**
- **Client contribution to revenue**
- **Service type performance**
- **Monthly workload trends**

### 🔍 Key DAX Measures:
```DAX
TotalRevenue = SUM(Projects[ActualRevenue])
AverageBillingRate = AVERAGE(Consultants[HourlyRate])
UtilizationRate = DIVIDE(SUM(Assignments[HoursWorked]), SUM(Assignments[AvailableHours])) * 100



