Nauthilus Idp Acceptance tests
==============================

## Environment startup

After selenium installation following the install instructions document at https://docs.google.com/a/paradigmadigital.com/document/d/1umfoV4tmyZWbgG3i1JO8uktXI41TDGy7aorvGMtMGsg/edit?usp=sharing

### Firefox profile
To work properly with firefox, some additional profile configuration is needed.

* $ firefox -ProfileManager -no-remote
* Create a profile, e.g. “selenium”
* Click at ‘Start firefox’, once firefox loads, open the SPs pages, and add the exception manually.
* Click at Preferences/advanced and network tab.
* Limit cache space to 0 MB

### Start selenium
```
java -jar selenium-server-standalone-3.0.0-beta3.jar
```

### Start local docker environment
Follow the nauthilus-saml-idp project README.md instructions to have a docker compose local environment (dockercomp-local.yml) ready to test.

## Environment variables

Features in this suite needs to be parametrized with SPs and browser used for testing.
Next variables must be setup in the JVM:

* browser.  Browser used for testing. It can be 'firefox' or 'chrome'. 
* selenium.address. Selenium service URL.
* SP1. Base URL for first SP used.
* SP2. Base URL for second SP used.
* IDP. Base URL for Idp used.
* ECP_TESTER. Base URL for ecp tester that emulates ASO integration

A typical JVM options could be:
```
-Dbrowser=chrome -Dselenium.address=http://localhost:4444/wd/hub -DSP1=https://sp.local:8443 -DSP2=https://sp2.local:9443 -DIDP=https://idp.local -DECP_TESTER=http://localhost:8080
```

## Execute tests from maven
Maven project include a profile to execute the test. This profile is configured to execute test against a complete docker environment
that include the IdP, SP for testing, ecp-tester and selenium docker images.

To execute all idp test suite, start docker compose first:
```
templates=$(pwd)/templates configrepo=$(pwd)/configrepo runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target metadatas=$(pwd)/confselenium/relying-metadata docker-compose -f docker/dockercomp-selenium.yml up

```

and launch the test suite

```
mvn clean -Pidp-acceptance-tests test
```

Test result will be stored in target/cucumber-html-report directory.

## Changelog
#### v1.0.3
##### New scenarios

* Single SP delegates authentication with incorrect username and password.
* Single SP delegates authentication with correct username and password.
* SSO between two SP's.
* SSO using ECP profile in ASO use case.
* Global Logout triggered by a Single SP.
* Global Logout triggered by Idp.
* Local Logout triggered by a Single SP.
* Attribute received by SP through assertions.


##### Deprecated scenarios
