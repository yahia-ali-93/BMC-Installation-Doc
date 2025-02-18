# SMTP Notes
- The SMTP Password cannot include any special characters
    - Must have a minimum of 7 characters.
    - Must contain at least one uppercase letter and one lowercase letter.
    - Must contain at least one digit.
    - Must not contain any special character. 
    - Must not contain `'admin'` and `'bmcuser'`.
- To change the SMTP credentials used in your common service installation do the following (save any file you edit after finishing and no action is required after that)
    - Update the SMTP ConfigMap:
        `kubectl edit cm smtp-credentials -n <mamespace>`
        - Update the required SMTP details; for example, SMTP_HOST or SMTP_PORT:
      
            ```yml
                apiVersion: v1
                data:
                SMTP_AUTH: "true"
                SMTP_HOST: mailhost.bmc.com
                SMTP_PORT: "25"
                kind: ConfigMap
            ```
      
    - Update the SMTP Secrets:
        - To edit the SMTP secrets file, run the following command:
        - `kubectl edit secret smtp-credentials -n <namespace>`
        - The SMTP secrets file is displayed, as shown in the following code block:
        
            ```yml
                apiVersion: v1
                data:
                SMTP_PASSWORD: IiI=
                SMTP_USERNAME: BMC
                kind: Secret
            ```   
          
        - You can change the username, password, and other details in the secrets file. 
        - To change the username or password, you must encode it into the Base64 format and then add it to the secrets file.
        - To encode the username in Base64 format, run the following command:
        - `echo -n <username> | base64`
        - Copy the encoded username that gets generated.
        - You can do the same for passwords
