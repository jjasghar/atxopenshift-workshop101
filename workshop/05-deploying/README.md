# Lab 3 - Deploying a Project to Red Hat OpenShift Kubernetes Cluster

This is a description of the Lab 3

Lab 3 consits of two parts:

- Setting up the environment for Lab 3
- Creating a Red Hat OpenShift project

Below please find the architecture of the project.

![](../.gitbook/assets/-Lti9WcAPYU97e8yFB7v-image.png)

## Lab 3 - Setting up

During this Lab you will use recently created project and deploy it to your cluster.

### Tools

Find the option for the tools most suitable for you

#### Prebuilding an Image with local Code

There is an image on DockerHub with all required tools. In order to use local IDEs and editors to modify code and configuraton files a Docker volume is used. This option works only for Mac and Linux.

#### Step 1: Run these commands in a terminal

```bash
$ git clone https://github.com/IBM/openshift-on-ibm-cloud-workshops.git
$ cd openshift-on-ibm-cloud-workshops
$ ROOT_FOLDER=$(pwd)
$ docker run -v $ROOT_FOLDER/:/cloud-native-starter -it --rm nheidloff/openshift-workshop-tools:v1
```

#### Step 2: Inside your running Docker image you can access your the local project

You should see the prompt like this `root@3f46c41f7303:/usr/local/bin#`, now run the following instructions:

```bash
$ cd /cloud-native-starter/
$ ls
$ ROOT_FOLDER=$(pwd)
```

Note: With the `--rm` option in the docker run command the container is deleted once you exit. This is intended.

#### Step 3: Move on with Verify Access to OpenShift on the IBM Cloud

##### Option 1

(prefered for Windows): Prebuilt Image with Code in Container There is an image on DockerHub with all required tools. This option works for Mac, Linux and Windows. To get started as quickly as possible, use this image.

Step 1: Run this command in a terminal

```bash
$ docker run -ti nheidloff/openshift-workshop-tools:v1
```
Step 2: After the container has been started, run these commands inside your running Docker image to get the lastest version of the workshop: You should see the prompt like this

```bash
root@3f46c41f7303:/usr/local/bin#
```
now run the following instructions:

```bash
$ cd
$ git clone https://github.com/IBM/openshift-on-ibm-cloud-workshops.git$ cd openshift-on-ibm-cloud-workshops
$ ROOT_FOLDER=$(pwd)
```
*Note:* If you using Windows you also need to download or clone the project to your local workstation for the upcoming Docker and Java lab, because you can't use Docker in the 'openshift-workshop-tools' Docker image.

Step 3: Move on with Verify Access to OpenShift on the IBM Cloud

#### Option 2

Install Tools locally on your desktop computer This approach works only for Mac and Linux.

Step 1: Install the following tools: oc kubectl git curl Optional: IBM Cloud CLI Optional: Editor, for example Visual Studio Code

Step 2: Get the code:

```bash
$ git clone https://github.com/IBM/openshift-on-ibm-cloud-workshops.git
$ cd openshift-on-ibm-cloud-workshops
$ ROOT_FOLDER=$(pwd)
```

Step 3: Move on with Verify Access to OpenShift on the IBM Cloud

## Verify Access to OpenShift on the IBM Cloud

###  Step 1: After you've created a new cluster, open the OpenShift console.

- Logon to the IBM Cloud web console - and choose the IBM organization
- Select OpenShift in the menu
- Chose Clusters and click on your OpenShift cluster
- Open the OpenShift web console

![From a left menu icon (🍔) choose OpenShift](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-Lti3mCtmtRafh0nR9de_-Lti7niQUUzz0zFGXCR7_image.png)

![Choose your pre-provisioned cluster](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-Lti3mCtmtRafh0nR9de_-Lti82UvtvkHGBO1S8hX_image.png)

![Open OpenShift Web Console](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-Lti3mCtmtRafh0nR9de_-Lti8EKcrm5FB-OaYbdN_image.png)

###  Step 2: Get our access token for the 'oc' CLI.

- From the dropdown menu in the upper right of the page, click 'Copy Login Command'. Paste the copied command into your terminal.

![Copying Login Command](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-Lti3mCtmtRafh0nR9de_-Lti8SzU-pNv1DiipAjZ_image.png)

- Verify 'oc' CLI

```bash
$ oc login https://c1-e.us-east.containers.cloud.ibm.com:23967 --token=xxxxxx'
$ oc get istag
```
- Verify 'kubectl' CLI

```bash
$ kubectl get pods
```

Congratulations! You are ready to start the Lab 3.
