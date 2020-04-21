# External configuration

TODO Steps for overwriting shibboleth class:
- Place the new  overwrite class in idp-external-configuration module (not mandatory, but preferable)
- Configure maven-shade plugin on idp-external-configuration/pom.xml (uber-jar include all classes except the one we overwrite)
- Full maven clean install in order to get all qa checks passed. In each case, analysis must be done: 
  * jacoco (you may configure exceptions)
  * checkstyle
  * findbugs
  * ...
- Exclude original class jar from idp overlay war file in idp-webapp-overlay\pom.xml, maven-war-plugin -> "packagingExcludes" configuration section  