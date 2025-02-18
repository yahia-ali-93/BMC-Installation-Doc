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
        - You should leave the username and password empty if you no longer need authentication to use SMTP (for fake SMTP for ex)
    
    - Update RSSO and rollout restarts as indicated in the following link: https://community.bmc.com/s/article/BMC-Helix-IT-Operations-Management-ITOM-Deployment-OnPrem-How-to-perform-a-post-deployment-update-of-the-SMTP-details-of-an-OnPrem-ITOM-environment
        - If the link doesn't open, first login to RSSO and go to tennants. 
        - Select a tennant by clicking on the pin icon
        - Go to services and then to email server section
        change the credentials to follow the SMTP server you're using
        - Do the same for the other tennant
        - Rollout restart of the following
            - notification-service
            - adereporting-apiservice
            - adereporting
            - adereporting-report-generator-service
            - notification-service
            - rsso
        - Rollout restarts can be done using:
            - For deployments: `kubectl rollout restart deploy <service> -n <NS>`
            - For STS: `kubectl rollout restart sts <service> -n <NS>`
            - To find out which is which you can run the following command: `kubectl get deployments,sts -n <NS> > deployments.txt`
                - This saves all deployments and STSs to a file named `deployments.txt` in the current directory. You can then `vim` and search within it

# Using Fake SMTP
It might happen that you're getting errors while installing the common services regarding SMTP, or you don't have any SMTP credentials from the client at the time of running the installation. If so, you need to follow steps in the following link
- https://community.bmc.com/s/article/BMC-Helix-Innovation-Suite-How-to-run-a-temporary-SMTP-server-to-receive-Helix-Platform-Common-Services-installation-emails

However, For simplicity, I've written down the steps in my prefered method of the alternatives mentioned in the aforementioned link.
- `yum -y install postfix tokyocabinet`
- Get the hostname of the VM You'll be running postfix's `smtp-sink` from using: `hostnamectl`
- run `smtp-sink -u root -d -c <hostname>:2525 100&`
- The tool catches all emails and saves them to the folder you ran the command above from.
- The tool saves every email as a file that starts with `-` (for ex: `-c259cec3b`)
- In case you're using this to get the tennant onboarding email, then the email recieved is in HTML format. Run the following   command to get the link you need to visit to continue the `hannah_admin` user creation process
    - `tcucodec quote -d ./<email recieved> |  grep confirm-registration | sed -e 's/.*href="//g;s/".*//g' -e 's/&#61;/=/g'`