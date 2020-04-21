idp-external-viewresolver
=========================
> Login html pages externalization
 
Nauthilus requirements include login page externalization to improve page maintenance.

This module implements needed changes over Shibboleth code and configuration to store login view html pages in
file system outside to application war file.

Additionally, a login view selection logic is defined and implemented by this module. View selection logic include is defined by next steps:
* If a customized login view exists for clientId (received in saml request extensions) that view will be served.
* If a customized login view exists for Service Provider, that view will be served as second option.
* If there aren't customized login view, nauthilus default login view will be served.

Pages storage structure
-----------------------
All login pages, must be stored in a local filesystem path identified by idp.loginviews property. 

### Login view templates
Each exiting Authentication Controller has its own login page to get from user the needed information, shared secret, etc. for authentication.
Next rules are applied to select the right login view for an authentication controller (AC).

If connected application to a SP (this application is informed by Armadillo's through **cliendId** param and is equivalent to Armadillo's clientId concept) has a customized look&feel for an AC, this login page
will be served to the user.
If connected application doesn't have customized pages but SP does, the SP customized login look&feel will be served to the user.
Finally, if there aren't any customized login look&feel Nauthilus default login page will be served to the user.

To achieve the requirement, customized look&feel is organized in storage filesystem as next diagram shows.

```
\<views root\>
      |__________idpDefault
      |                |________ \<AC Id 1\>
      |                |              |_____ login.html  <----- default login page for Auth. Controler id 1
      |                | 
      |                |        .............     
      |                |_________ \<AC Id n\>
      |                |              |_____ login.html  <----- default login page for Auth. Controler id n
      |
      |
      |
      |__________\<SP or clientId Name 'a'\>
      |                  |________ \<AC Id 1\>
      |                  |              |_____ login.html  <----- SP/clientId 'a' login page for Auth. Controler id 1
      |                  | 
      |                  |        .............     
      |                  |________ \<AC Id n\>
      |                                 |_____ login.html  <----- SP/clientId 'a' default login page for Auth. Controler id n
      |
      |__________\<SP or clientId Name 'n'\>
                         |________ \<AC Id 1\>
                         |              |_____ login.html  <----- SP/clientId 'n' login page for Auth. Controler id 1
                         | 
                         |        .............     
                         |________ \<AC Id n\>
                                        |_____ login.html  <----- SP/clientId 'n' default login page for Auth. Controler id n      
      
```

#### path element formats
This section explain the format of different path elements. Notice that path elements cannot include the *'/'* character to avoid view resolution failures.
##### AC Id (authentication controller id)
This Id is a two numeric digits string. It's the identification code for de AC assigned in the common AC identification table.

##### SP Name
This path element is directly the SP entity Id except if entity Id has a URL format. In that case SP Name will be a string composed by host and port as *'host_port'*. If URL doesn't
include any port, URL host name will be assigned as SP Name.

##### clientId Name
If clientId is used, the path element will be directly the clientId received from Armadillo. 


Static resources storage structure
----------------------------------
Login pages for a SP or CliendId could need static resources (js, css, images, etc.) different from Nauthilus defaults.
Nauthilus will publish the local filesystem path identified by idp.loginviews.static property through the URL 'https://idphost:port/idp/static'

Each login page have to locate its own static resources in the published structure. Next diagram shows how filesystem is organized.
```
\<static root\>   <----- default Nauthilus static resources
      |________ \<css\>
      |________ \<js\>
      |________ \<images\>   
      |        .............     
      |
      |__________\<SP or clientId Name 'a'\>    <----- SP/clientId 'a' static resources
      |                   |________ \<css\>
      |                   |________ \<js\>
      |                   |________ \<images\>   
      |                   |        ............. 
      |
      |__________\<SP or clientId Name 'n'\>    <----- SP/clientId 'n' static resources
      |                   |________ \<css\>
      |                   |________ \<js\>
      |                   |________ \<images\>   
      |                   |        .............      
      
```

SP Name and ClientId has the same sense and format explained before.


Variables received by view from Nauthilus
-----------------------------------------
Nauthilus will inject in html templates 2 variables:

* ErrorKey: Error Id if authentication fails due information introduced by the user.

