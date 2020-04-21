[[View Changelog here]](CHANGELOG.md)

## Projects

- profile-spring-password
- mongo-storage-service
- nauthilus-authn-shibboleth-plugin
- nauthilus-luxiam-plugin
- nauthilus-shibboleth-idp
- saml-utils
- shibboleth-external-configuration
- nauthilus-shib-sessions
- idp-semaas-client

## Docker compose
### Local environment

This project provide a docker compose configuration for local testing called _dockercomp-local.yml_ .

You should initialize your local configrepo in order to use it. Use the _git init_ command in your configrepo folder and then then _add_ and _commit_ the files inside.
You must specify this folder to the docker compose using the configrepo variable. By default it uses the configrepo provided within this project: configrepo=$(pwd)/configrepo

*Configuring the templates*
You can specify which templates you want to be used in the local deploy.
It can be done in two ways:

* Specifying a local folder you wan to use. To do this you must set the *templates* variable with the path to your local templates.
Example: _templates=/home/user/workspace/nauthilus/kjrye_nauthilus_idp_templates_repo_dev_

* Via Url. You can use a remote repository as a source for your templates. You must mount a volume with your private key (called id_rsa) to allow your local idp to access the remote repository.
This must be done in the docker compose you are using, and your volume must be pointing to /home/tomcat/.ssh (because tomcat is the user in the docker container).
Example:
```
  shibbolethidp:
  ...
    volumes:
       - ~/.ssh:/home/tomcat/.ssh
```
If your key is password protected you must specify it using this variable _idp.loginviews.git.rsa.password_ otherwise the idp will assume that your are using none.
```
    -Didp.loginviews.git.rsa.password=myId_rsaPassword
```

And finally we have to specify which branch and repository we want to use. If no branch is specified it will use master by default.
```
    -Didp.loginviews.git.branch.name=default
    -Didp.loginviews.git.uri=ssh://git@globaldevtools.bbva.com:7999/kjrye/kjrye_nauthilus_idp_templates_repo.git 
```

Note: If you want to use a remote repository in a local deploy be sure that the templates are not mounted as a volume in your local compose (- ${templates}:/opt/tomcat/templates) or otherwise it will fail while attempting to delete the templates (just before cloning the remote repository).

In summary, you must mount your .ssh folder, and specify your private key password (if it existed) and set the remote rempository and branch.
This way with docker-compose you would be able to launch the whole system in a local fashion, without dependencies on external deployments.

*_Examples:_*

Local launch, using the templates located within this repo:
The next example show how to launch a docker-compose from project-home-dir, once the local git repo is initialized and committed the files.
```
configrepo=$(pwd)/configrepo templates=$(pwd)/templates runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target docker-compose -f docker/dockercomp-local.yml up 
```

Local launch enabling the rabbitMq bus in order to be able to refresh via the monitor endpoint in the configserver.
```
NAUTHILUS_HOME=/home/user/workspace/nauthilus/ configrepo=$(pwd)/configrepo templates=/tmp/templates runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target docker-compose docker-compose -f docker/dockercomp-rabbitmq.yml up
```

### To test refresh configuration through rabbitmq bus

You can test the hot refresh configuration through rabbitmq using this docker compose file: docker/dockercomp-rabbitmq.yml

```
NAUTHILUS_HOME=<PATH OF YOUR NAUTHILUS GIT ROOT> configrepo=$(pwd)/configrepo templates=$(pwd)/templates runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target docker-compose -f docker/dockercomp-rabbitmq.yml up 

```

To test this you can use either the manager, or send:
**POST** https://{{url}}{{port-configserver}}/monitor
{"path":"**"} // this will refresh all


**POST** https://{{url}}{{port-configserver}}/monitor
{"path":"idpSAML"} // this will refresh only the idp



### To test HA local environment

```
nginx=$(pwd)/docker/nginx templates=$(pwd)/templates configrepo=$(pwd)/configrepo runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target metadatas=$(pwd)/docker/relying-metadata docker-compose -f docker/dockercomp-ha-local.yml up
```

### Local environment using local compiled versions of all Nauthilus microservices:

This compose uses our generic docker maven image to embed in it the target-jar compiled version of all Nauthilus components (Manager, MDQ server, ...)

