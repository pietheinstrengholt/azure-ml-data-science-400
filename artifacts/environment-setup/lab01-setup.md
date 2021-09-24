# Setup Lab 01

**Contents**

<!-- TOC -->

- [Lab Setup](#lab-setup)
   - [Task 1: Create a Compute Instance](#task-1-create-a-compute-instance)
   - [Task 2: Import the Lab Notebooks](#task-2-import-the-lab-notebooks)
- [Additional Lab Requirements](#additional-lab-requirements)

## Lab Setup

### Task 1: Create a Compute Instance

In this task, you add a compute resource to your Azure Machine Learning workspace.

1. On the Machine Learning blade in the [Azure portal](https://portal.azure.com/), open Azure Machine Learning studio by selecting **Launch studio** from the center section of the screen.

   ![The Launch studio button is highlighted on the Machine Learning blade.](media/machine-learning-launch-studio.png "Launch Azure Machine Learning studio")

2. In the new Azure Machine Learning studio window, select **Create new** and then select **Compute instance** from the context menu.

   ![Within Azure Machine Learning studio, Create new is selected and highlighted, and Compute instance is highlighted in the context menu.](media/machine-learning-studio-create-new-compute-instance.png "Create new compute instance")

3. On the create compute instance screen, enter the following information and then select **Create**:

  - - **Compute name**: Enter `ml-bootcamp-SUFFIX`, where SUFFIX is your Microsoft alias, initials, or other value to ensure uniquely named resources.
   - **Virtual machine type**: Select `CPU`.
   - **Virtual machine size**: Select `Select from recommended options` and then select `Standard_DS3_v2`.

   ![On the create compute instance dialog, CPU is selected for the virtual machine type. Select from recommended options is selected under virtual machine size, and Standard_DS3_v2 is selected and highlighted in the recommended virtual machine sizes.](media/machine-learning-studio-create-compute-instance-select-virtual-machine.png "Create Compute Instance")

4. Wait for the Compute Instance to be ready. It takes approximately 3-5 minutes for the compute provisioning to complete.

### Task 2: Import the Lab Notebooks

In this task, you import Jupyter notebooks from GitHub that you will use to complete the exercises in this hands-on lab.

1. From within Azure Machine Learning studio navigate to **Compute, Compute instances**, and then select **Jupyter** link to open Jupyter Notebooks interface for the compute instance **ml-bootcamp-SUFFIX**.

   ![The Jupyter link is highlighted next to the ml-bootcamp-SUFFIX compute instance.](media/ml-workspace-compute-instances.png "Compute instances")

2. Check **Yes, I understand** and select **Continue** in the trusted code dialog.

   ![In the Always use trusted code dialog, Yes, I understand is checked, and the continue button is highlighted.](media/trusted-code-dialog.png "Always use trusted code")

3. In the new Jupyter window, select **New** and then select **Terminal** from the context menu.

   ![In the Jupyter notebooks interface, the New dropdown is selected, and Terminal is highlighted in the context menu.](media/jupyter-new-terminal.png "Open new terminal window")

   > **Note**: If you see errors when loading terminal, you will have to first run `cd ~/cloudfiles/code/Users/<username>` before proceeding.
  
4. Run the following commands in order in the terminal window:

   - `mkdir ml-labs`
   - `cd ml-labs`
   - `git clone https://github.com/solliancenet/azure-ml-data-science-400.git`

   > **Note**: If you have two-factor authentication set up on your GitHub account, you have to use a PAT as the password.

   ![In the Jupyter terminal window, the commands listed above are displayed.](media/jupyter-terminal.png "Import repository")

5. Wait for the `clone` command to finish importing the repo.

## Additional Lab Requirements

The lab will create the following resources on the fly and thus we need to ensure necessary quota for the lab user.

- AML compute cluster vm_size='Standard_DS12_v2', max_nodes=2
- ACI container cpu_cores=3, memory_gb=15
- AKS cluster Standard_D3_v2 (default is 3 nodes)
