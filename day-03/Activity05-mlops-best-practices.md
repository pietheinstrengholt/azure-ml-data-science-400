# Activity 05 - MLOps Best Practices

DevOps has become ubiquitous in the world of classical development. Almost all projects that exceed a certain level of complexity become inevitably DevOps projects. Yet, there is one category of projects that are stepping out of the line. You’ve guessed it right, it’s the category of Data Science projects.

When it comes to DevOps, Data Science projects pose a range of unique challenges, whether it’s about the technical side of things, the philosophy of the people involved, or the actors involved. Think about a straightforward example: versioning. While in a “classical” development project, versioning refers almost exclusively to source code, in the world of data science, it gets another vital aspect: data versioning. It’s not enough to know the version of the code for your model, it’s equally important to know the “version” of the data it was trained on. Another interesting question is, for example, what does a “build” mean in the world of data science? Or a “release”?

MLOps empowers data scientists and machine learning engineers to bring together their knowledge and skills to simplify the process of going from model development to release/deployment. ML Ops enables you to track, version, test, certify and reuse assets in every part of the machine learning lifecycle and provides orchestration services to streamline managing this lifecycle. This allows practitioners to automate the end-to-end machine learning lifecycle to frequently update models, test new models, and continuously roll out new ML models alongside your other applications and services.

In this activity we will aim to discuss together some of the most important MLOps best practices that help addressing the main challenges faced when Data Science projects move into operations.

## 1. MLOps and traditional DevOps application CI/CD pipelines

When it comes to DevOps principles, any non-trivial project that includes a data science component will need to take advantage of a combination of features from the following two major platforms:

- [Azure Machine Learning service](https://azure.microsoft.com/services/machine-learning-service/)
- [Azure DevOps](https://azure.microsoft.com/services/devops)

The official definition of **Azure Machine Learning service** is:

> *Azure Machine Learning service provides a cloud-based environment you can use to prep data, train, test, deploy, manage, and track machine learning models. Start training on your local machine and then scale out to the cloud. The service fully supports open-source technologies such as PyTorch, TensorFlow, and scikit-learn and can be used for any kind of machine learning, from classical ml to deep learning, supervised, and unsupervised learning.*

The official definition of **Azure DevOps** is:

> *Azure DevOps provides developer services to support teams to plan work, collaborate on code development, and build and deploy applications. Developers can work in the cloud using Azure DevOps Services or on-premises using Azure DevOps Server, formerly named Visual Studio Team Foundation Server (TFS).*

The DevOps mindset applied to data science solutions is commonly referred to as Machine Learning Operations (MLOps). The most essential aspects of MLOps are:

- Deploying Machine Learning projects from anywhere
- Monitoring ML solutions for both generic and ML-specific operational issues
- Capturing all data that is necessary for full traceability in the ML lifecycle
- Automating the end-to-end ML lifecycle using a combination of Azure Machine Learning service and Azure DevOps

**Team Challenge 1**

Analyze the complementarity of the two platforms and how they support the following core MLOps concepts:

- Boards
- Code repos
- Artifacts management
- Pipelines
- Data versioning
- Model training, deployment, and monitoring

## 2. MLOps with Azure DevOps Reference Architecture

The most important components involved in such an architecture are:

[Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines). This build and test system is based on Azure DevOps and used for the build and release pipelines. Azure Pipelines breaks these pipelines into logical steps called tasks. For example, the Azure CLI task makes it easier to work with Azure resources.

[Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-machine-learning) is a cloud service for training, scoring, deploying, and managing machine learning models at scale. This architecture uses the Azure Machine Learning Python SDK to create a workspace, compute resources, the machine learning pipeline, and the scoring image. An Azure Machine Learning workspace provides the space in which to experiment, train, and deploy machine learning models.

[Azure Machine Learning Compute](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-set-up-training-targets) is a cluster of virtual machines on-demand with automatic scaling and GPU and CPU node options. The training job is executed on this cluster.

[Azure Machine Learning pipelines](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-ml-pipelines) provide reusable machine learning workflows that can be reused across scenarios. Training, model evaluation, model registration, and image creation occur in distinct steps within these pipelines for this use case. The pipeline is published or updated at the end of the build phase and gets triggered on new data arrival.

[Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview). Blob containers are used to store the logs from the scoring service. In this case, both the input data and the model prediction are collected. After some transformation, these logs can be used for model retraining.

[Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro). The scoring Python script is packaged as a Docker image and versioned in the registry.

[Azure Container Instances](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-overview[]). As part of the release pipeline, the QA and staging environment is mimicked by deploying the scoring webservice image to Container Instances, which provides an easy, serverless way to run a container.

[Azure Kubernetes Service](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes). Once the scoring webservice image is thoroughly tested in the QA environment, it is deployed to the production environment on a managed Kubernetes cluster.

[Azure Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview). This monitoring service is used to detect performance anomalies.

**Team Challenge 2**

Define and analyze an MLOps architecture involving as many of the above mentioned components as possible.

## 3. MLOps with GitHub Actions

GitHub Actions represents an entirely new way to automate your development workflow, consisting of a collection of open-source actions that facilitate ML Ops. This discussion focuses on the GitHub Actions relevant to machine learning and data science that you can use to automate tasks.

**Team challenge 3**

Analyze the the most important AML-related GitHub actions:

- aml-workspace
- aml-compute
- aml-run
- aml-registermodel
- aml-deploy