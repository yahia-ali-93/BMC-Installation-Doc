# Installation

- No special needs for the java version, but the tried version was **java jdk 17**

# General
- All applications will ask for an IP and port with the login credentials for **hannah_admin**
    - Run `kubectl get svc -n <BMC ITSM NAMESPACE>`
    - Look for the **platform-admin-ext** service
    - the IP is the **EXTERNAL-IP**
    - the port is **46262** which is the first port in the **PORT(S)** column

## Atrium Spoon

- You have to add two system environment variables:
    - `PENTAHO_JAVA_HOME`: The path to the java folder in program files (EX: `C:\Program Files\Java\jdk-17`)
    - `PENTAHO_DI_JAVA_OPTIONS`: `-Xms1024m`
- You might need to run it as administrator too, **if the application logo appears then the app crashes**
