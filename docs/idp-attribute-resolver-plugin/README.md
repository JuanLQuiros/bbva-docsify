Session Attribute Resolver plugin
=================================
Data connector for Shibboleth attribute resolver to get session information as attributes along with other attributes 
 that will be retrieved from an external endpoint (UserInfo).

Nauthilus requirements specify some user session information must be returned to SP as attribute assertions.
This plugin extract that information from session context and prepare it to be used as attributes.


Collected information
---------------------
Next information is collected by the plugin from the User Session.

### User UID
This attribute is the *'principal id'* used by the user for login. This information is collected form the JWT token generated as
session id the first time the user logged in.

### Authentication Controller List
This attribute store the list of AC's used to authenticate the user. This information is collected form the JWT token generated as
session id the first time the user logged in.

### SP opened sessions list
This attribute store the list of SP's whose the user has a opened session currently. This information is collected from Shibboleth
session context.

Nauthilus will try to load any other attribute configured at attribute-resolver and attribute-filter configuration files, from the
 external UserInfo endpoint.

