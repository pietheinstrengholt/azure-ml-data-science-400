# Setup Lab 03

**Contents**

- [Lab Setup](#lab-setup)
   - [Task 1: Create a Compute Instance](#task-1-create-a-compute-instance)
   - [Task 2: Import the Lab Notebooks](#task-2-import-the-lab-notebooks)
- [Additional Lab Requirements](#additional-lab-requirements)

## Lab Setup

### Task 1: Create a Compute Instance

Already done in Lab03

### Task 2: Import the Lab Notebooks

Already done in Lab03

## Additional Lab Requirements

The lab will create the following resources on the fly and thus we need to ensure necessary quota for the lab user.

- AML compute cluster vm_size='Standard_DS12_v2', max_nodes=2
- An AKS compute cluster with at least 1 Gb RAM

Also, the lab will need an extract of the dataset "COVID-19 Case Surveillance Public Use Data", publicly available [here](https://raw.githubusercontent.com/pauldenoyes/tool-kit/master/COVID-19_Case_Surveillance_Public_Use_Data_shuffled_100000.csv). We need to ensure it will be available for the lab users within their Azure Machine Learning ressource.
