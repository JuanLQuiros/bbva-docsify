Static Content Policy
=====================

This project implements a Static Content Policy service. It provides the interface to manage IdP configurations.

IdP configuration is searched in DB by Profile name. In case it's not present, default configuration will be loaded instead.

Default configuration can be provided either from classpath or external file. To specify file path or classpath -DdefaultConfigLocation
 argument must be passed:
 
 - Classpath -> -DdefaultConfigLocation=/path/to/resource.json
 - File path -> -DdefaultConfigLocation=file:///path/to/resource.json
 
If any of these options could be loaded, "defaultIdpConfig.json" resource will be loaded.


In order to load into MongoDB a new configuration you must compose a json file with the following structure:

```
[
{
  "_id" : "profileName",
  "servalUrl" : "servalUrl",
  "luxUrl" : "https://luxiampre.i4slabs.com/pdp/decision",
  "luxFirstCall" : true,
  "luxSecondCall" : false,
  "availableAcs": ["01","13","35","00"]
}
]
```

And then import json file into Nauthilus Database:

```
mongoimport --db nauthilusShibSessions --collection staticContent --file /path/to/jsonfile/newConfig.json --jsonArray

```

StaticContent will be loaded into context every time a new Authentication flow is started, so if configuration is changed
in DB, the next flow will use this one (there's no need to restart IdP)


