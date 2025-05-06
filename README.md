# Working with Data in Microsoft Fabric Eventhouse

## Overview
This lab demonstrates how to work with real-time data in Microsoft Fabric Eventhouse using both Kusto Query Language (KQL) and Transact-SQL (T-SQL). The exercise focuses on analyzing bike-sharing system data.

## Prerequisites
- Microsoft Fabric tenant
- Web browser access

## Lab Steps

### 1. Setup Environment
1. Create a new Fabric workspace with capacity
2. Create an Eventhouse with sample bike-sharing data

### 2. Query Data with KQL

```kql

// Basic data retrieval

Bikestream

| take 100

// Project specific columns

Bikestream

| project Street, No_Bikes

| take 10

// Summarize data

Bikestream

| summarize ["Total Number of Bikes"] = sum(No_Bikes) by Neighbourhood

| project Neighbourhood = case(

    isempty(Neighbourhood) or isnull(Neighbourhood),
 
    "Unidentified",
 
    Neighbourhood),
 
  ["Total Number of Bikes"]

| sort by Neighbourhood asc

// Filter data

Bikestream

| where Neighbourhood == "Chelsea"
```

### 3. Query Data with T-SQL

```sql

-- Basic data retrieval

SELECT TOP 100 * FROM Bikestream

-- Summarize and group data

SELECT
 
  CASE

    WHEN Neighbourhood IS NULL OR Neighbourhood = '' THEN 'Unidentified'

    ELSE Neighbourhood

  END AS Neighbourhood,

  SUM(No_Bikes) AS [Total Number of Bikes]

FROM Bikestream

GROUP BY CASE

          WHEN Neighbourhood IS NULL OR Neighbourhood = '' THEN 'Unidentified'

          ELSE Neighbourhood

        END

ORDER BY Neighbourhood ASC;

-- Filter grouped data

SELECT
 
  CASE

    WHEN Neighbourhood IS NULL OR Neighbourhood = '' THEN 'Unidentified'

    ELSE Neighbourhood

  END AS Neighbourhood,

  SUM(No_Bikes) AS [Total Number of Bikes]

FROM Bikestream

GROUP BY CASE

          WHEN Neighbourhood IS NULL OR Neighbourhood = '' THEN 'Unidentified'

          ELSE Neighbourhood

        END

HAVING Neighbourhood = 'Chelsea'

ORDER BY Neighbourhood ASC;
```

## Key Learnings
- Creating and configuring Eventhouse in Microsoft Fabric
- Querying real-time data with KQL
- Using T-SQL with Eventhouse (with limitations)
- Data summarization and filtering techniques
- Handling null/empty values in analytics queries

## Clean Up
1. Navigate to workspace settings
2. Select "Remove this workspace"

# 👤 Author >> Sefa Öztürk
IT Trainee | Azure Data Engineer in progress

📇 LinkedIn: https://www.linkedin.com/in/sefa-ozturk1972
