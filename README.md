# AKG Data Dictionary

## Overview
The AKG Data Dictionary is a metadata documentation solution built to provide complete visibility into all tables, fields, calculations and dependencies within the Snowflake Data Warehouse.
It serves as a central reference for IT and BI, reducing time spent searching for fields, validating logic, or tracking dependencies.

## Business Problem
Prior to this project, the organisation did not have a single, consolidated reference for data definitions or transformation logic. This created multiple challenges:
  - New analysts struggled to understand the data landscape without a clear reference source.
  - Data knowledge was siloed across teams and undocumented.
  - Calculated fields and business metrics were not consistently defined.
  - Developers often recreated logic because no central record existed.
  - Changing data structures was risky due to unknown downstream dependencies.

## Solution
The AKG Data Dictionary centralises and documents:  
    
  1. All Snowflake Tables & Fields
Extracted directly from Snowflake metadata (e.g., INFORMATION_SCHEMA.COLUMNS), including:
  - Schema
  - Table type (base table vs. view)
  - Column names
  - Data types

2. Calculated Fields  
The dictionary identifies which fields are derived versus raw, making KPI logic, flags and metrics fully transparent.

3. Filters & Business Rules  
If logic requires filters or special rules, they are captured in dedicated fields.

4. Dependencies  
The tool records where each field originates and which upstream/downstream dependencies there are, such as:
  - CTEs
  - Other views or base tables
  - Presentation layer tables

5. Searchable Structure  
The final dictionary is stored in a format that allows users to quickly:
  - Search for a field name
  - Identify whether it is calculated
  - View the SQL logic
  - Understand dependencies across layers
  - Confirm where the “source of truth” resides

6. Maintaining the Data Dictionary   
When new fields were added or existing logic was changed by the business, a ticket would be raised for the Data Dictionary to be updated, ensuring it was always up-to-date.


## Technical Approach
1) Automated extraction of metadata from Snowflake using INFORMATION_SCHEMA function.
2) SQL logic used to: Identify calculated fields, capture calculation expressions from definitions.
3) Delivered as a structured, tabular dictionary on Excel and integrated into AKG sharepoint.

| Table Schema | Table Name | Table Type | Column Name |  Data Type  |  Calculated Field  |  Calculation  |  Filters  |  Dependance  |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| STAGING  |	VW_DAILY_REPORT  |  VIEW  |  DISENGAGED_60_V1  |  TEXT  |	 Y  |	 "case when datediff(day, cac.appointment_completed_date, current_date()) >= 60 and datediff(day, cen.STATUS_CHANGE_DATE,current_date()) >= 28 then 'Yes' else 'No' end"  |  |  cte_appointment_completed, CTE_ENERGISE  |
|  STAGING  |  VW_DIM_PARTICIPANT  |  BASE TABLE  |  DISENGAGED_60_V1  |  TEXT  |  N  |   |   |  VW_DAILY_REPORT  |
|  PRESENTATION  |  DIM_PARTICIPANT  |  BASE TABLE  |  DISENGAGED_60_V1  |  TEXT  |  N  |   |   |  VW_DAILY_REPORT  |
---			


## Impact  
The AKG Data Dictionary delivered several meaningful business and technical benefits:
  - Reduced time spent searching for data by IT/BI teams.
  - Improved consistency and accuracy in reporting and metrics.
  - Enabled faster onboarding for new team members.
  - Streamlined data governance processes.
  - Enhanced visibility into Snowflake schemas and data lineage.
  - Minimised risk when altering business logic.
  - Reduced duplication of metrics, calculations, and logic

## Future Enhancements  
  - Automated refresh pipeline for continuous metadata updates.
  - Integration with data cataloging tools (e.g., Alation, Collibra, Atlan).
  - Field-level lineage visualisation.
