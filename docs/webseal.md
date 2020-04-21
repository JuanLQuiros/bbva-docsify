SingleSignOn on Webseal
=

### Webseal login flow explained

SSO with Webseal is achieved using a BBVA service called EAI, who is responsible of generating a SAML request for Nauthilus 
and receiving the corresponding SAML response in order to login with Webseal and send login cookie to browser. 
Process is completely transparent for the user, as it is done in background. 

For Webseal SSO process to execute, a regular login with any of the registered SP must be done with Nauthilus. 
Before SAML response is sent back to the SP, Webseal login steps are initiated:
> Please note that this flow has nothing to do with Spring Webflow.
1. First of all, Nauthilus evaluates if webseal SSO is enabled in config (property `nauthilus.websealsso.enabled`. See Configuration properties)
2. If Webseal SSO is enabled, we check if browser has a valid Webseal cookie, and therefore user is already logged. 
This is achieved by checking a EAI endpoint created for this purpose (set by `nauthilus.websealsso.checkAlreadyLoggedResourceSrc` property).
This endpoint returns a JSON with only one field (iv-user). If value is not equal to "Unauthorized", then user is already logged in webseal. 
In that case Webseal SSO flow is bypassed, as it's not necessary to login again (Continue to step 6).
If value is equal to "Unauthorized" (or exception is raised), we continue with next step.
3. Obtain a proper SAML request for Shibboleth ECP, using EAI URL endpoint service (property `nauthilus.websealsso.eaisamlrequesturl`)
4. Perform an ECP login with Nauthilus, using SAML request from previous step (endpoint on property `nauthilus.websealsso.idpecpurl`). 
As user is currently logged (before step 1), Nauthilus does not need user/pass because a valid Nauthilus cookie is sent to ECP.
5. Nauthilus SAML response is gathered and sent to another EAI endpoint specified by config property `nauthilus.websealsso.eaissourl`. 
This endpoint performs the actual login with webseal and generates a valid webseal cookie.
6. SAML response from the original login request is sent back to the SP.

> Actually, another validation is done before all of this steps are performed. We check if SP asking for Nauthilus auth is a Webseal protected SP
> (by evaluating if SP host contains 'community' word). If it's a webseal protected SP, webseal SSO is bypassed, because a webseal cookie
> will be obtained if saml login flow is passed.  

### Configuration properties

Properties are located in `idpSAML-<ENVIRONMENT>.properties` on our config repo. All of them are mandatory unless otherwise specified
* `nauthilus.websealsso.eaisamlrequesturl`: URL for EAI service that composes SAML request or Shibboleth ECP.
* `nauthilus.websealsso.idpecpurl`: URL for Shibboleth ECP login.
* `nauthilus.websealsso.eaissourl`: URL for EAI service that receives ECP SAML response and performs Webseal login / Webseal cookie generation.
* `nauthilus.websealsso.checkAlreadyLoggedResourceSrc`: URL of a Webseal protected resource, needed to check if user is already logged on Webseal (ivUser endpoint).
* `nauthilus.websealsso.checkAlreadyLoggedTimeout`: Timeout to complete webseal sso (see below for considerations).
* `nauthilus.websealsso.enabled`: Type boolean. Not mandatory. Defaults to false. Whether or not Webseal SSO process is enabled. 
* `nauthilus.websealsso.serviceworkermode.enabled`: Type boolean. Not mandatory. Defaults to true. Whether or not Webseal SSO 
is done with Service Workers (if browser supports them). If value is `false` Webseal login is done in a synchronous manner.

> NOTE: Webseal timeout considerations. This timeout acts differently depending of the execution context:
>   - Service Worker mode: Timeout until service worker is invoked
>   - Synchronous mode (non Microsoft Explorer): Timeout until the whole SSO is completed
>   - Internet Explorer: Timeout for the ivUser endpoint to respond.

### Service worker vs synchronous mode

