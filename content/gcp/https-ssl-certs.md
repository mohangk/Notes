---
draft: true
title: Https Ssl Certs
categories:
  - Gcp
---
##### Create self signed certs for used by Internal load balancer

https://cloud.google.com/load-balancing/docs/ssl-certificates/self-managed-certs#create-key-and-cert

1. openssl genrsa -out private-key.pem 2048

2. Create an openssl config file
   
   ```
   [req]
   default_bits              = 2048
   req_extensions            = extension_requirements
   distinguished_name        = dn_requirements
   
   [extension_requirements]
   basicConstraints          = CA:FALSE
   keyUsage                  = nonRepudiation, digitalSignature, keyEncipherment
   subjectAltName            = @sans_list
   
   [dn_requirements]
   countryName               = Country Name (2 letter code)
   stateOrProvinceName       = State or Province Name (full name)
   localityName              = Locality Name (eg, city)
   0.organizationName        = Organization Name (eg, company)
   organizationalUnitName    = Organizational Unit Name (eg, section)
   commonName                = Common Name (e.g. server FQDN or YOUR name)
   emailAddress              = Email Address
   
   [sans_list]
   DNS.1                     = SUBJECT_ALTERNATIVE_NAME_1
   DNS.2                     = SUBJECT_ALTERNATIVE_NAME_2
   
   EOF
   
   ```

3. Create a CSR
   
   ```
   openssl req -new -key PRIVATE_KEY_FILE \
       -out CSR_FILE \
       -config CONFIG_FILE
   ```


