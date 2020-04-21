Nauthilus Saml Features
======================

* Refresh the configuration through the spring bus
* Works with any compliant SAML 1.1/2.0 Service Provider implementation.
* SAML's Service Provider self registration service.
* SSO cookies in JWT format.
* User Sessions logged at BBDD.
* Single Logout (SLO) implemented.
* Externalized properties that can be updated online without restarting Nauthilus.
* Administration via REST Api and Front UI.
* User info parameters returned after successful login in a configurable way.
* SAML's ECP profile active.
* LuxIAM integration (integrated callings for anti-fraud services and 1st and 2nd factor).
* Armadillo attributes passed through SAML extensions mechanism and sent to LuxIAM.
* User-Info endpoint added for accessing any registered user attribute from employee ldap source.
* Authentication Mechanism pages Templating service provided.
* Service Provider registration moved to IDP rest api.
* Customized returned attributes CRUD rest api for sp's registered.
* Nauthilus Policy Configuration (aka "static configuration").
* Serval integration for CA's 
* Sentry integration
* Correlation id propagated among all the pieces (serval, lux, etc.)
* Connected to configserver via MQ queues in order to be refreshed
* jmustach templates engine
* Cookies configurable via parameters
* Log generation for bbva security cert.