
1)**Create the certificate using openssl at mkdir /usr/local/docker/localregistry/certs**


a) **openssl genrsa -out ca.key 2048**

b) **openssl req -new -x509 -days 10240 -key ca.key -out ca.crt**
       Country Name (2 letter code) [AU]:IN
       
       State or Province Name (full name) [Some-State]:MH
       
       Locality Name (eg, city) []:MUM
       
       Organization Name (eg, company) [Internet Widgits Pty Ltd]:ABC
       
       Organizational Unit Name (eg, section) []:IT
       
       Common Name (e.g. server FQDN or YOUR name) []:mumbaisupport.com
       
       Email Address []:test@mumbaisupport.com


       
c) **openssl x509 -in ca.crt –text**

d) **openssl genrsa –out doamin.key 2048**

e)**openssl req -new -key domain.key –out domain.csr**
      Country Name (2 letter code) [AU]:IN
      
      State or Province Name (full name) [Some-State]:MH
      
      Locality Name (eg, city) []:MUM
      
      Organization Name (eg, company) [Internet Widgits Pty Ltd]: <Any>
      
      Organizational Unit Name (eg, section) []:IT
      
      Common Name (e.g. server FQDN or YOUR name) []:<server ip>
      
      Email Address []:<your email id>

f) **cat>>v3.txt**

      Append the below detail in v3 file.
          
          authorityKeyIdentifier=keyid,issuer
          
          basicConstraints=CA:FALSE
          
          keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
          
          extendedKeyUsage = serverAuth,clientAuth
          
          subjectAltName = @alt_names
          
          [alt_names]
          
          IP.1  = <IP>

g) **openssl x509 -req -extfile v3.ext -days 1024 -in domain.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out domain.crt**

2) **docker-compose –f docker.yml up** 

3)  **docker ps**

4)   **curl -X GET https://<IP>:5000/v2/_catalog**
