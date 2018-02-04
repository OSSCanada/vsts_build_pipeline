# Build Out VSTS CD Pipeline

In this portion of the lab we will be creating the Continuous Deployment (CD) pipeline in VSTS.

## Exercise 1 - Create Release Pipeline

1. ***Create Empty Release Pipeline***

* Hover over the **Build and Release** navigation item near the top of the VSTS window. You will see a list of drop-down options, click on **Releases**.
* Click on **New definition** to create a new Release.
* Enter **Kubernetes** in the search box.
* Hover over the **Deploy to Kubernetes cluster** item and click on **Apply** twice to add two Kubernetes tasks.
* Enter **K8s** in the search box.
* Hover over the **k8s downloader** item and click on **Add** once to add a kubectl downloader task.
* Enter **bash** in the search box.
* Hover over the **Shell Script** item and click on **Add** once to add a shell script execution task.
* You should now have 4 tasks under the **Agent phase**.

2. ***Add Dev Environment***

* Change the **Environment name** to **Dev**. Notice the visualization to the left was updated.

3. Add Artifact to Release Process

* Click on the **Add artifact** link in the visualization on the left.
* If you recall from the build setup, the reason for this step is to pull in the yaml artifacts we added to the build publishing output.
* In the **Add artifact** windows that pops-up ensure the following are selected:
    * Project:   akspipeline (Shoudl be the Default)
    * Source (Build Definition):   Select **akspipeline-CI** from the drop-down
    * Default Version:   Latest
* Click on **Add** button to add updates.

4. Configure Release Tasks

* Click on the **Dev** Environment.
* You will see we have similar Process bar after clicking on the Environment. There is not much exciting here except the ability to change the name of the Environment.
* At the top, hover over the **New Release Definition**, click on the pencil icon, and give the definiton a new name of **K8S**.
* Click on **Agent phase**. Similar to what we did during the build phase, we need to change the type of Build Agent. Select **Hosted Linux Preview** from the **Agent queue** drop-down.
* Although we will not be changing anything else, take note of the **Parallelism** options under the **Execution Plan** section farther down. If you hover over the information icon you will see more details.

* Click on the first **kubectl apply** task and update the following:
    * Display name:   Apply DB Image to K8s Cluster
    * Kubernetes Service Connection:   Click on **+ New** to add connection to your cluster.
        * Hint: To get the contents of the kube config file, open it in VS Code and copy and paste the content.
        * Hint: The kube config file is located in your home directoy in a hidden folder called **/.kube/config**
    * Command:   apply
    * Use Configuration files:   Ensure this is Checked
    * Configuration File:   Use ... to select the **heroes-db.yaml** file in the yaml output folder
    * Expand Container Registry Details:   Point to your Azure Container Registry and use **vstssecret** as the **Secret name**. Ensure the **Force update secret** Checkbox is enabled.
* Click on **Save**, then **Ok**.

* Click on the second **kubectl apply** task and update the following:
    * Display name:   Update K8s DB Image
    * Kubernetes Service Connection:   Select existing **AKS** endpoint.
    * Command:   apply
    * Use Configuration files:   Ensure this is NOT Checked
    * Arguments:   image deployment/heroes-db-deploy heroes-db-cntnr=<ACR_NAME>.azurecr.io/azureworkshop/rating-db:$(Build.BuildId)
    * Expand Container Registry Details:   Point to your Azure Container Registry and use **vstssecret** as the **Secret name**. Ensure the **Force update secret** Checkbox is enabled.

* Click on the third **k8s downloader** task and update the following:
    * Display name:   kubectl downloader
    * Kubernetes Service Connection:   Select existing **AKS** endpoint.
    * Don't change anything else.

* Click on the fourth **Bash** task and update the following:
    * Display name:   Load DB
    * Script Path:   Use ... and select **databaseload.sh** file from the build output.
    * Arguments:   (Leave Blank)

* Click on **Pipeline** in the navigation under **All definitions**.

5. Kick off New Release

* Click on the **+ Release** link in the upper right-hand corner and select **Create release**.
* Click the **Create** button to kick off the release process.
* You will see a new Release  with a number link near the upper left-hand corner, click on that release number.
* A new browsing tab will have opened and you can look at the progress of the release. If it is green then all is good. If it is **red** then click on the **Deployment Status** to see a summary and logs of the errors.
* Troubleshoot the release using the **Logs** navigation itme, fix the errors, kick off a new release.

6. Add Web and API Deployments

* Leverage content in the previous steps 1 and 4 to add tasks for the Web and API parts of the application.
* Hint: You will need 3 more Kubernetes Tasks. The first to apply the web and api containers use the yaml file. The second and third tasks to update the web and api images on the cluster.
    * First Task Name:   Apply Web and API Images to K8s Cluster
    * Second Task Name:   Update K8s API Image
    * Third Task Name:   Update K8s WEB Image

7. Kick off Another Release to Test Changes

* **The only difference in this one versus the one above is that all of the Containers are now being deployed.**
* Click on the **+ Release** link in the upper right-hand corner and select **Create release**.
* Click the **Create** button to kick off the release process.
* You will see a new Release  with a number link near the upper left-hand corner, click on that release number.
* A new browsing tab will have opened and you can look at the progress of the release. If it is green then all is good. If it is **red** then click on the **Deployment Status** to see a summary and logs of the errors.
* Troubleshoot the release using the **Logs** navigation itme, fix the errors, kick off a new release.

8. Trigger Continuous Deployment

* Now that we have all of the kinks worked out in our release, let's have it automatically trigger upon a successful build.
* Hover over the **Build and Release** menu navigation at the top and select **Releases**.
* On the left-hand side select your release definiton, it should be **K8S** if all the directions were followed correctly.
* Once the release is selected, you should see an **Edit** button near the top, click it.
* You should now see the pipeline diagram of your release. In the upper right-hand corner of the **akspipeline-CI** build artifact you will see a lightning bolt, click it.
* In the windows that pops up, enable the trigger.
* Now you have Continuous Deployment along with Continuous Integration now setup.

## Summary

At this point you should have a fully functioning CI/CD process that deploys the application to your Kubernetes cluster.