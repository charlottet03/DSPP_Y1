# DSPP_Y1
Data Science Professional Practice Assignment

[Data set](https://www.kaggle.com/datasets/prasad22/healthcare-dataset?resource=download)

## Data Engineering for Analysis
Below is the M-Code behind the PowerQuery steps completed (after Source):

`#"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Name", type text}, {"Age", Int64.Type}, {"Gender", type text}, {"Blood Type", type text}, {"Medical Condition", type text}, {"Date of Admission", type date}, {"Doctor", type text}, {"Hospital", type text}, {"Insurance Provider", type text}, {"Billing Amount", type number}, {"Room Number", Int64.Type}, {"Admission Type", type text}, {"Discharge Date", type date}, {"Medication", type text}, {"Test Results", type text}}),
    #"Capitalized Each Word" = Table.TransformColumns(#"Changed Type",{{"Name", Text.Proper, type text}}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Capitalized Each Word",{{"Billing Amount", Currency.Type}}),
    #"Added Conditional Column" = Table.AddColumn(#"Changed Type2", "Male", each if [Gender] = "Male" then 1 else if [Gender] = "Female" then 0 else "Other"),
    #"Added Custom" = Table.AddColumn(#"Added Conditional Column", "Days Admitted", each Duration.Days([Discharge Date] - [Date of Admission])),
    #"Added Conditional Column8" = Table.AddColumn(#"Added Custom", "Arthritis", each if [Medical Condition] = "Arthritis" then 1 else 0),
    #"Added Conditional Column9" = Table.AddColumn(#"Added Conditional Column8", "Asthma", each if [Medical Condition] = "Asthma" then 1 else 0),
    #"Added Conditional Column10" = Table.AddColumn(#"Added Conditional Column9", "Cancer", each if [Medical Condition] = "Cancer" then 1 else 0),
    #"Added Conditional Column11" = Table.AddColumn(#"Added Conditional Column10", "Diabetes", each if [Medical Condition] = "Diabetes" then 1 else 0),
    #"Added Conditional Column12" = Table.AddColumn(#"Added Conditional Column11", "Hypertension", each if [Medical Condition] = "Hypertension" then 1 else 0),
    #"Added Conditional Column13" = Table.AddColumn(#"Added Conditional Column12", "Obesity", each if [Medical Condition] = "Obesity" then 1 else 0),
    #"Removed Other Columns" = Table.SelectColumns(#"Added Conditional Column13",{"Medical Condition", "Days Admitted", "Arthritis", "Asthma", "Cancer", "Diabetes", "Hypertension", "Obesity"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Removed Other Columns",{{"Days Admitted", Int64.Type}, {"Arthritis", Int64.Type}, {"Asthma", Int64.Type}, {"Cancer", Int64.Type}, {"Diabetes", Int64.Type}, {"Hypertension", Int64.Type}, {"Obesity", Int64.Type}})
in
    #"Changed Type1"`
    
Description of the above steps:
1. Promote first line as headers
2. Change all data types
3. Capitalise each word in Name
4. For each medical condition, add a conditional column for that
5. Remove not needed columns
6. Change data types of the new columns

## Data Analysis

## Data Engineering for Visualisation

## Data Visualisation