* locale: Locale configured in server for this page.

To receive it in html template and process it with javascript, include next tags in your page.
```
<textarea id="error_key" style="display:none;">#ErrorKey#</textarea>
<textarea id="locale" style="display:none;">#Locale#</textarea>
```

Shibboleth Integration
----------------------
To integrate this view resolver in shibboleth, next configuration changes must be applied

### configuration properties
To use a git server as view resources server, next properties must be available in Spring context. That properties should be included in idp.properties file.

```
idp.loginviews.git.uri.desc: View resources git server uri
idp.loginviews.git.uri: ssh://git@52.19.152.31:222/identity-provider/nauthilus-viewresources-git.git
idp.loginviews.git.username.desc: View resources git server access user
idp.loginviews.git.username: gmartinez
idp.loginviews.git.privatekey.desc: Private key to access View resources git server
idp.loginviews.git.privatekey: file://%{idp.home}/credentials/git_rsakey
idp.loginviews.git.password.desc: User private key password
idp.loginviews.git.password: Paradigma123
idp.loginviews.git.basedir.desc: Path where view resources are downloaded.
idp.loginviews.git.basedir: /opt/gitresources
idp.loginviews.pages.desc: Path to login view pages
idp.loginviews.pages = %{idp.loginviews.git.basedir}/views/
idp.loginviews.static.desc: Path to login pages static resources
idp.loginviews.static = file:%{idp.loginviews.git.basedir}/static/
```

### login view-state configuration
Dynamic logic viewName for login view states must be used to apply selection logic. The view-state view attribute must be changed with next sentence:
```
view="#{T(com.igrupobbva.dyd.sl.nauthilus.view.ViewNameParamBuilder).getLoginViewLogicName(opensamlProfileRequestContext)}"
```

To modify Shibboleth standard 'Password' flow, this change must be done overriding 'idp/system/flows/authn/password-authn-flow.xml' changing view-state by:
```
<view-state id="DisplayUsernamePasswordPage" view="#{T(com.igrupobbva.dyd.sl.nauthilus.view.ViewNameParamBuilder).getLoginViewLogicName(opensamlProfileRequestContext)}">

    <on-render>
        <evaluate expression="environment" result="viewScope.environment" />
        <evaluate expression="opensamlProfileRequestContext" result="viewScope.profileRequestContext" />
        <evaluate expression="opensamlProfileRequestContext.getSubcontext(T(net.shibboleth.idp.authn.context.AuthenticationContext))" result="viewScope.authenticationContext" />
        <evaluate expression="authenticationContext.getPotentialFlows().values().?[id matches 'authn/(' + (flowRequestContext.getActiveFlow().getApplicationContext().containsBean('shibboleth.authn.Password.ExtendedFlows') ? flowRequestContext.getActiveFlow().getApplicationContext().getBean('shibboleth.authn.Password.ExtendedFlows').trim() : '') + ')']" result="viewScope.extendedAuthenticationFlows" />
        <evaluate expression="flowRequestContext.getActiveFlow().getApplicationContext().containsBean('shibboleth.authn.Password.PrincipalOverride') ? flowRequestContext.getActiveFlow().getApplicationContext().getBean('shibboleth.authn.Password.PrincipalOverride') : null" result="viewScope.passwordPrincipals" />
        <evaluate expression="authenticationContext.getSubcontext(T(net.shibboleth.idp.ui.context.RelyingPartyUIContext))" result="viewScope.rpUIContext" />
        <evaluate expression="authenticationContext.getSubcontext(T(net.shibboleth.idp.authn.context.AuthenticationErrorContext))" result="viewScope.authenticationErrorContext" />
        <evaluate expression="authenticationContext.getSubcontext(T(net.shibboleth.idp.authn.context.AuthenticationWarningContext))" result="viewScope.authenticationWarningContext" />
        <evaluate expression="authenticationContext.getSubcontext(T(net.shibboleth.idp.authn.context.LDAPResponseContext))" result="viewScope.ldapResponseContext" />
        <evaluate expression="T(net.shibboleth.utilities.java.support.codec.HTMLEncoder)" result="viewScope.encoder" />
        <evaluate expression="flowRequestContext.getExternalContext().getNativeRequest()" result="viewScope.request" />
        <evaluate expression="flowRequestContext.getExternalContext().getNativeResponse()" result="viewScope.response" />
        <evaluate expression="flowRequestContext.getActiveFlow().getApplicationContext().containsBean('shibboleth.CustomViewContext') ? flowRequestContext.getActiveFlow().getApplicationContext().getBean('shibboleth.CustomViewContext') : null" result="viewScope.custom" />
    </on-render>

    <transition on="proceed" to="ExtractUsernamePasswordFromFormRequest">
        <evaluate expression="authenticationContext.setAttemptedFlow(thisFlow)" />
    </transition>
    <transition on="#{currentEvent.id.startsWith('authn/')}" to="PreserveAuthenticationFlowState">
        <evaluate expression="authenticationContext.setAttemptedFlow(authenticationContext.getPotentialFlows().get(currentEvent.id))" />
    </transition>

    <on-exit>
        <evaluate expression="opensamlProfileRequestContext.addSubcontext(new net.shibboleth.idp.consent.context.ConsentManagementContext(), true).setRevokeConsent(requestParameters._shib_idp_revokeConsent == 'true')" />
    </on-exit>
</view-state>
```