NOTE: You must have every project generated with maven in his corresponding target folder.
NOTE2: You must provide the root of your git home for all nauthilus projects, using NAUTHILUS_HOME variable
  
```
NAUTHILUS_HOME=<PATH OF YOUR NAUTHILUS GIT ROOT> docker-compose -f docker/dockercomp-local-jar.yml up

```

A script for automatically compile all projects prior to execute this docker compose is provided. In order to use it:

(into ./docker folder)

```
NAUTHILUS_HOME=<PATH OF YOUR NAUTHILUS GIT ROOT> BUILD_REPOS=[Y:N:undefined] QUICK_MAVEN=[Y:N:undefined] ./docker-compose-build-local-jar.sh

```
- if BUILD_REPOS variable exists in environment, and has "Y" value, this script compiles all projects needed, and run docker-compose if nothing fails 
- if QUICK_MAVEN variable exists in environment, and has "Y" value (and BUILD_REPOS=Y), maven build is executed without tests, checkstyle, ... (in other words, in fast manner).

### compass demo

This project provides a docker-compose for emulating Compass BBVA integration. It uses a 2FA of Security Question.
You have to launch

```
configrepo=$(pwd)/configrepo runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target docker-compose -f docker/dockercomp-compass.yml up

```

For testing the 2FA, enter _openplatformast2_ as login (with any password), and in the Security Question page, you can enter what answer you desire.

 
### selenium environment

This project provide a test environment for selenium also. That environment include the nauthilus services, selenium containers to execute tests using mozilla
or chrome browsers and a container with 'nauthilus-ecp-tester' project deployed to emulate ASO integration scenarios.

launch environment for running selenium test with next command:

```

configrepo=$(pwd)/configrepo runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target metadatas=$(pwd)/confselenium/relying-metadata docker-compose -f docker/dockercomp-selenium.yml up

``` 

In case you want to test an specific and already uploaded version, you should use this other script with the proper modifications:
  
```

configrepo=$(pwd)/configrepo version=1.0.2.RELEASE metadatas=$(pwd)/docker/relying-metadata docker-compose -f docker/dockercomp-selenium-version.yml up

```
  
### hosts config

Include next hosts in your /etc/hosts file.

```
127.0.0.1   idp.local
127.0.0.1   sp.local
127.0.0.1   sp2.local
127.0.0.1   armadillo.local
127.0.0.1   mongo.local
127.0.0.1   configserver.local
127.0.0.1   manager.local
127.0.0.1   mdq.local
127.0.0.1   zuul.local
127.0.0.1   userinfo.local
127.0.0.1   ldap.local
```


### Available services

Once the environment is started, next services are available to be used for idp debugging and test the idp.

#### Nauthilus idp metadata

https://idp.local/idp/metadata

####  Shibboleth sp 1

https://sp.local:8443/secure/index.html

This SP is configured with next options:

* NameId_Format=urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress; 
* ECP enabled; 
* SAML extensions enabled;

If request signed are disabled in metadata file, you can run IdP-Initiated SSO:
https://idp.local/idp/profile/SAML2/Unsolicited/SSO?providerId=https%3A%2F%2Fsp.local%3A8443%2Fshibboleth-sp

####  Shibboleth sp 2

https://sp2.local:9443/secure/index.html

This SP is configured with next options:

* NameId_Format=urn:oasis:names:tc:SAML:1.1:nameid-format:transient; 
* ECP disabled; 
* SAML extensions disabled;

####  Armadillo SP

https://armadillo.local:10443/armadillo-example-saml

This is a simple armadillo sp (include SAML delegation only) to test integration
with it.

####  Nauthilus Manager

https://manager.local:11443/nauthilus-manager/upload


#### DYNATRACE

In order to embeed a dynatrace agent in our Idp Container, you must add the line

DYNATRACE     = "{agent_name}"

where {agent_name} is the name you and OCTA decided to named the container. In our case, we only have one name, which it's NauthilusQA.

So, you must add:

DYNATRACE       = "NauthilusQA"

**IMPORTANT** You must add the line on the **nauthilusDockerEmbedded** group!



# Changelog
[changelog](CHANGELOG.md)

# Features
[features](features.md)

# Performance
[performance](performance.md)

# URI's
[uris](uris.md)

# Webseal SSO
[webseal](webseal.md)

# Reloadable Spring beans
[see this](reloadable-beans.md)