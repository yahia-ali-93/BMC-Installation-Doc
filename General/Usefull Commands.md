# Useful commands while dealing with clusters
- To create a pod and `sh` inside of it you can use the following command
    - `kubectl run -it test-dns3 -n helixdev-itom --image=alpine:latest -- sh`
    - To attach to that pod you can use this: `kubectl attach test-dns3 -c test-dns3 -i -t`
    - To add a package to that pod ue `apk add <package-name>`

# Useful commands when dealing with certificates
- To check a url and see if it is trusted in your environment
    - `curl -Iv https://link.domain.com`
    - `openssl s_client -CAfile /etc/ssl/certs/ca-certificates.crt -verify 12 -showcerts -connect link.domain.com:443`
- To check the issuer and subject of a certificate use the following command
    - `openssl storeutl -noout -text -certs ./custom_cacert.pem| grep -E -i 'subject:|issuer:'`