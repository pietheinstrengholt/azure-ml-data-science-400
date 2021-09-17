# Lab 06 - Using AML from Synapse Analytics Studio

## Lab overview

DataSet
Hardware part performance (https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/machineData.csv)
Dataset Attribute Information:
1. VendorName
2. ModelName
3. MYCT: machine cycle time in nanoseconds (integer)
4. MMIN: minimum main memory in kilobytes (integer)
5. MMAX: maximum main memory in kilobytes (integer)
6. CACH: cache memory in kilobytes (integer)
7. CHMIN: minimum channels in units (integer)
8. CHMAX: maximum channels in units (integer)
9. PRP: published relative performance (integer)
10. ERP: estimated relative performance from the original article (integer)

Will need to take a subset of the above data to use as the R&D table for 

Azure Synapse Analytics overview
Integration with AML
AutoML definition
Scenario overview (goal of the model - regression, label=ERP)
Usage of model in a PREDICT statement

## Exercise 1: Establish a linked service connection to the Azure Machine Learning workspace

Overview paragraph

### Task 1: Establish an Azure Synapse Analytics Managed Workspace Identity (MSI)
### Task 2: Grant Contributor rights to the Azure Machine Learning Workspace to the Synapse MSI
### Task 3: Create a linked service in Azure Synapse Analytics to connect with the Azure Machine Learning workspace

## Exercise 2: Ingest data

Overview paragraph

### Task 1: Ingest dataset into a Spark table

## Exercise 3: Train a regression model
### Task 1: Create new AutoML experiment using the AML Integration with Azure Synapse Analytics
### Task 2: View details of the experiment run in AML Studio

## Exercise 4: Consume the trained regression model
### Task 1: Enrich research and development table with the trained model



