# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.69.0.RELEASE]
### Added
- SEC2-1921 Etag support

## [3.68.2.RELEASE]
### Fix
- Returning in http response same x-rho-traceid received in request

## [3.68.1.RELEASE]
### Fix
- Fixing typo in onboarding

## [3.68.0.RELEASE]
### Added
- SEC2-1904 - Getting armadillo traceId from x-rho-traceId http header, not from SAML extensions

## [3.67.1.RELEASE]
### Cleanup
- SEC2-1931 Cleaning up changes made in SEC2-1865 (version 3.57.0), unused since SEC2-1924 (3.60.0)

## [3.67.0.RELEASE]
### Added
- New semaas logger
- SEC2-1953 Not using telephoneNumber
### Fixed
- SEC2-1942 Spanid failing in live

## [3.66.0.RELEASE]
### Added
- SEC2-1861 Bbva on boarding status.

## [3.65.0.RELEASE]
### FIXED
- SEC2-1941 ie8 compatibility

## [3.64.2.RELEASE]
### Fixed
- SEC2-1947 Google Auth is a brand new AC

## [3.64.0.RELEASE]
### Added
- SEC2-1934 Google Auth support

## [3.63.0.RELEASE]
### Added
- SEC2-1937 New metrics to semaas like a ksni (Error metric, AuthTime metric)

## [3.62.2.RELEASE]
### Added
- Lux Error is Authentication Error now

## [3.62.1.RELEASE]
### Hotfix
- SMS otp wasn't being sent in non spanish numbers

## [3.62.0.RELEASE]
### Added
- Webseal disabled for configurable list of SP entity Id's

## [3.61.0.RELEASE]
### Added
- SEC2-1914 adapting the otp behaviour to recognize new fields avoiding repetition

## [3.60.0.RELEASE]
### Added
- SEC2-1924 Logout SLO with webseal is now initiated from logout html template using iframe

## [3.59.0.RELEASE]
### Added
- SEC2-1925 Added http code without description in U/P view when user typed incorrect user/password pair.

## [3.58.0.RELEASE]
### Added
- SEC2-1889 Webseal hot refresh and ivuser timeout management

## [3.57.2.RELEASE]
### Fixed
- SEC2-1918 set OTP country as ESP when GIAM LDAP doesn't return user country

## [3.57.0.RELEASE]
### Added
- SEC2-1865 new /api/logout endpoint

## [3.56.0.RELEASE]
### Added
- SEC2-1883 Changed keystore password. Now it's stored on vault.

## [3.55.0.RELEASE]
### Added
- SEC2-1826 User expired flow like a KSN
- Renew templates and messages

## [3.54.0.RELEASE]
### Added
SEC2-1894 - 2FA mechanisms not available

## [3.52.0.RELEASE]
### Fixed
- Avoid logging properties at startup

## [3.51.0.RELEASE]
### Added
- SEC2-1813-resolve-all-attributes-at-the-same-time-5
Now the data connector resolve all the attributes at the same time, and when the idp goes asking for all the attributes
they are already resolved in the context.
 
This also affect how the info for the otp is retrieved. Now we ask for the corporate phone, the personal phonen and the 
mail at the same time.

## [3.50.0.RELEASE]
### Fixed
- New ServalCLient version

## [3.49.0.RELEASE]
### Added
- SEC2-18849 Metrics init version

## [3.48.0.RELEASE]
### Added
- SEC2-1804: Webseal SSO

## [3.47.9.RELEASE]
### Hotfix
- hotfix/message_user_blocked, add properties

## [3.47.7.RELEASE]
### Bugfix
- SEC2-1800 -> Luxiam client library version upgraded

## [3.47.6.RELEASE]
### Added
- SEC2-1799 -> Default MDQ cache using correct format

## [3.47.5.RELEASE]
### Added
- SEC2-1796 -> Mdq-dto reference updated

## [3.47.0.RELEASE]
### Added
- Integrated new Meigas url and templates based vault

## [3.46.0.RELEASE]
### Added
- Reducing User info calls

## [3.45.0.RELEASE]
### Added
- Java 11 support

## [3.44.3.RELEASE] - [3.44.4.RELEASE]
### Fixed
- This both are is 3.44.2.RELEASE just upgraded because of Jenkins deploy errors.

## [3.44.2.RELEASE]
### Fixed
- SEC2-1754 - Semaas TraceID sent to User Info is well formed now

## [3.44.1.RELEASE]
### Fixed
- SEC2-1751 - User blocked template is missing

## [3.44.0.RELEASE]
### Changed
- extract cert logget to a library

## [3.43.1.RELEASE]
### Fixed
- fix NPE on InputDataConnector with no "attributeNames"

## [3.43.0.RELEASE]
### Added
- SEC2-1711 migrate to new shibboleth syntax version > 3.4.3, ready for 4.0.0 deprecations
---
## [3.41.2.RELEASE]
### Fixed
- Fixed bug when login using a cookie. The idp didn't check with lux what to do and log in the user.
- When trying to clone the templates for the first time doesn't delete the local folder within the templates.

