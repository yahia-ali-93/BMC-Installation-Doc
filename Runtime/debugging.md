# Debugging
while using the app you might come against issues. I'll list them as **case** and **solution** below.

## Case: Email server not working
- Access the pod `platform-fts-0` or `platform-fts-1` and check the **AREmail** logs
    - You'll find the logs in the following path `/opt/bmc/ARSystem/AREmail/Logs`
    - To open an ssh terminal inside a pod run the following command `oc exec -it <podname> -n <namespace> -- bash`
    - If you find errors related to the certificate it might be due to the custom certificate you're using not having the exchange certificate as part of it
    - If you're not bothered with fixing this issue properly you can ignore the certificate by doing the following but be warned that if the platform pods get deleted **you'll have to repeat the steps again for it to work**
    - To ignore certificates go to `/opt/bmc/ARSystem/AREmail` and open file `emaild.sh`.
        - inside the file look for the `JAVA_OPTS` command and add to it the following flag `-Dmail.smtp.ssl.trust=*`
        - If you're using TLS you might need to add the following flags `-Dmail.smtp.starttls.enable=true -Dmail.smtp.starttls.required=true`
        - Run `emaild.sh stop &` which should restart the mail engine
- Access the other platform pod `platform-fts-1` or `platform-fts-0` and repeat all steps you did for the other pod above

## Case: After a fresh install you try to access midtier but is says **err hannah_admin code 0**
- This is a certificate error. Which means you either didn't follow the instructions in the certificate section in the **Deployment Engine** file, or the certificate you have is invalid.
    - Note that the documentation says that you need the public key to add to the **cacerts** file but if you can't get it then you can use the **.pem** full chain certificate too, you just can't use **keystore explorer** to do this as it doesn't allow it so you have to follow the documentation command line instructions 
