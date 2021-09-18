# Lab 06 - Using AML from Synapse Analytics Studio

## Notes (will be removed)
DataSet
Hardware part performance (https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/machineData.csv)
Will need to take a subset of the above data to use as the R&D table

Automate deployment
provide deployment for storage acct, Azure Synapse Analytics workspace (SQL pool + Spark pool), AML workspace

## Lab overview

Azure Synapse Analytics is an enterprise-grade analytics service and a one-stop shop for all your analytical processing needs. Azure Synapse Analytics combines enterprise data warehousing, industry-leading SQL (both serverless and dedicated), Spark technologies, ETL/ELT pipelines, and deep integrations with other Azure services such as Power BI, Cosmos DB, and Azure ML. All of these integrations and capabilities are made available via a single user interface, Synapse Studio. The focus of this lab is to explore the integration of Azure Machine Learning with Azure Synapse Analytics. 

Contoso Hardware is in the business of building high performing hardware modules for smart factory equipment. Contoso has partnered with many vendors to obtain critical compute components that provide processing power for initial signal acquisition and processing of vibration related data, namely displacement (Peak to Peak), Velocity (Peak), Acceleration (True Peak), and High-Frequency Accelerations (RMS). This data is ultimately used for predictive maintenance functions, performing specific actions and initiating alerts should failure conditions arise. It is important that the product that Constoso Hardware produces yield the best performance possible for its form factor.

Contoso Hardware Research and Development is tasked with training a machine learning model that can quickly assess the estimated relative performance of a vendor compute module. Identifying the features that lead to a more performing compute module can assist in the design of new hardware and also the quick elimination of candidate hardware proposed by external vendors. Contoso Hardware has provided the following dataset structure to assist with this effort.

| Field | Type | Description |
|-------|------|-------------|
| VendorName | string | |
| ModelName | string | |
| MYCT | int | machine cycle time in nanoseconds |
| MMIN | int | minimum main memory in kilobytes |
| MMAX | int | maximum main memory in kilobytes |
| CACH | int | cache memory in kilobytes |
| CHMIN | int | minimum channels in units |
| CHMAX | int | maximum channels in units |
| PRP | int | published relative performance |
| ERP | int | estimated relative performance |

## Exercise 1: Establish a linked service connection to the Azure Machine Learning workspace

The integration with the Azure Machine Learning workspace with Azure Synapse Analytics is made possible through a linked service. A linked service is essentially a connection string from Azure Synapse Analytics to external resources. This exercise demonstrates establishing security for the integration as well as the creation of the linked service.

### Task 1: Grant Contributor rights to the Azure Machine Learning Workspace to the Synapse Workspace Managed Identity

Azure Synapse Analytics supports the concept of a Managed Identity. This identity is automatically created when the Synapse Workspace is deployed and represents the Azure Synapse Analytics workspace when interacting with other Azure Resources. Data pipelines defined within Azure Synapse Analytics will run under the context of this identity, therefore it is imperative that it must be granted appropriate permissions to all the integrations contained within the pipeline. These permission requirements are equally true for the Azure Machine Learning integration.

In this task, the Synapse Workspace Managed Identity will be granted Contributor rights to the Azure Machine Learning workspace.

1. In the Azure Portal, open the lab resource group, then select the **Machine Learning** resource.
2. From the left menu, select **Access Control (IAM)**
3. Expand the **Add** button and choose **Add role assignment**.
4. In the **Add role assignment** blade, populate the form as follows, then select **Save**:

    | Field | Value |
    |-------|-------|
    | Role | Select **Contributor** |
    | Assign access to | Select **User, group, or service principal** |
    | Select | Enter the name of the Synapse Workspace, then select it from the search results |


### Task 2: Create a linked service in Azure Synapse Analytics to connect with the Azure Machine Learning workspace

The Azure Synapse Analytics managed identity now has Contributor access to the Azure Machine Learning Workspace. To complete the integration between these products, we will establish a linked service between both resources.

1. In the Azure portal, open the lab resource group and select the **Synapse Workspace** resource.
2. Select the link to open **Synapse Studio**.
3. From the left menu, select the **Manage** hub.
4. From the Manage Hub menu, select **Linked Services**.
5. Select **Add**, then search for and select **Azure Machine Learning**.
6. In the Add Linked Services blade, select the Azure Machine Learning workspace from the subscription.

## Exercise 2: Ingest data

Contoso Hardware Research and Development has been provided a sample dataset in CSV format to use when training the machine learning model. In order to make use of this data, it needs to be ingested. In this exercise, the Apache Spark pool in Azure Synapse Analytics is used to ingest the dataset and persist the data as a Spark table. From a physical perspective, when a Spark table is persisted, data is stored in parquet format in the Synapse Worspace storage account.

### Task 1: Ingest dataset into a Spark table

TODO: Create Synapse Notebook to ingest and persist the dataset

## Exercise 3: Train a regression model

Contoso Hardware Research and Development has been tasked with training a model that will predict the estimated relative performance (ERP) of a compute module based on a series of features. The dataset that has been provided is a labeled dataset, the ERP value for each compute module listed in the file is known and its corresponding attribute values recorded. In this exercise, this dataset will be used to train a regression model that is used to predict the ERP value for future hardware.

### Task 1: Create new AutoML experiment using the AML Integration with Azure Synapse Analytics

Contoso Hardware would like to streamline the process of identifing a suitable machine learning algorithm. To do this, they will leverage the AutoML capabilities in Azure Machine Learning that will evaluate multiple algorithms in parallel to determine the best fit. Furthermore, the AML integration with Azure Synapse Analytics also eases the generation of the AutoML experiment. In this task, an AutoML experiment is created on the provided dataset to obtain a regression model to predict the ERP value.

### Task 2: View details of the experiment run in AML Studio

## Exercise 4: Consume the trained regression model

Now that the best regression model has been trained and identified, it is now ready for use. Contoso Hardware has received benchmark data for prospective compute modules that is subsequently stored as a table in the dedicated SQL Pool of Azure Synapse Analytics. In this exercise, the regression model is used to enhance the data obtained for the prospective hardware and will predict the ERP value.

### Task 1: Enrich research and development table with the trained model

## Conclusion

In this lab, the integration of Azure Synapse Analytics and Azure Machine Learning was leveraged to evaluate multiple regression algorithms. The best fit model was identified and leveraged to enhance the prospective hardware datasets using SQL PREDICT.



