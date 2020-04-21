# docker-tomcat-nexus
Docker image based in Alpine and with java 8.45.14 jdk from Oracle installed along with tomcat 8 and native library loaded

# Overview
This image is intended to provide a small docker base image for using tomcat 8 with APR native library, useful for production environment 
 with automatic deployment of a nexus artifact.
 
# Exposed ports
This image exposes 8080 port for http communications and 1443 for https
 
# Build instructions

* The original image can be built this way

```

docker build -t <org_id>/<image_name> .
```


* Building a docker image with embedded Jar:

```

docker build -t [name] . --build-arg GROUP_ID="[APP_GROUP_ID]"  --build-arg ARTIFACT_ID="[APP_ARTIFACT_ID]" --build-arg VERSION="[APP_VERSION]"

```


# Run instructions

You can run the image with the following params: 

* [GROUP_ID] -> mandatory, maven groupId of the maven artifact
* [ARTIFACT_ID] -> mandatory, artifactId of the maven artifact
* [VERSION] -> optional, LATEST if no provided
* [TYPE] -> optional, maven type, war, zip...
* [JAVA_OPTS] -> optional, list of arguments for the JVM
* [TOMCAT_KEYSTORE_PASSWORD] -> optional, if no provided 'changeit' will be used, keystore password for tomcat ssl certificate
* [TOMCAT_SSL_SUBJECT] -> optional, if no provided '/C=ES/ST=Madrid/L=Madrid/O=BBVA/OU=BBVASecurity/CN=bbva.es' will be provided
* [TOMCAT_MANAGER_PASSWORD] -> optional, password for tomcat manager ui access, if no provided 'changeit' will be provided
* [CONTEXT_PATH] -> optional, context path desired or it will be the name of the war file.
* [DEBUG] ->  optional if informed, launches tomcat with debug port 5005 opened, so it's necessary to export this port outside
 docker image (-p 5005:5005).
 
i.e: 


```

docker run -d -p 443:1443 -e GROUP_ID=<groupid> \
    -e ARTIFACT_ID=<artifact> \
    -e ARGS="-Dspring.profiles.active=ksni3" --name oauth-server maven-tomcat

```

# Local deployment

In case you need to deploy a local war, you can map the folder with the war generated to */tmp/todeploy* dir inside docker image, and it
 will be deployed, not needing to take care about groupid and artifact id parameters
 
# https SSL custom .pfx
 
In case you need to use your custom https certificates instead the locally generated, you can map the folder with the .pfx keystore to */tmp/pfx* dir inside docker image.
Keystore password must be provided using TOMCAT_KEYSTORE_PASSWORD param.
 
# Environment variables

The image create the next environment variables

```
JAVA_VERSION_BUILD=14
HOSTNAME=eb041aeeb27d
TERM=xterm
CATALINA_HOME=/opt/tomcat
JAVA_VERSION_MAJOR=8
TOMCAT_HTTPS_PORT=443
NEXUS_PASSWORD=jenkinsrest
TOMCAT_MAJOR_VERSION=8
JAVA_OPTS= 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/jdk/bin:/opt/tomcat/bin:/bin
PWD=/
JAVA_HOME=/opt/jdk
TOMCAT_HTTP_PORT=80
SHLVL=1
HOME=/root
JAVA_PACKAGE=jdk
NEXUS_LOGIN=jenkinsrest
TOMCAT_MINOR_VERSION=8.0.11
TOMCAT_JPDA_PORT=5005
JAVA_VERSION_MINOR=45
```

# Size

The size of the built image is about 373M.

# Changelog

v2.1.0
https://globaldevtools.bbva.com/jira/browse/SEC2-606. Certificate is now obtained from Vault via API and installed on deploy time instead of build time

v2.0.0
Docker build arguments for create images with embedded war


v1.1.1

Custom https pfx can be added instead generate it.

v1.1.0

JCE libraries added to JVM to allow full strength encryption policy.

v1.0.3

Fix: Download Release Candidate (RC) version  from nexus.