### Enabling NauthilusViewResolver
The new view resolver bean must be created and ordered with the existing Shibboleth view resolvers. This configuration must be done over 'idp/system/conf/mvc-beans.xml':
 
Include in the view resolver bean definition section:
```
<bean id="nauthilus.NauthilusViewResolver"
      class="com.igrupobbva.dyd.sl.nauthilus.view.NauthilusViewResolver">
    <property name="order" value="2"/>
    <property name="resourcePath" value="#{'%{idp.loginviews:%{idp.home}/views}'.trim()}"/>
    <property name="cache" value="true"/>
    <property name="suffix" value=".html"/>
</bean>
<bean id="jGitSshEnvironmentRepository" class="com.igrupobbva.dyd.sl.nauthilus.view.git.JGitSshEnvironmentRepository">
    <property name="uri" value="%{idp.loginviews.git.uri}"/>
    <property name="basedir" value="%{idp.loginviews.git.basedir}"/>
    <property name="username" value="%{idp.loginviews.git.username}"/>
    <property name="password" value="%{idp.loginviews.git.password}"/>
    <property name="cloneOnStart" value="true"/>
</bean>
```

Reorder the other available view resolvers bean in the same section:
```
<bean id="shibboleth.VelocityViewResolver"
      class="org.springframework.web.servlet.view.velocity.VelocityViewResolver">
    <property name="order" value="3"/>
    <property name="cache" value="true"/>
    <property name="prefix" value=""/>
    <property name="suffix" value=".vm"/>
    <property name="contentType" value="text/html;charset=utf-8"/>
</bean>


<bean id="shibboleth.InternalViewResolver"
      class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="order" value="4"/>
    <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
    <property name="contentType" value="text/html;charset=utf-8"/>
</bean>
```

Finally, modify mvcViewFactoryCreator bean in 'idp/system/conf/webflow-config.xml' file to include the new view resolver
```
<bean id="mvcViewFactoryCreator" class="org.springframework.webflow.mvc.builder.MvcViewFactoryCreator">
    <property name="viewResolvers">
        <list>
            <ref bean="nauthilus.NauthilusViewResolver"/>
            <ref bean="shibboleth.VelocityViewResolver"/>
            <ref bean="shibboleth.InternalViewResolver"/>
        </list>
    </property>
</bean>
```

### Enabling static content publishing
Static content is published by nauthilus too. To get static content form file system next changes must be done.

Add /static mapping to 'WEB-INF/web.xml'
```
<servlet-mapping>
    <servlet-name>idp</servlet-name>
    <url-pattern>/status</url-pattern>
    <url-pattern>/profile/*</url-pattern>
    <url-pattern>/api/*</url-pattern>
    <url-pattern>/static/*</url-pattern>
</servlet-mapping>
```

Add mvc resource to 'idp/conf/mvc-beans.xml'
```
<mvc:resources mapping="/**" location="#{'%{idp.views.static:file:/opt/resources/static/}'.trim()}" />
```
