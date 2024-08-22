# DSPP_Y1
Data Science Professional Practice Assignment

[Data set](https://www.kaggle.com/datasets/prasad22/healthcare-dataset?resource=download)

For this, I wanted to test if there's a relationship between days admitted and a patient's medical condition. 

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
3. Capitalise each word in Name. This was completed as an initial data cleaning step, however as the name isn't used, this wouldn't be neccesary now. 
4. For each medical condition, add a conditional column for that. Turning this variable into a binary numerical field allows for the regression to be completed during analysis
5. Remove columns that aren't needed. This reduces te file size and helps performance.  
6. Change data types of the new columns

## Data Analysis
The analysis can be found as a separate file. This includes the EDA and analysis completed. 

Initially I visualised the data with scatter plots, however this isn't helpful due to the limited values. Next I used mean averages as a descriptive statistic which showed differences better. From there I was able to complete the regressions to test for the relationship between days admitted and medical condition. This showed statisically significant results for asthma and diabetes. 
![Scatter Plot](C:\Users\charlotte.townsend\OneDrive - Osborne Clarke\Desktop\BPP\Professional Practice\Portfolio\Images\Scatter Plot.png)
## Data Engineering for Visualisation
Below is the M-Code behind the PowerQuery steps completed (after Source):

`#"Changed Type3" = Table.TransformColumnTypes(healthcare_dataset_Table,{{"Obesity", type text}, {"Hypertension", type text}, {"Diabetes", type text}, {"Cancer", type text}, {"Asthma", type text}, {"Arthritis", type text}, {"Days Admitted", Int64.Type}}),
    #"Replaced Value2" = Table.ReplaceValue(#"Changed Type3","0","No Obesity",Replacer.ReplaceText,{"Obesity"}),
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","1","Obesity",Replacer.ReplaceText,{"Obesity"}),
    #"Replaced Value4" = Table.ReplaceValue(#"Replaced Value3","0","No Diabetes",Replacer.ReplaceText,{"Diabetes"}),
    #"Replaced Value5" = Table.ReplaceValue(#"Replaced Value4","1","Diabetes",Replacer.ReplaceText,{"Diabetes"}),
    #"Replaced Value6" = Table.ReplaceValue(#"Replaced Value5","0","No Hypertension",Replacer.ReplaceText,{"Hypertension"}),
    #"Replaced Value7" = Table.ReplaceValue(#"Replaced Value6","1","Hypertension",Replacer.ReplaceText,{"Hypertension"}),
    #"Replaced Value" = Table.ReplaceValue(#"Replaced Value7","0","No Cancer",Replacer.ReplaceText,{"Cancer"}),
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","1","Cancer",Replacer.ReplaceText,{"Cancer"}),
    #"Replaced Value8" = Table.ReplaceValue(#"Replaced Value1","0","No Asthma",Replacer.ReplaceText,{"Asthma"}),
    #"Replaced Value9" = Table.ReplaceValue(#"Replaced Value8","1","Asthma",Replacer.ReplaceText,{"Asthma"}),
    #"Replaced Value10" = Table.ReplaceValue(#"Replaced Value9","1","Arthritis",Replacer.ReplaceText,{"Arthritis"}),
    #"Replaced Value11" = Table.ReplaceValue(#"Replaced Value10","0","No Arthritis",Replacer.ReplaceText,{"Arthritis"}),
    #"Renamed Columns" = Table.RenameColumns(#"Replaced Value11",{{"Days Admitted", "DaysAdmitted"}, {"Medical Condition", "MedicalCondition"}})
in
    #"Renamed Columns"`

Description of the above steps:
1. Change data type
2. All replace value steps change the binary values back to text values. This makes it more 'people-friendly' and the stakeholder will better understand these compared to binary values.
3. Renames columns ensures all the column headers are in a standard format.

I also loaded in the regression stats, however this didn't need any steps completing on it. I then created a blank dataset for measures as this is a standard practice for myself. I find this to be helpful as it keeps the data sources tidy, particularly when there's many measures from different datasets. For this dashboard, there are these 2 measures: 
1. `Average Days Admitted = average(healthcare_dataset1[DaysAdmitted])`
2. `Median Days Admitted = MEDIAN(healthcare_dataset1[DaysAdmitted])`

## Data Visualisation
Finally, the analysis results are presented through a Power BI dashboard. 
