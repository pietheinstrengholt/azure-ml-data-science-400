# Lab 06 - Using AML from Synapse Analytics Studio

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

### Task 1: Grant Storage Blob Contributor to the user account

When executing a Synapse Notebook it will run under the context of the user executing the notebook. Therefore, the current user needs to have sufficient access to the storage account to write the Spark table parquet files.

1. In the Azure Portal, open the lab resource group, and select the **datalake{SUFFIX}** storage account.

2. From the left menu, select **Access control (IAM)**.

3. Expand the **+ Add** button and select **Add role assignment**.

4. In the **Add role assignment** blade, select **Storage Blob Data Contributor**.

5. In the **Select**, search for and select your user account, then select **Save**.

    ![The Add role assignment blade displays with the previous values.](media/addusertoblobstoragecontributor.png "Add Storage Blob Data Contributor")

### Task 2: Ingest dataset into a Spark table

This task demonstrates ingesting CSV data and storing the results as a Spark table.

1. In Synapse Studio, select the **Develop** hub from the left menu.

2. Expand the **+** menu on the Develop menu, and select **Notebook**.

    ![Synapse Studio displays with the Develop hub selected from the left menu, and the + button expanded with the Notebook item highlighted.](media/synapse_createnotebook.png "Create new Notebook")

3. In the Notebook toolbar, expand the **Attach to** drop down and select **sparkpool01**.

    ![A Synapse Notebook displays with the Attach To field set to sparkpool01.](media/synapsenotebook_attachtosparkpool01.png "Attach to sparkpool01")

4. In the first cell of the notebook, copy and paste the following code to read the CSV data and display a preview of the data:

    ```python
    import pandas as pd

    df = pd.read_csv('https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/machineData.csv')

    display(df)
    ```

5. Execute the cell by selecting the triangular play button in the cell margin. Please note that it takes a few minutes for a Spark cluster to be provisioned when the first cell is executed. Please note that it is safe to ignore the warning message displayed at the bottom of the output.

    ![The triangular play button is highlighted to the left of the cell. The output displays a sampling of the data read in from the CSV file.](media/readcsv_displaydataframe.png "Sample data display")

6. Beneath the first cell, select the **+ Code** button to add a new cell. Copy and paste the following code to create a Spark database and table for the training data, then, execute the cell.

    ```python
    spark.sql("CREATE DATABASE research_development")

    sparkdf = spark.createDataFrame(df)

    sparkdf.write.saveAsTable("research_development.compute_module_data")
    ```

    ![The notebook cell to create the Spark database and table displays that the command has been executed.](media/createsparkdatabaseandtablecellexecution.png "Create Spark database and table")

7. Close the notebook document and select the **Close + discard changes** button. There isn't a need to save this notebook.

## Exercise 3: Train a regression model

Contoso Hardware Research and Development has been tasked with training a model that will predict a compute module's estimated relative performance (ERP) based on a series of features. The dataset that has been provided is a labeled dataset; the ERP value for each compute module listed in the file is known along with its corresponding attribute values. In the previous exercise, this training data was ingested into a Spark table. In this exercise, the Spark table is used to train a regression model to predict the ERP value for future hardware.

### Task 1: Create new AutoML experiment using the AML Integration with Azure Synapse Analytics

Contoso Hardware would like to streamline the process of identifying a suitable machine learning algorithm. To do this, they will leverage the AutoML capabilities in Azure Machine Learning that will evaluate multiple algorithms in parallel to determine the best fit. Furthermore, the AML integration with Azure Synapse Analytics also eases the generation of the AutoML experiment. In this task, an AutoML experiment is created on the provided dataset to obtain a regression model to predict the ERP value.

1. In Synapse Studio, select the **Data** hub from the left menu.

2. Remaining on the **Workspace** tab, hover over the **Databases** header, then select the ellipsis button that appears to the right of the heading and select the **Refresh** button.

3. Once refreshed, the **research_development (Spark)** database is visible.

4. Expand the **research_development** database, and its **Tables** folder to reveal the **compute_module_data** table.

    ![Synapse Studio displays with the Data hub selected from the left menu. The Databases Refresh button is selected. The research_development database is expanded and the compute_module_data table is highlighted.](media/sparkdatabasetableexpansion.png "Spark database and table")

5. Hover over the **compute_module_data** table, select the ellipsis button and choose **Machine Learning**, then **Train a new model**.

    ![The ellipsis menu is expanded on the compute_module_data Spark table. The Machine Learning item is expanded with the Train a new model item highlighted.](media/sparktable_trainanewmodel.png "Train a new model")

6. In the Train a new model blade, select **Regression**, then select **Continue**.

    ![The Train a new model blade displays with the Regression model type selected.](media/amlwizard_regressionmodeltype.png "Train a new regression model")

7. In the Train a new model - Configure experiment blade, populate the form as follows and select **Continue**:

    | Field | Value |
    |-------|-------|
    | Azure Machine Learning Workspace | amlworkspace{SUFFIX} |
    | Experiment name | leave default value |
    | Best model name | leave default value |
    | Target column | ERP (long) |
    | Apache Spark pool | sparkpool01 |

    ![The configure experiment blade is shown populated with the preceding value.](media/amlwizard_configureexperiment.png "Configure experiment blade")

8. In the Train a new model - Configure regression model, populate the form as follows, then select **Open in notebook**.

    | Field | Value |
    |-------|-------|
    | Primary metric | Normalized root mean squared error |
    | Maximum training job time (hours) | 0.3 |
    | Max concurrent iterations | 4 |
    | ONNX model compatibility | Enable |

    ![The Configure regression model blade displays with the preceding values. The Open in notebook button is highlighted.](media/amlwizard_configureregressionmodel.png "Configure regression model blade")

9. A new Notebook is generated and is populated with the code that leverages the integration with Azure Machine Learning to create and run the experiment. From the Notebook toolbar, select the **Run all** button to execute the notebook.

    ![The Run all button is highlighted in the generated notebook toolbar.](media/amlwizard_generatednotebook_runall.png "Run all cells")

10. Once the 6th cell executes, the output displays a URL into the experiment in the Azure Machine Learning portal. Select this link to watch the experiment progress if desired.

    ![Cell 6 output contains a link to the Azure Machine Learning portal experiment.](media/amlexperimentlink_cell6output.png "Azure Machine Learning experiment link")

11. The experiment runs in the 7th (and last) cell. This cell will take approximately 30 minutes to complete. Please remain patient and let the notebook execution complete uninterrupted. Grab a snack and refreshment!

    ![The experiment run cell completes in 28 minutes.](media/amlnotebook_experimentcelloutput.png "Experiment completion")

12. Keep this notebook open once execution completes.

### Task 2: View details of the experiment run in AML Studio

## Exercise 4: Consume the trained regression model

Now that the best regression model has been trained and identified, it is now ready for use. Contoso Hardware has received benchmark data for prospective compute modules. This data is available as a table in the dedicated SQL Pool of Azure Synapse Analytics. In this exercise, the regression model enhances the data obtained for the prospective hardware and predicts the ERP value.

### Task 1: Enrich research and development SQL table with the trained model

## Conclusion

This lab leveraged the integration of Azure Synapse Analytics and Azure Machine Learning to evaluate multiple regression algorithms. The best fit model was identified and used to enhance the prospective hardware datasets using SQL PREDICT.
