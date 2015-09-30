## Setting up the development environment

### Overview
The current development environment utilizes Vagrant running an Ubuntu image, which in turn launches Docker containers. Conceptually the Host launches a VM, which in turn launches Docker containers.

**Host -> VM -> Docker**

This model allows developers to leverage their favorite OS/editors and execute the system in a controlled environment that is consistent amongst the development team.

### Prerequisites
* [Git client](https://git-scm.com/downloads)
* [Go](https://golang.org/)
* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)

### Steps

#### Set your GOPATH
Make sure you have setup your Host's [GOPATH environment variable](https://github.com/golang/go/wiki/GOPATH) properly set. This allows for both building within the Host and the VM.

#### Note to Windows users
If you are running Windows, before running any `git clone` commands, run the following command.
```
git config --get core.autocrlf
```
If `core.autocrlf` is set to `true`, you must set it to `false` by running
```
git config --global core.autocrlf false
```
If you continue with `core.autocrlf` set to `true`, the `vagrant up` command will fail with the error `./setup.sh: /bin/bash^M: bad interpreter: No such file or directory`

#### Cloning the Openchain Peer project

Create a fork of the github.com/openblockchain/obc-peer repository using the GitHub web interface. Next, clone your fork in the appropriate location.

```
cd $GOPATH/src
mkdir -p github.com/openblockchain
cd github.com/openblockchain
git clone https://github.com/<username>/obc-peer.git
```


#### Cloning the Openchain Development Environment project
Choose another location (**NOT** within the GOPATH directory tree) which will be referred to as the WORKSPACE.  Change to this chosen WORKSPACE directory and clone the github.com/openblockchain/obc-dev-env repository.

    cd WORKSPACE
    git clone https://github.com/openblockchain/obc-dev-env.git


#### Boostrapping the VM using Vagrant    

Now change to the WORKSPACE/obc-dev-env directory and run the following command:

    vagrant up

Once complete, you should now be able to SSH into your new VM with following command from the same WORKSPACE/obc-dev-env directory.

    vagrant ssh

Once inside the VM, you can find your WORKSPACE directory under /openchain and the obc-peer project under $GOPATH/src/github.com/openblockchain/obc-peer. Additional [instructions](https://github.com/openblockchain/obc-peer/blob/master/README.md) describe how to build, run and test the Openchain Peer project.

**NOTE** any time you *git clone* any of the projects in your Host's WORKSPACE, the update will be instantly available within the VM's /openchain directory.
