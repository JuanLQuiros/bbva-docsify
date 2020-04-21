
# Shibboleth-IdP3-External-Login-Auth
> Working connector for Shibboleth login with external authenticator. 

External authentication module for Shibboleth IdP v3.
Modify the Shibboleth User/Password flow. This module modify the authn/Password to use an external system for authentication
instead the out of the box login repository implementations (LDAP, JAAS, Kerberos).

This implementation support authentication through User-Password Authentication Controller.

Requirements
------------

Shibboleth IdP v3.2.x
Java 8

Installing
----------

* Compile source code with maven - ```mvn clean package```
* Copy and extract the zip compresed distribution file form your authentication method.
    nauthilus-authn-shibboleth-AC-dist/target/nauthilus-authn-shibboleth-ac-dist-<version>-UPassAc.zip


* Unzip the compressed file in $IDP_HOME (/opt/shibboleth by default).


This package modify $IDP_HOME/conf/authn/password-authn-config.xml. Review it if it contains your own modifications.

### Rebuild idp.war
* run $IDP-HOME/bin/build.sh
* If you need, move that war-file to containers "webapps" directory (tomcat, jetty, etc)
* Restart container


User Password Authentication Controller
---------------------------------------

This plugin reuse some shibboleth system properties to get access to ldap server. Next, that properties are
explained:

**idp.authn.LDAP.ldapURL** LDAP server connection string formated as: ldap://host:port

**idp.authn.LDAP.baseDN** Search users DN resolution (example: cn=Users,ou=native,o=igrupobbva) 

**idp.authn.LDAP.userFilter** User identification format (example: (cn={user}))
