## Setting up the development environment

### Overview
The current developnment environment utilizes Vagrant running an ubuntu image, which in turn launches docker containers.
Conceptually the Host launches a VM, which in turn launches docker containers.

**Host -> VM -> docker**

This model is utilized to allow for developers to leverage their favorite OS/Editors and execute the system in a controlled environment that is consistent amongst the development team.

### Prerequisites
* Git client
* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)

### Steps

#### Cloning Golang projects
First make sure you have setup your Host's GOPATH environment variable properly.  This allows for both building within the Host and the VM.

You will then need to make sure the following directory exists within your Host's GOPATH.  If it does not, create it:

>$GOPATH/src/github.com

From your Host's **$GOPATH/src/github.com** directory, perform the following commoand:

    git clone https://github.com/apacheopenchain/obc-peer

#### Cloning the development environment project
Choose another location (**NOT** within the GOPATH directory tree) which will be referred to as the WORKSPACE.  Change to this chosen WORKSPACE directory and clone the following project from within that directory. (**NOTE:** you may need to request access to the repositories)

    git clone https://github.com/apacheopenchain/obc-dev-env


####Boostrapping the VM using Vagrant    

Now change to the WORKSPACE/obc-dev-env directory and perform the following command:

    vagrant up

Once complete, you should now be able to SSH into your new VM with following command from the same WORKSPACE/obc-dev-env directory.

    vagrant ssh

Once inside the VM, simply change to the /openchain directory.  Here you will see all of the projects you cloned into your Host's WORKSPACE directory.

**NOTE** any time you *git clone* any of the projects in your Host's WORKSPACE, the update will be instantly available within the VM's /openchain directory.
