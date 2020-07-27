---
title: How to generate a wildcard cert CSR with a config file for OpenSSL
date: '2017-06-07T10:50:10.165Z'
excerpt: The code snippet
layout: post
---
**The code snippet**

    $ mkdir domain.com.ssl && cd domain.com.ssl  
    $ openssl genrsa -out ./domain.com.key 2048
    $ openssl req -config csr.conf -new  -key ./domain.com.key -out ./domain.com.csr -verbose

**First though, the csr.conf** **file** looks like this

    [ req ]  
    default_bits       = 4096  
    default_md         = sha512  
    default_keyfile    = domain.com.key  
    prompt             = no  
    encrypt_key        = no  
    distinguished_name = req_distinguished_name
    # distinguished_name  
    [ req_distinguished_name ]  
    countryName            = "DE"                     # C=  
    localityName           = "Berlin"                 # L=  
    organizationName       = "My Company"             # O=  
    organizationalUnitName = "Departement"            # OU=  
    commonName             = "*.domain.com"           # CN=  
    emailAddress           = "me@domain.com"          # CN/emailAddress=

**Guides that help:**

[**How to create multidomain certificates using config files**  
*openssl can make life easy be creating its keys, CSRs and certificates on the basis of config files. Creating these…*apfelboymchen.net](https://apfelboymchen.net/gnu/notes/openssl%20multidomain%20with%20config%20files.html "https://apfelboymchen.net/gnu/notes/openssl%20multidomain%20with%20config%20files.html")[](https://apfelboymchen.net/gnu/notes/openssl%20multidomain%20with%20config%20files.html)

[**Generate a CSR with OpenSSL**  
*The Rackspace Cloud is not a certificate authority (and does not resell SSL certificates), so you need to go to a third…*support.rackspace.com](https://support.rackspace.com/how-to/generate-a-csr-with-openssl/ "https://support.rackspace.com/how-to/generate-a-csr-with-openssl/")[](https://support.rackspace.com/how-to/generate-a-csr-with-openssl/)

[**The Most Common OpenSSL Commands**  
*One of the most versatile SSL tools is OpenSSL which is an open source implementation of the SSL protocol. There are…*www.sslshopper.com](https://www.sslshopper.com/article-most-common-openssl-commands.html "https://www.sslshopper.com/article-most-common-openssl-commands.html")[](https://www.sslshopper.com/article-most-common-openssl-commands.html)