## [3.41.1.RELEASE]
### Fixed
- When using a remote repo for the templates doesn't delete the folder until a successful clone of the new templates.

## [3.41.0.RELEASE]
### Added
- Almighty resolver. Reduce the calls to the user info and enables to change the parameters that we want in the response dynamically. 
- Refresh the templates from the git repo.

## [3.40.0.RELEASE]
### Changed
- SEC2-1625 refresh idp not refreshing templates
- SEC2-1381 resolve attributes using external application config, and only if needed 

## [3.39.0.RELEASE]
### Changed
- SEC2-1580 MDQ Application's refactor

## [3.38.0.RELEASE]
### Added
- SEC2-1626 Merge Kerberos + 343 develop

## [3.37.0.RELEASE]
### Changed
- SEC2-1611 Diferenciar móvil personal del móvil de trabajo
---
## [3.36.1.RELEASE]
### Added
- SEC2-1579 User blocked now returns the correct code

## [3.36.0.RELEASE]
### Added
- SEC2-1579 User blocked

## [3.35.1.RELEASE]
### Fix
- SEC2-1585 Don't handle Cookie info when sp marks ForceAuthn 

## [3.35.0.RELEASE]
### Added
- SEC2-1507 Properties Hot Reload
Properties can be refreshed through the spring bus.

## [3.34.1.RELEASE]
### Fixed
- Fix for 3.34.0.RELEASE bug --> realName attribute resolution protected against non existent ldap entry.

## [3.34.0.RELEASE]
### Added
- SEC2-1572 Userinfo client, concatenation of givenName and sn ldap entries for "realName" attribute (splunk SP)

## [3.33.0.RELEASE]
### Added
- SEC2-1535 sending entity-id as application-id to lux

## [3.32.0.RELEASE]
### Changed
- SEC2-1515 nameid override config in mongo

## [3.31.0.RELEASE]
### Added
- SEC2-1521 Cert Logs return correct ASO codes

## [3.30.1.RELEASE]
### Fixed
- Fill new attribute resolver context when call Attribute-resolver-test endpoint

## [3.30.0.RELEASE]
### Added
- Handled external application attributes when required from mdq

## [3.29.0.RELEASE]
### Added
- SEC2-1483 - templates from templates git repo

## [3.28.0.RELEASE]
### Added
- SEC2-1495 - Attribute-resolver-test endpoint added for test purposes

## [3.25.0.RELEASE]
### Added
- SEC2-1486 - XML Attribute config runtime refresh and reload
- SEC2-1478 - Added the error message on 2FA fail for OTP on the zip file

## [3.24.0.RELEASE]
### Fixed
- Fixing the 3.22.0 Hotfix. Now we check that the attribute searched for gnoss and bpm is roles to return the special path.

## [3.23.0.RELEASE]
### Fixed
- SEC2-1487 - JSESSIONID removed from idp url

## [3.22.0.RELEASE]
### Fixed
- Hotfix (Táctico Fix) to return the roles right to bpm and gnoss

## [3.21.0.RELEASE]
### Fixed
- ConfigServer health indicator changed to /info

## [3.20.0.RELEASE]
### Added
- added the Aso code to the logs and the SAML response

## [3.19.0.RELEASE]
### Fixed
- Handle cookie info when there are not active auth results in mongo (Cookie X file)

## [3.18.0.RELEASE]
### Added
- print region along with transaction id when error

## [3.17.0.RELEASE]
### Added
- GNOSS integration

## [3.16.0.RELEASE]
### Added
- always forward/create header true-client-ip for Lux

## [3.15.0.RELEASE]
### Added
- Emulate spring.cloud.config.label property 

## [3.14.0.RELEASE]
### Added
- Added Origin to the data sent to Lux


## [3.13.0.RELEASE]
### Fix
- Sensitive information ok cookie not sent to lux needlessly.

## [3.12.0.RELEASE]
### Fix
- Update Cookie info after authentication (authentication methods used) 
  
## [3.11.0.RELEASE]
### Fix
- Let the user select another otp method whatever makes the otp to fail.
  Related to 3.7.0.RELEASE


## [3.10.0.RELEASE]
### Added
- Lux calls on a loop, not 2 fixed calls
- OTP Phone number fill with telephoneNumber ldap's field AND the classic mobile field
- Conditional Captcha on front and back

### Fix
- Refactor with new Methods and Obligations Codes. Clean Lux and Auth contexts


## [3.7.0.RELEASE]
### Fix
- Checks if the otp send request returns a timeout and shows an error, telling the user to reselect another otp sending method.

## [3.6.0.RELEASE]
### Added
- Added ldap field - telephoneNumber
- Recaptcha as brute force prevention
- Conditional Captcha
- Block ECP conditional
### Hotfix
- Not using cookie info when session expired

