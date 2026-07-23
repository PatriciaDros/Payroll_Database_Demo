# Data Anonymization

## Purpose

This portfolio project is based on a production payroll database used in an environmental consulting business.

To protect confidential business information and employee privacy, all personally identifiable information (PII), client identifiers, and company-specific information have been removed or replaced while preserving the workbook's structure, formulas, relationships, and business logic.

The objective was to create a realistic case study that accurately demonstrates the system's design without exposing confidential information.

---

# Anonymization Strategy

The guiding principle was:

> Preserve the data model and business logic while removing information that could identify employees, clients, projects, or company operations.

---

# Data Transformations

| Production Data | Portfolio Version | Reason |
|-----------------|------------------|--------|
| Employee Names | Employee Aliases (e.g., `CFH-008`) | Protect employee identity while preserving relationships. |
| Client Name (Public Agency) | CLIENT-A | Protect client identity while maintaining business rules. |
| Private Client Addresses | Removed | Personally identifiable information not required for the case study. |
| Internal Tracking Columns | Removed | Production-only workflow fields that do not contribute to understanding the system. |
| Historical Payroll Data | Reduced to representative sample | Creates a concise, reviewable dataset while preserving functionality. |

---

# Data Preserved

The following information remains unchanged because it is essential to demonstrating the payroll system.

- Payroll dates
- Hours worked
- Hourly rates
- Payroll types
- Job relationships
- Check numbers (anonymized where appropriate)
- Validation logic
- Lookup relationships
- Business rules
- Formulas
- Excel table structure

Hourly rates remain unchanged because they are necessary for payroll calculations and are no longer associated with identifiable employees.

---

# Columns Removed

The following production columns were intentionally excluded from the portfolio version because they represent internal operational workflow rather than payroll system functionality.

- Private_Client_Address
- Timesheet_Filed
- In_QB

Removing these columns simplifies the workbook without affecting calculations, validation, or reporting.

---

# Validation

After anonymization, the workbook was fully tested to ensure:

- All lookup formulas continued to function.
- Validation reports produced identical results.
- Check number relationships remained intact.
- Business rules were preserved.
- Workbook calculations remained unchanged.

---

# Result

The portfolio workbook represents a fully functional, anonymized subset of the production payroll system.

The case study preserves the original system architecture while protecting confidential employee, client, and company information.
