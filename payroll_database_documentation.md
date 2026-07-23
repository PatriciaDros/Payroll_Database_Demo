# Payroll Database Version 1.0
## Technical Specification

**Version:** 1.0

**Project:** Payroll Database

**Author:** Patricia Dros

**Status:** Production

---

# Table of Contents

1. Project Overview
2. Workbook Architecture
3. Worksheet Specifications
4. Table Specifications
5. Formula Catalog
6. Business Rules
7. Validation System
8. Data Flow
9. Future Enhancements

---

# 1. Project Overview

## Purpose

The Payroll Database is an operational Excel database used to manage weekly payroll processing.

Its primary functions are:

- Store imported payroll transactions
- Automatically assign payroll check numbers
- Automatically assign SCA contract numbers
- Validate payroll integrity
- Detect duplicate payroll submissions
- Support weekly payroll reporting

The workbook is designed to minimize manual data entry while ensuring payroll accuracy through validation and exception reporting.

---

## Scope

Included in Version 1.0

- Payroll processing
- Check Entry
- Job List
- Building Locations
- Validation
- Weekly Reports

Not Included

- Data Warehouse
- Fact Tables
- Dimension Tables
- Labor Rate Engine
- Fixed Fee Billing
- Laboratory Billing

---

# 2. Workbook Architecture

| Worksheet | Purpose |
|-----------|---------|
| PAYROLL MASTER | Central payroll transaction table |
| CHECK ENTRY | Payroll check control table |
| JOB LIST | Job reference table |
| BUILDING LOC | Building reference table |
| VALIDATION | Data integrity checks |
| TECHNICIAN | Technician reference data |
| RATES | Technician rate reference |
| BELLOS CHECKS | Manager payroll checks |
| SAI REPORT | Weekly SAI reporting |
| DOE REPORT | Weekly DOE reporting |
| DATA DICTIONARY | Workbook documentation |

---

# 3. Worksheet Specifications

---

## PAYROLL MASTER

### Purpose

The central transactional table of the Payroll Database.

Each row represents one payroll transaction.

### Table

tblPayroll

### Inputs

- Payroll Import
- Job List
- Check Entry

### Outputs

- Reports
- Validation
- Payroll Analysis

---

### Columns

| Column | Data Type | Formula |
|---------|-----------|---------|
| Employee_Name | Text | |
| Job_Code | Text | |
| Hourly_Rate | Currency | |
| Check_Date | Date | |
| Date_Worked | Date | |
| Week_Ending_Date | Formula | See Formula Catalog |
| Hours_Worked | Decimal | |
| Job_No | Text | |
| Building_ID | Text | |
| Expenses | Currency | |
| Job_Type | Text | |
| Private_Client_Address | Text | |
| Notes | Text | |
| Check_No | Formula | See Formula Catalog |
| TS_Filed_Correctly | Integer (0/1) | |
| In_QB | Integer (0/1) | |
| Contract_No | Formula | See Formula Catalog |
| Accounting_Notes | Text | |
| Validation_Key_No_CheckDate | Formula | See Formula Catalog |

---

## CHECK ENTRY

### Purpose

Stores payroll check numbers and provides duplicate detection.

### Table

tblCheckEntry

### Responsibilities

- Enter payroll checks once
- Supply check numbers to Payroll
- Detect duplicate payroll submissions

---

# 4. Formula Catalog

---

## Week_Ending_Date

**Worksheet**

PAYROLL MASTER

**Table**

tblPayroll

**Column**

Week_Ending_Date

### Formula

```excel
=DATE_WORKED+(7-WEEKDAY(DATE_WORKED,1))
```

*(Replace with the exact structured-reference formula used in the workbook.)*

### Purpose

Calculates the payroll week-ending date.

### Inputs

- Date_Worked

### Returns

Week-ending date

---

## Check_No

**Worksheet**

PAYROLL MASTER

**Table**

tblPayroll

**Column**

Check_No

### Formula

```excel
=XLOOKUP(
    [@[Validation_Key_No_CheckDate]],
    tblCheckEntry[Validation_Key_No_CheckDate],
    tblCheckEntry[Check_No],
    "",
    0
)
```

### Purpose

Retrieves the payroll check number from Check Entry.

### Business Rule

Check numbers are entered only once.

Payroll automatically retrieves them.

### Dependencies

- tblCheckEntry

---

## Contract_No

**Worksheet**

PAYROLL MASTER

**Table**

tblPayroll

**Column**

Contract_No

### Formula

```excel
=IF(
    TRIM([@[JOB_TYPE]])="SCA",
    XLOOKUP(
        TRIM([@JOBID]),
        tblJobList[JOBID],
        tblJobList[Con_No],
        "no contract",
        0
    ),
    ""
)
```

### Purpose

Retrieves the SCA Contract Number.

### Business Rule

Only SCA jobs receive Contract Numbers.

---

## Validation_Key_No_CheckDate

### Formula

```excel
=[@[EMPLOYEE_NAME]]
& "|"
& [@[JOB_TYPE]]
& "|"
& TEXT([@[WEEK_ENDING_DATE]],"yyyymmdd")
```

### Purpose

Creates the lookup key used by Payroll and Check Entry.

---

## Count_Week_Ending

### Formula

```excel
=COUNTIFS(
    [Employee Name],[@[Employee Name]],
    [Week Ending Date],[@[Week Ending Date]],
    [Job Type],[@[Job Type]]
)
```

### Purpose

Counts duplicate payroll records for investigation.

---

# 5. Business Rules

## Contract Numbers

Only SCA jobs receive Contract Numbers.

---

## Check Numbers

Entered only in Check Entry.

Never manually entered in Payroll.

---

## Duplicate Payroll

Duplicates are not automatically errors.

Each duplicate must be investigated.

The Notes field records the outcome.

---

# 6. Validation System

The VALIDATION worksheet confirms that Payroll and Check Entry remain synchronized.

Validation uses exception reporting rather than manual review.

Expected Result:

No exceptions.

A blank validation report indicates successful reconciliation.

---

# 7. Data Flow

```text
Payroll Import
      │
      ▼
tblPayroll
      │
      ├──────────────► tblJobList
      │                     │
      │                     └── Contract_No
      │
      ├──────────────► tblCheckEntry
      │                     │
      │                     └── Check_No
      │
      ▼
Validation
      │
      ▼
Reports
```

---

# 8. Future Enhancements

Items considered but intentionally deferred from Version 1.0:

- Employee Master (`tblEmployee`)
- Employee License Tracking (`tblEmployeeLicense`)
- Employee IDs replacing employee names as long-term keys
- Expanded Technician reference data
- Additional automation and reporting

---

# Document History

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | July 2026 | Initial technical specification for Payroll Database Version 1.0 |
