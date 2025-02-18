# The following is the methodology to install and configure a tennant using the TCTL utility

**Note:** read this file first, then proceed with the documentation and fill in any steps not mentioned here with the equivalent from the BMC Documentation

You have to run the utility from a windows maching ... but most probably you won't be able to connect to the OC cluster from within the windows machine.
The solution is to run all commands that you need to deduce the values of the parameters in the config file inside a linux vm that can login to the cluster and then copy the resulting file to the windows machine where you'll connect to TCTL.

## Populating the config file

- before you begin `login to the cluster`.
- to get the `<tms pod name>` variable run `oc get pods` and look for a pod that starts with **tms**
- any mention of `<namespace>` refers to the **platform namespace**

## Running the tool

- Just put the **config file** and **tctl.exe** files inside a folder named **tctl** inside the home directory of the user you're logged in with
- Then open a **CMD** inside that folder to be able to use the tctl command line tool

## Important Links

- **TCTL Tool installation**: https://docs.bmc.com/docs/itomdeploy244/downloading-and-configuring-the-tctl-utility-1414590724.html
- **TCTL Usage for fixing SMTP Issues with the common services installation**: https://community.bmc.com/s/article/BMC-Helix-IT-Operations-Management-ITOM-Deployment-OnPrem-How-to-resend-a-Tenant-Activation-email-using-the-TCTL-utility-INCLUDES-VIDEO
