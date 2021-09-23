# Activity 02: Establishing a Security Baseline for Azure ML

Creating an Azure Machine Learning workspace requires the creation of several services, including the Machine Learning service, a Storage account, Application Insights, a Key vault, and a Container registry.  Each of these services includes its own security requirements, something all the more important as organizations often use sensitive or proprietary information to train machine learning models.  The purpose of this activity is to guide a customer through the process of securing an Azure Machine Learning installation by creating a security baseline.

## Creating a Baseline

For each category, use the scenario prompts to answer the question or explain what a customer can do to improve their Azure Machine Learning security posture.

### Network Security

* For employees who work in the office, what Azure services would you recommend using to improve network security between the office and their Azure Machine Learning workspace?
* What actions would you recommend the customer take to ensure that remote employees access the AML workspace securely?
* In order to train machine learning models, the customer will need to access data from Azure SQL Database.  What can they do to ensure that this connection is as secure as possible?
* How might the customer protect against a malicious actor attempting a DNS poisoning or DNS spoofing attack?

#### Workspace Virtual Network Security

* Azure Machine Learning requires some amount of inbound and outbound access to the public internet.  Which ports need to be open to inbound and outbound traffic?
* Azure Machine Learning supports storage accounts configured to use either service endpoints or private endpoints.  When would you recommend the customer use each of these?
* Can you perform validation on datastore or dataset resources after putting Azure Machine Learning on a locked-down virtual network?  If not, what can the customer do to avoid errors?

#### Training Environment Virtual Network Security

* If you configure Azure Machine Learning to run over a Virtual Network, what resources get created for each compute instance or cluster?
* What steps are required to use Jupyter Notebooks on a compute instance running on a Virtual Network?

### Identity Management

* What steps can a customer take to reduce the risk of an insufficiently strong password allowing a malicious third party to access AML resources?

### Privileged Access

* The customer's security team has indicated that they do not want to grant blanket permissions to everybody involved.  What can they use to restrict administrative access to resources?
* What are examples of access controls you have set up in the past to follow the principle of least privilege?

### Data Protection

* How would the customer be able to distinguish between sensitive internal data and less sensitive data, such as data collected from public resources?
* What can the customer do to protect data and machine learning models at rest?  Suppose that this data includes data stored in Parquet files as well as in Azure Synapse Analytics dedicated SQL pools.
* What can the customer do to protect data in transit?
* How can the customer know if somebody has performed an unauthorized transfer of sensitive data?

### Asset Management

* The customer's security team would like to know if you have any recommendations around asset management for AML resources.  What would you recommend they do?
* Are there any tools the security team can use to limit the types of resources users can create?

### Logging and Threat Detection

* What tools could the customer use to perform logging or threat detection against AML resources?
* What types of information would you recommend the customer log to support threat hunting or generation of security alerts?
* What tooling would you recommend to the customer in order to centralize their log management?

### Posture and Vulnerability Management

* What can the customer do to limit their risk of accidentally using a malware-infested container image?
* The data science team would like the authority to create and destroy compute instances on an as-needed basis, but the security team wants to limit the risk of a misconfiguration exposing sensitive data.  What recommendations do you have to reduce this risk?

### Backup and Recovery

* For the following AML resources, which are geo-replicated by Microsoft, and which would the customer need to geo-replicate?  Describe the process for geo-replication of each resource which would require manual operation for geo-replication.

    1. Machine Learning workspace
    2. Machine Learning compute
    3. Key Vault
    4. Container Registry
    5. Storage Account
    6. Application Insights
* What resources would you recommend the customer build backup policies to protect?
