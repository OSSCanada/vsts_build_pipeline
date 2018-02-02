# Build Out VSTS CD Pipeline

In this portion of the lab we will be creating the Continuous Deployment (CD) pipeline in VSTS.

## Exercise 1 - Create Release Pipeline

1. Create Empty Release Pipeline

* Hover over the **Build and Release** navigation item near the top of the VSTS window. You will see a list of drop-down options, click on **Releases**.
* Click on **+ New definition** to create a new Release.
* Enter **Kubernetes** in the search box.
* Hover over the **Deploy to Kubernetes cluster** item and click on **Apply**.

2. Add Dev Environment

* Change the **Environment name** to **Dev**. Notice the visualization to the left was updated.

3. Add Artifact to Release Process

* Click on the **Add artifact** link in the visualization on the left.
* In the **Add artifact** windows that pops-up ensure the following are selected:
    * Project:   akspipeline (Shoudl be the Default)
    * Source (Build Definition):   Select **akspipeline-CI** from the drop-down
    * Default Version:   Latest
* Click on **Add** button to add updates.

4. Add Release Tasks

* Click on the **Dev** Environment.
* You will see we have similar Process bar after clicking on the Environment. There is not much exciting any here except the ability to change the name of the Environment.
* At the top, hover over the **New Release Definition**, click on the pencil icon, and give the definiton a new name of **K8S**.
* Click on **Agent phase** to the left.
* Similar to what we did during the build phase, we need to change the type of Build Agent. Select **Hosted Linux Preview** from the **Agent queue** drop-down.
* Although we will not be changing anything, take note of the **Parallelism** options under the **Execution Plan** section farther down. If you hover over the information icon you will see more details.
* Click on the **kubectl apply** task and update the following:
    * Display name:   Apply to K8s Cluster
    * Kubernetes Service Connection:   Click on **+ New** to add connection to your cluster.
        * Hint: To get the contents of the kube config file, open it in VS Code and copy and paste the content.
        * Hint: The kube config file is located in your home directoy in a hidden folder called **/.kube/config**
    * Command:   apply
    * Use Configuration files:   Ensure this is Checked
    * Configuration File:   Use ... to select the heroes-db.yaml file in the yaml output folder
    * Expand Container Registry Details:   Point to your Azure Container Registry and use **vstssecret** as the **Secret name**. Ensure the **Force update secret** Checkbox is enabled.
* Click on **Save**, then **Ok**.
* Click on **Pipeline** in the navigation under **All definitions**.

5. Kick off New Release

* Click on the **+ Release** link in the upper right-hand corner and select **Create release**.
* Click the **Create** button to kick off the release process.
* You will see a new Release  with a number link near the upper left-hand corner, click on that release number.
* A new browsing tab will have opened and you can look at the progress of the release. If it is green then all is good. If it is **red** then click on the **Deployment Status** to see a summary and logs of the errors.
* Troubleshoot the release using the logs, fix the errors, re-run the release.

## Summary

At this point you should have a fully functioning CI/CD process that deploys the application to your Kubernetes cluster.