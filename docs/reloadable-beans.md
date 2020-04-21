Reloadable Spring Beans
=

*Summary:*

IdP Bean Refresh Service searches for instances of `net.shibboleth.utilities.java.support.service.AbstractReloadableService` beans 
(subclasses) and invokes their `reload()` method -> which internally calls abstract `doReload()` (that in fact is
the method we want to implement to do the refresh job)
> NOTE: You can see the real thing in `net.shibboleth.idp.spring.services.RefreshService.refreshPropertiesAndContexts()` 

In order to implement refresh for one of your beans, there are two ways to achieve this:
- First one: Make your bean directly inherit `net.shibboleth.utilities.java.support.service.AbstractReloadableService` and implement
abstract method `doReload()`. There you can code the reloading work (setting all the attributes again from config repo vars, for example)
> NOTE: In order to reach the environment vars that you need from Spring, you must "spring autowire" `org.springframework.context.ConfigurableApplicationContext`
> in your class 
- Or, if you cannot inherit `net.shibboleth.utilities.java.support.service.AbstractReloadableService` (maybe your bean already inherits another class, 
or it is a native Java type like Map), you can use a class that we created for this purpose: `com.igrupobbva.dyd.sl.nauthilus.view.NauthilusComponentReloadService`
For this to work, follow these steps:
    - Inject your bean into this service, in `global.xml`. Create the class attribute and his getter-setter
    - Make the changes you need for your bean, coding them into `doReload()`
    - To get environment vars values, use `getConfigPropertyValue(String key, String defaultValue)` method. This 
    class already has Spring env context injected.
> NOTE: For this to be tested in your local environment, you can use the IdP "/refresh" endpoint: `https://idp.local/idp/api/refresh`  

