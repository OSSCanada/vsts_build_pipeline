# Build Out VSTS CD Pipeline

In this portion of the lab we will be creating the Continuous Deployment (CD) pipeline in VSTS.

## Exercise 1 - Create Release Pipeline

1. Create Empty Release Pipeline

* Hover over the **Build and Release** navigation item near the top of the VSTS window. You will see a list of drop-down options, click on **Releases**.
* Click on **+ New definition** to create a new build.

6. Add Kubernetes Steps

* Next, click on the **Phase 1** link on the left-hand side. You will notice that this Build Agent Phase automatically inherits the type of Build Agent from the Parent Build Process.
* You can change the **Display name** or leave it as is, up to you.
* The next step is to add Docker Build and Docker Push steps to the pipeline. This is done by clicking on the **+** next to **Phase 1**, entering **docker** in the search box, hover over the **Docker** taks, then hit the **Add** button twice.
