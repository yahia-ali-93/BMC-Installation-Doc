# Common Services Installation
While installing the common services make sure of the following:
- Read the documentation carefully, as sections that seem similar (password requirements for ex) almost always contain differences between each other
- Don't create serviceaccounts or roles or role bindings .. just use the default value always
- Any file you want to edit according to the documentation has to be copied with it's filled in values to act as a backup.
    - This is essential in the case of `secrets.txt` as the installation deletes during each run
- Before running the installation, you have to use the `HCT` in the pre-install mode to make sure everything is ready for the installation

# Filling in the `infra.config` file
Filling the infra.config file, follow the instructions in the documentation but regard the following:
- Fill in the following properties as follows:

    ```js
        LB_HOST=helix-rsso.<domain>
        LB_PORT=443
        TMS_LB_HOST=helix-tms.<domain>
        MINIO_LB_HOST=helix-minio.<domain>
        MINIO_API_LB_HOST=helix-minio-api.<domain>
        KIBANA_LB_HOST=helix-kibana-api.<domain>
        TENANT_NAME=helix
        TENANT_EMAIL=valid email address (match the SMTP email if you are using live smtp)
        TENANT_FIRST_NAME=admin
        TENANT_LAST_NAME=admin
        TENANT_TYPE=itom
        TENANT_COUNTRY="Egypt"
    ```

- Anything in the `storage class`, `Optimize Storage`, `Nginx controller details`, `Security Context details` sections should be filled by the infra team
- Please refer to the SMTP file, in `General` if you want details on how to use a fake SMTP server to get the first tennant email
- Please refer to the certificate file in `General` if you want more details on how to generate the full chain certificate

# Running the installation
- The installation can get stuck sometimes on a restarting pod .. This happens and you should just run it again
- If the VPN is unreliable, a good idea is to run the installation from the DB windows VM so that you can install `mobaxterm` on it and run the installation steps from there .. this way if your vpn disconnects the session is not terminated
- Do Not Use `nohup` with this installation .. the process recieves `hangup` as soon as the terminal disconnects
