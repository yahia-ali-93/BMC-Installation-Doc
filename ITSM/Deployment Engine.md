# Installation Instructions

***note:*** Please follow along the bmc documentation for version **22.6.01** as some steps are explained more in the documentation.

## Hardware Requirements

just follow the requirements in the documentation

## Software Requirements

Use the latest of the certified alternatives .. so either ***RHEL 8*** or ***CentOS 8***.

***Important:*** DO NOT USE ANY OTHER OS TYPE EVEN IF IT IS SUPPORTED .. DO NOT USE FEDORA FOR PRODUCTION, AS THIS IS NOT RECOMMENDED BY BMC

## Pre-install commands

Commands to update and prepare the machine for deployment

- Login using the **root** account
-       yum update -y
        yum upgrade -y
        yum clean all

## Installing 3rd party libraries

These are helper libraries required for the installation

- Unzip utility
  -       sudo yum install unzip
- Perl
  -       sudo yum install perl
- perl-Data-Dumper
  -       sudo yum makecache
            sudo yum -y install perl-Data-Dumper

## Check VM availability from another machine in the cluster

You have to check that the machine is visible from other machines in the same cluster

```shell
nslookup <FQDN>
nslookup <server-ip>
```

## Download the required files

Download the two files mentioned in the documentation to the VM

- The **BMC_Helix_Innovation_Suite_And_Service_Management_Apps_Version_22.1.06.zip** which is the deployment engine
- One of the patches for the common services depending on the installed version of the common service
- Put both files in the root directory of the machine

## Creating users

Run the following commands in sequence. Note the commented line **#** above each command explains the command below so ***don't run the commented line***

```shell
# add the git user
sudo useradd git -m
# set password for the get user ... don't use @ or # characters in here .. use _ as it is safe to use
sudo passwd git
# add the jenkins user
sudo useradd jenkins -m
# set password for the jenkins user .. don't use @ or # characters in here .. use _ as it is safe to use
sudo passwd jenkins
# Add the jenkins user to the git group and vise versa
sudo usermod -a -G git jenkins
sudo usermod -a -G jenkins git
# add sudo access to the git user
sudo usermod -aG wheel git
```

## provide passwordless access to the git user

follow the documentation in this section to provide the passwordless sudo access to the git user

## Copy files to the intended directory

Copy both of the downloaded files to the home directory of the **git** user by running the following two commands

```shell
cp <path-to-first-file> /home/git
cp <path-to-second-file> /home/git
```

## Switch to the git user and perform the pre-install configurations

perform the following steps in order

1. switch to the git user

    ```shell
    su - git
    ```

2. unzip the **BMC_Helix_Innovation_Suite_And_Service_Management_Apps_Version_2.1.06.zip**

    ```shell
    unzip BMC_Helix_Innovation_Suite_And_Service_Management_Apps_Version_22.1.06.zip
    ```

3. change the owner of each file from the extracted zip files

    ```shell
    sudo chown -R git:git BMC_Remedy_Deployment_Manager_Configuration_Release_22.1.06.zip
    sudo chown -R git:git BMC_Remedy_Deployment_Engine_Setup_22.1.06.zip
    ```

4. unzip the **BMC_Remedy_Deployment_Engine_Setup_22.1.06** file

    ```shell
    unzip BMC_Remedy_Deployment_Engine_Setup_22.1.06.zip
    ```

5. update the **FQDN** of the machine

    ```shell
    hostnamectl set-hostname <new-host-name>
    ```

5. change directory to the **DE1.0** folder and update the build.properties file

    ```shell
    cd DE1.0
    vi build.properties
    ```

    - follow all instrutions in the documentation in this section with the following comments
    - all paths that you need to update that start with **../** update it with an absolute path like so **/home/git/**
    - in the **ITSM_REPO_GIT_ZIP** property, the file name is written incorrectly .. it should end with **22.1.06.zip** instead of whatever is written there
    - don't forget to update the **FQDN**

## Check and install prequisets

Some prequisets are not installed by default by BMC and are not mentioned in any of the steps so check the following

- **Ansible**
  - Ansible is not installed by default and there is no mention of any steps to install it in the documentation
  - run `yum install ansible` to install it but check that the version to be installed is **2.9.xx**.
    - If this is not true, and you are on ***RHEL 8*** then do the following.

            ```shell
            # Add the ansible 2.9 repo to RHEL 8
            sudo subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms

            # install ansible 2.9.xx from yum
            sudo yum install ansible-2.9.27
            ```

    - If you're not on **RHEL 8** and want to install ansible 2.9 you can install **pip** and install ansible 2.9 from there but keep in mind
      - This won't work for newer versions of python
      - This won't work on python2
      - you can't downgrade the installed version of python on an OS as this can break it
      - This method of installing ansible is not tried and tested by me so i can't be sure it will work, use it as a last option

- **groovy**
  - First thing to do is try to run `yum install groovy` .. you might be lucky and the package is available from yum
    - don't forget to try the above command from the **root** user
  - If it's not available you can install it using **sdkman**. run the following commands **from the git user**

        ```shell
        # Fetch and install sdkman
        curl -s "https://get.sdkman.io" | bash

        # Add sdkman to the current terminal session
        source "$HOME/.sdkman/bin/sdkman-init.sh"

        # check if its working
        sdk version

        # install groovy
        sdk install groovy
        ```

## Script editing

The installation script has four lines that should be commented **before running the script**. click [https://community.bmc.com/s/article/BMC-Helix-ITSM-OnPrem-setup-Helix-ITSM-onPrem-pl-fails-with-error-sed-can-t-read-etc-sysconfig-jenkins-No-such-file-or-directory](here) and follow the instructions in the solutions section

## Running the script and post-running configurations

1. Run the script to install the deployment engine

    ```shell
    sudo perl setup-Helix-ITSM-onPrem.pl  2>&1 | tee ~/BMC-HELIX-DE-AUTO.log.$$
    ```

2. Remove sudo access from the git user

    ```shell
    gpasswd --delete git wheel
    ```

3. Update the kubectl version by removing the binary at `/usr/local/bin` and adding the cluster version in it's place
