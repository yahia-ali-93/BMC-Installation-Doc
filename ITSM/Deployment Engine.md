# Installation Instructions

***note:*** Please follow the documentation and only consider these notes below as notes on the installation.

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
    - in the **ITSM_REPO_GIT_ZIP** property, the file name is written incorrectly
    - don't forget to update the **FQDN**

### Certificate Considerations during pipeline execution

The required certificate to upload in the pipeline is not just the pem file, no it's a keystore file you'll find in the documentation in the following link:
- https://docs.bmc.com/docs/brid22106/preparing-to-use-self-signed-or-custom-ca-certificates-1218539860.html

So in order to use the certificate, do the following:
- Download the keystore, **cacert** file, from the documentation
- Open it in a tool like **keystore Explorer**
- Add the certificate to this keystore
- check the links inside the certificate you just added by clicking on it from the list and then
    - click **extensions**
    - click **Subject Alternate Name**
    - Check the links to make sure they include all links you need for this certificate
- Save the keystore and use it as needed

### Pipeline parameters considerations

- `HELIX_PLATFORM_CUSTOMER_NAME`: The company name inside `infra.config`
- `PLATFORM_ADMIN_PLATFORM_EXTERNAL_IPS`: The external IP of a worker node
- `IMAGESECRET_NAME`: Just use the name in the example
- `REGISTRY_TYPE`: Always select **DTR** even if you're using a local harbor repository
- `PRODUCT-DEPLOY`: This section contains all the pipelines that will run. If you're doing a fresh installation and one step has succeeded before then uncheck it when you run the pipeline again
    - This is just to save time but no harm will come from leaving all as checked everytime
- `LOGS_ELASTICSEARCH_HOSTNAME`: Use the example in the documentation
- `FTS_ELASTICSEARCH_HOSTNAME`: Use the example in the documentation