Nauthilus Saml Performance Tests
======================

An Gatling based test has been created in other to test the Nauthilus Ids's performances.

The steps to use it are quite simple:

* Deploy the environment using the docker-compose created ad-hoc for the test:

```
configrepo=$(pwd)/configrepo runpath=$(pwd)/nauthilus-shibboleth-idp/idp-webapp-overlay/target  docker-compose -f docker/testing/dockercomp-performance.yml up

```

This docker-compose will only run the necessary containers to test the Idp and it will mock the behaviour of the component that interacts with it.

* Run the Gatling test

The Gatling based test is store at /src/test/scala/LoginSimulation.scala. By default, it is configured to test 15 users trying to Login at the same time 100 times. 1500 logins in total.

A reference result report is saved at /src/test/scala/result/reference/index.html. You can use it to compare your result with the previous ones.

To execute the test, just write

```
mvn --non-recursive gatling:execute -Djsse.enableSNIExtension=false -Pperformance
```

At the end, a report will be created on /src/test/scala/result/{your-report}
