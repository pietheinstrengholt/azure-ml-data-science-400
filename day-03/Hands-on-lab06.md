# Lab 06 - Using AML from Synapse Analytics Studio

## Notes (will be removed)
DataSet
Hardware part performance (https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/machineData.csv)
Will need to take a subset of the above data to use as the R&D table

## Lab overview

Azure Synapse Analytics is an enterprise-grade analytics service and a one-stop-shop for all your analytical processing needs. Azure Synapse Analytics combines enterprise data warehousing, industry-leading SQL (both serverless and dedicated), Spark technologies, ETL/ELT pipelines, and deep integrations with other Azure services such as Power BI, Cosmos DB, and Azure ML. All of these integrations and capabilities are made available via a single user interface, Synapse Studio. The focus of this lab is to explore the integration of Azure Machine Learning with Azure Synapse Analytics.

Contoso Hardware is in the business of building high-performing hardware modules for smart factory equipment. Contoso has partnered with many vendors to obtain critical compute components that provide processing power for initial signal acquisition and processing of vibration-related data, namely displacement (Peak to Peak), Velocity (Peak), Acceleration (True Peak), and High-Frequency Accelerations (RMS). This data is ultimately used for predictive maintenance functions, performing specific actions, and initiating alerts should failure conditions arise. Therefore, it is vital that Constoso Hardware's product yield the best performance possible for its form factor.

Contoso Hardware Research and Development has been tasked with training a machine learning model that can quickly assess the estimated relative performance of a vendor compute module. Identifying the features that lead to a more performing compute module can assist in the design of new hardware and also the quick elimination of candidate hardware proposed by external vendors. Contoso Hardware has provided the following dataset structure to assist with this effort.

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

The integration of Azure Machine Learning workspace with Azure Synapse Analytics is made possible through a linked service. A linked service is essentially a connection string from Azure Synapse Analytics to external resources. This exercise demonstrates establishing security for the integration as well as the creation of the linked service.

### Task 1: Grant Contributor rights to the Azure Machine Learning Workspace to the Synapse Workspace Managed Identity

Azure Synapse Analytics supports the concept of a Managed Identity. This identity is automatically created when the Synapse Workspace is deployed and represents the Azure Synapse Analytics workspace when interacting with other Azure Resources. Data pipelines defined within Azure Synapse Analytics will run under the context of this identity; therefore, it must be granted appropriate permissions to all the integrations contained within the pipeline. These permission requirements are equally valid for the Azure Machine Learning integration.

In this task, the Synapse Workspace Managed Identity will be granted **Contributor** rights to the Azure Machine Learning workspace.

1. In the Azure Portal, open the lab resource group, then select the **Machine Learning** resource named **amlworkspace{SUFFIX}**.

2. From the left menu, select **Access Control (IAM)**

3. Expand the **Add** button and choose **Add role assignment**.

    ![The AML Workspace resource screen displays with Access control (IAM) selected from the left menu. The Add menu is expanded with the Add role assignment item chosen.](media/amlworkspace_iam_addroleassignment_menu.png "Add role assignment menu")

4. In the **Add role assignment** blade, populate the form as follows, then select **Save**:

    | Field | Value |
    |-------|-------|
    | Role | Select **Contributor** |
    | Assign access to | Select **User, group, or service principal** |
    | Select | Enter the name of the Synapse Workspace **synapseworkspace{SUFFIX}**, then select it from the search results |

    ![The Add role assignment blade displays with the above values.](media/amlworkspace_addrole_synapse.png "Add role assignment blade")

### Task 2: Create a linked service in Azure Synapse Analytics to connect with the Azure Machine Learning workspace

The Azure Synapse Analytics managed identity now has Contributor access to the Azure Machine Learning Workspace. To complete the integration between these products, we will establish a linked service between both resources.

1. In the Azure portal, open the lab resource group and select the **Synapse Workspace** resource named **synapseworkspace{SUFFIX}**.

2. Under the **Getting started** section, select the **Open** link on the card titled **Open Synapse Studio**.

    ![The Synapse workspace resource screen is shown with the Open Synapse Studio card highlighted beneath the Getting started heading.](media/synapseworkspace_opensynapsestudio.png "Open Synapse Studio")

3. From the left menu, select the **Manage** hub.

4. From the Manage Hub menu, select **Linked Services**.

5. Select **New** from the Linked Services toolbar menu.

    ![Synapse Studio displays with the Manage hub selected in the left menu and Linked services selected in the middle menu. The New button is highlighted on the toolbar.](media/synapse_newlinkedservice_menu.png "New Linked service menu")

6. In the New linked service blade, search for and select **Azure Machine Learning**.

   ![The new linked service blade displays with Azure Machine Learning in the search box and Azure Machine Learning selected from the search results.](media/newlinkedservice_search_aml.png "New linked service search")

7. In the New linked service (Azure Machine Learning) blade, populate the form as follows, then select **Create**:
  
    | Field | Value |
    |-------|-------|
    | Name | amlworkspace |
    | Azure Machine Learning workspace selection method | From Azure subscription |
    | Azure Subscription | Select the lab subscription. |
    | Azure Machine Learning workspace name | Select the **amlworkspace{SUFFIX} resource. |

    ![The New linked service (Azure Machine Learning) blade displays with the form pre-populated with the previous values. The Create button is highlighted.](media/newamllinkedservice.png "New linked service blade")

8. From the Synapse workspace toolbar, select **Publish all** to save the linked service, then select **Publish** on the pending changes blade.

    ![Publish all is highlighted on the Synapse workspace toolbar.](media/publishall_linkedservice.png "Publish all")

## Exercise 2: Ingest data

Contoso Hardware Research and Development has been provided a sample dataset in CSV format to use when training the machine learning model. First, this data needs to be ingested. This exercise uses the Apache Spark pool in Azure Synapse Analytics to ingest the dataset and persist the data as a Spark table. From a physical perspective, when a Spark table is persisted, the data is stored in parquet format in the Synapse Workspace storage account.

### Task 1: Ingest dataset into a Spark table

TODO: Create Synapse Notebook to ingest and persist the dataset

## Exercise 3: Train a regression model

Contoso Hardware Research and Development has been tasked with training a model that will predict a compute module's estimated relative performance (ERP) based on a series of features. The dataset that has been provided is a labeled dataset; the ERP value for each compute module listed in the file is known along with its corresponding attribute values. In this exercise, the provided dataset is used to train a regression model to predict the ERP value for future hardware.

### Task 1: Create new AutoML experiment using the AML Integration with Azure Synapse Analytics

Contoso Hardware would like to streamline the process of identifying a suitable machine learning algorithm. To do this, they will leverage the AutoML capabilities in Azure Machine Learning that will evaluate multiple algorithms in parallel to determine the best fit. Furthermore, the AML integration with Azure Synapse Analytics also eases the generation of the AutoML experiment. In this task, an AutoML experiment is created on the provided dataset to obtain a regression model to predict the ERP value.

### Task 2: View details of the experiment run in AML Studio

## Exercise 4: Consume the trained regression model

Now that the best regression model has been trained and identified, it is now ready for use. Contoso Hardware has received benchmark data for prospective compute modules. This data is available as a table in the dedicated SQL Pool of Azure Synapse Analytics. In this exercise, the regression model enhances the data obtained for the prospective hardware and predicts the ERP value.

### Task 1: Enrich research and development table with the trained model

## Conclusion

This lab leveraged the integration of Azure Synapse Analytics and Azure Machine Learning to evaluate multiple regression algorithms. The best fit model was identified and used to enhance the prospective hardware datasets using SQL PREDICT.
