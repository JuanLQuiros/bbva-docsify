### Quick instructions

> NOTE: This document assumes root samlidp project as your working directory in all consoles 

- Start kerberos kdc: 
```
cd docker/kerberos-kdc
docker-compose up
```
- Copy generated base64-keytab for idp service from kerberos-kdc to the shared volume with idp container:
```
docker cp kerberos-kdc:/tmp/base64.idp.local.keytab ./docker/kerberos-kdc/idp-keytab
```
- Start IdP docker-compose (kerberos version for now):
```
configrepo=$(pwd)/configrepo templates=$(pwd)/templates runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target docker-compose -f docker/dockercomp-local-kerberos.yml up
```
- login with your kerberos kdc:
```
kinit xe00001@NAUTHILUS.COM
(password: 1234)

# this will show your initial ticket:
klist
```
> NOTE: you must have your local kerberos client properly configured (see kerberos client configuration)

> NOTE: You can login with otp users (otpmail1, otpmail2, otpsms1, otpsms2, otpsms3) and PRU0001 as well (see ./docker/kerberos-kdc/init-script.sh, array "user_principals_to_create")
- Open local SP test in browser (preferably firefox for quick testing): [https://sp.local:8443/secure/index.html]. You should directly access the SP protected page without need to login with Naitilus, and from now you have your awesome SSO cookie.
> NOTE: You must have your browser properly configured for SPNEGO (see browser configuration)


### Common errors and solutions
TBW

### Kerberos Client Configuration

You must install the client first, if you user a debian base distro exec this command:
```sh
sudo apt install krb5-user
```

During installation it will prompt you asking for configuration values. You can put whenever you want here because we will use a custom configuration file later.

Once finished there will be a configuration file in /etc/krb5.conf. We must overwrite this file with the one provided in: ./docker/kerberos-kdc/config/krb5.conf

We have to add this entry to /etc/hosts:
```
127.0.0.1   kdc-kadmin
```
### SPNEGO Browser Configuration (Linux)
##### Firefox

Insert into _about:config_ and set the variable "network.negotiate-auth.trusted-uris" value to "local"

##### Chrome
TBW