---
## [3.3.1.RELEASE]
### Fix
- New appender for SemaaS - Replace tab character for two "space" chars 

---
## [3.3.0.RELEASE]
### Added
- ECP profile with user/password always returns UNAUTHORIZED response from Serval client from now

---
## [3.2.0.RELEASE]
### Added
- new header added into lux request: idp-domain, contains idp server name (http request hostname)

---
## [3.1.0.RELEASE]
### Changed
- Correlation Id header in rest endpoints not mandatory anymore

### Added
- Semaas Trace Id now appears in login page message on error condition for all SPs (not only Armadillo)

### Fixed
- New appender for semaas: 
    - Max length of log message printed. 
    - log level semaas-compliant for warning messages.

---
## [3.0.0.RELEASE]
### Removed 
 - Removed endpoints for Sp managing and static policies
 
---
## [2.3.2.RELEASE] - 2018-11-21
### Fixed
- SAML request logged when semaas context is already initialized (so it is traceable now)

---
## [2.3.1.RELEASE] - 2018-11-21
### Fixed
- Fixed ISO8601 format for timestamps in CERT logs

---
## [2.3.0.RELEASE] - 2018-11-21
### Changed
- Shibboleth Utils removed from UserInfo data connector, as transaction id is no longer retrieved from saml context in this phase of saml dialog
- CERT logs data added: 
    - Error description from Serval
    - ISO8601 UTC timestamp added for every step
    - Added SAML Binding used in saml process
- Docker compose and script for using local compiled versions of all Nauthilus microservices.


---
[Unreleased]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp
[3.39.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.39.0.RELEASE
[3.38.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.38.0.RELEASE
[3.37.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.37.0.RELEASE
[3.36.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.36.0.RELEASE
[3.35.1.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.35.1.RELEASE
[3.35.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.35.0.RELEASE
[3.34.1.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.34.1.RELEASE
[3.34.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/projects/KJRYE/repos/kjrye_nauthilus_idp_samlidp/browse?at=refs%2Ftags%2Fv3.34.0.RELEASE
[3.33.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.32.0.RELEASE&to=v3.33.0.RELEASE
[3.32.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.31.0.RELEASE&to=v3.32.0.RELEASE
[3.31.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.30.1.RELEASE&to=v3.31.0.RELEASE
[3.30.1.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.30.0.RELEASE&to=v3.30.1.RELEASE
[3.30.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.29.0.RELEASE&to=v3.30.0.RELEASE
[3.29.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.28.0.RELEASE&to=v3.29.0.RELEASE
[3.28.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.26.0.RELEASE&to=v3.28.0.RELEASE
[3.26.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.25.0.RELEASE&to=v3.26.0.RELEASE
[3.25.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.24.0.RELEASE&to=v3.25.0.RELEASE
[3.24.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.23.0.RELEASE&to=v3.24.0.RELEASE
[3.23.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.22.0.RELEASE&to=v3.23.0.RELEASE
[3.22.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.21.0.RELEASE&to=v3.22.0.RELEASE
[3.21.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.20.0.RELEASE&to=v3.21.0.RELEASE
[3.20.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.19.0.RELEASE&to=v3.20.0.RELEASE
[3.19.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.18.0.RELEASE&to=v3.19.0.RELEASE
[3.18.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.17.0.RELEASE&to=v3.18.0.RELEASE
[3.17.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.16.0.RELEASE&to=v3.17.0.RELEASE
[3.16.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.15.0.RELEASE&to=v3.16.0.RELEASE
[3.15.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.14.0.RELEASE&to=v3.15.0.RELEASE
[3.14.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.13.0.RELEASE&to=v3.14.0.RELEASE
[3.13.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.12.0.RELEASE&to=v3.13.0.RELEASE
[3.12.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.11.0.RELEASE&to=v3.12.0.RELEASE
[3.11.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.10.0.RELEASE&to=v3.11.0.RELEASE
[3.10.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.6.0.RELEASE&to=v3.10.0.RELEASE
[3.6.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.5.0.RELEASE&to=v3.6.0.RELEASE
[3.3.1.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.3.0.RELEASE&to=v3.3.1.RELEASE
[3.3.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.2.0.RELEASE&to=v3.3.0.RELEASE
[3.2.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.1.0.RELEASE&to=v3.2.0.RELEASE
[3.1.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v3.0.0.RELEASE&to=v3.1.0.RELEASE
[3.0.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v2.3.2.RELEASE&to=v3.0.0.RELEASE
[2.3.2.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v2.3.1.RELEASE&to=v2.3.2.RELEASE
[2.3.1.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v2.3.0.RELEASE&to=v2.3.1.RELEASE
[2.3.0.RELEASE]: https://globaldevtools.bbva.com/bitbucket/plugins/servlet/repository/KJRYE/kjrye_nauthilus_idp_samlidp#?&from=v2.2.1.RELEASE&to=v2.3.0.RELEASE