By default, this process is executed within a [Service Worker](https://developers.google.com/web/fundamentals/primers/service-workers) if current browser supports this functionality 
(see this [browser compatibility chart](https://caniuse.com/#feat=serviceworkers)). Service workers are processes that run on a browser in background asynchronous mode, and
thus not affecting to webpage performance.

However, if current browser does not support Service Workers (or if it is disabled by Nauthilus config-repo), 
a conventional synchronous process is executed, and therefore it is completed before navigation goes back to the original SP.

Code is composed of these files. All paths are relative to `nauthilus-shibboleth-idp/idp-webapp-overlay/src/main/webapp/`:
* `idp/views/templates/saml2-post-binding.vm`. Velocity template that composes the HTML responsible of sending SAML
 response to original SP. We override this page to insert Webseal Login process before SP redirection.
* `idp/views/templates/add-html-body-content.vm` and `idp/views/templates/add-html-head-content.vm`. Includes in `saml2-post-binding.vm` page. These templates contain the actual code 
that registers/execute service worker and runs the synchronous process.
* `idp/views/templates/add-html-body-content-ie.vm`. Same as above, but only for IE
* `webseal_sw.js`. Code for the service worker
* `js/webseal_lib.js`. Javascript functions that perform webseal SSO steps (ajax fetches to EAI and ECP endpoints). Shared between service worker and synchronous process.
* `idp/conf/global.xml`. This xml defines a bean with id `shibboleth.CustomViewContext` that reads config properties. This bean is injected into 
Velocity context (named `custom`). Performed by `com.igrupobbva.dyd.sl.nauthilus.view.NauthilusHTTPPostEncoder` class

### Internet Explorer vs Web Browsers

Internet Explorer exists. Our users use it. That's a fact. That's life. So webseal SSO must be compatible with this ~~web browser~~. 
In order to achieve this and at the same time avoiding the obliteration of the functionality for real browsers, an independent velocity template 
that runs only-ie-compatible code has been created. IE (all versions) are not compatible at all with 'service workers', nor with 'fetch' api, that 
is widely used in this development. We check the value of the http request header "User-Agent", in order to determine which browser we are dealing with.
If it's explorer, "add-html-body-content-ie.vm" template is parsed (see previous section).

> NOTE: All IE code is contained in this template. No libs or externals JS files are used for IE.  
  
### Injecting Java objects into Velocity context
A Spring bean is defined with id `shibboleth.CustomViewContext`, that reads config properties. This bean is injected into 
Velocity context (named `custom`), by inheriting Shibboleth class `org.opensaml.saml.saml2.binding.encoding.impl.HTTPPostEncoder`: See class 
`com.igrupobbva.dyd.sl.nauthilus.view.NauthilusHTTPPostEncoder`

### Local testing
In order to test this process in local environment, a docker-compose is supplied that must be executed next to nauthilus docker-compose-local.
This compose creates 2 nginx SSL enabled and CORS configured, that mock both EAI and Nauthilus ECP endpoints. Nauthilus ECP endpoint needs to be mocked because 
we don't have a working EAI that generates proper SAML request for ECP.

Run this compose in a fresh console:
```
# from project root path:
cd docker/webseal-eai-mock

docker-compose up
```
> NOTE: obviously idp local compose must be running in another console

After mocked Webseal login is done with an ok result, a fake webseal cookie named `ws_cookie` is generated.

For this to work, Chrome browser must be launched with some security constrains relaxed, as it checks a valid signed TLS certificate prior to register service worker. 
In order to do so, open it from a terminal with this command:

`nohup google-chrome --user-data-dir=/tmp/foo --ignore-certificate-errors --unsafely-treat-insecure-origin-as-secure=https://idp.local >/dev/null 2>&1 &`

> NOTE1: A bash script (`run-chrome.sh`) is provided to easily open Chrome this way.
>
> NOTE2: This command runs an instance of Chrome in another userspace, so it does not change anything on your configured and beloved browser

_Debugging Service Workers in Chrome_: Console for service workers in chrome is located in `chrome://serviceworker-internals`.

_Debugging Service Workers in Firefox_: Located in `about:debugging`, "Workers" menu 
(see [this link](https://hacks.mozilla.org/2016/03/debugging-service-workers-and-push-with-firefox-devtools/))


### Note on Firefox

Firefox may have Service Workers disabled. In order to re-enable them, follow this steps:
1. Open `about:config` page.
2. Search for `dom.service` string.
3. Set `dom.serviceWorkers.enabled` to true.
