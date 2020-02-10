# Next Steps

In this section you will discover where you can find materials to learn more about Red Hat OpenShift, and use free of charge environment on your local machine with Minishift labs.

## The 4 workshops

* ​https://github.com/IBM/minishift101 - basic workshop - shown below
* https://github.com/IBM/node-red-workshop-starter - adding Node-Red
* https://github.com/IBM/machine-learning-with-minishift - adding machine learning
* https://github.com/IBM/openshift-on-ibm-cloud-workshops - the full lab from today

## Minishift setup and Minishift 101 lab

The following section describes how to install Minishift and the required dependencies.

These are the basic steps for setting up Minishift on your personal system:

- ​Configure the virtualization environment
- ​Download and install Minishift
- ​Start the OpenShift Server

The setup procedure should be run as a regular user with permission to launch virtual machines. In the procedure, you will see how to assign that permission, along with ways to configure your hypervisor and command shell to start and effectively interact with Minishift.

## Configure the virtualization environment

Minishift can be installed on Windows, Linux and Mac but depending on your platform, you will need to configure a compatible hypervisor in order for the OpenShift cluster to be created within the virtualised environment. Verify that the hypervisor of your choice is installed and enabled on your system before you set up Minishift. Once the hypervisor is up and running, additional setup is required for Minishift to work with that hypervisor.

See the appropriate section for your hypervisor and operating system:
*F or Linux, [set up the KVM driver](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html#setting-up-kvm-driver]
* For macOS, [set up the hyperkit driver](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html#setting-up-hyperkit-driver)
* For Windows, [set up the Hyper-V driver](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html#setting-up-hyperkit-driver)
* For VirtualBox (all platforms), [set up Minishift to use VirtualBox](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html#setting-up-virtualbox-driver)

*NOTE:* If you have installed docker-ce for Mac on your machine, the hyperkit driver is already installed
## Download and install Minishift

To [download Minishift](https://docs.okd.io/latest/minishift/getting-started/installing.html), you can either install from source or if on Mac, you can install using Homebrew.
## Start the OpenShift Server

Providing you have completed the previous steps successfully, you will now be ready to start the OpenShift server. To do so, you should be able to run:
```bash
$ minishift start --vm-driver <driver>
```
Make sure to replace `<driver>` with the driver that you have used e.g. 'hyperkit' or 'virtualbox'. Once successfully started, you should be given the credentials to login to the cluster and the UI address as shown below:

```bash
OpenShift server started.

The server is accessible via web console at:
    https://192.168.64.11:8443/console

You are logged in as:
    User:     developer
    Password: <any value>

To login as administrator:
    oc login -u system:admin


-- Exporting of OpenShift images is occuring in background process with pid 5703.
```

*NOTE: Both your console address and your pid will be different.*

## Further cluster configurations

After setting up your cluster, you may have specific requirements that you want to enforce in your cluster. The minishift tool allows you to manage the lifecycle of the single-node OpenShift cluster as well as set environment variables, persistent storage and proxy options if your machine is behind a proxy. For more information on these configurations, see the following link.

Once you are all set up, you can move on to Lab 1
