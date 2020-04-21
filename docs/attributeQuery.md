Estado del profile AttributeQuery
=================================

### Entorno de pruebas

El entorno de pruebas para este profile consiste en el Sp shibboleth junto con el plugin **shibsp-plugin-AttributeQuery-Handler** que permite 
lanzar este profile desde el SP y mostrar los atributos devueltos en formato JSON.

El plugin se encuentra integrado en la versión 1.2.1 de la imagen docker de shibboleth-sp.  No obstante, toda la información disponible
sobre el mismo se encuentra en el proyecto del plugin [https://bitbucket.org/PEOFIAMP/shibsp-plugin-attributequery-handler/src/eeeb820ad937?at=default](https://bitbucket.org/PEOFIAMP/shibsp-plugin-attributequery-handler/src/eeeb820ad937?at=default)

#### Problemática de los certificados https.

Cuando el idp utiliza sus propios certificados para https en lugar de utilizar alguno de los generados para la firma o encriptación de mensajes del
protocolo SAML, el profile AttributeQuery lanzado desde el SP falla mostrando el siguiente error en el fichero del sp */var/log/shibboleth/shibd.log* de
la máquina del SP

```
2016-12-22 12:00:32 ERROR XMLTooling.SOAPTransport.CURL [4]: supplied TrustEngine failed to validate SSL/TLS server certificate
2016-12-22 12:00:32 ERROR XMLTooling.SOAPTransport.CURL [4]: Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 15354675671107235346 (0xd516c42b99adee12)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=ES, ST=Madrid, L=Madrid, O=MYORG, OU=MYUNIT, CN=MYCN
        Validity
            Not Before: Dec 22 11:22:12 2016 GMT
            Not After : Dec 20 11:22:12 2026 GMT
        Subject: C=ES, ST=Madrid, L=Madrid, O=MYORG, OU=MYUNIT, CN=MYCN
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (1024 bit)
                Modulus:
                    00:df:aa:e3:51:00:37:e3:39:7d:ce:fc:cc:a4:ed:
                    0c:a2:ef:8f:58:04:8f:27:80:84:ae:fa:c0:59:15:
                    7a:5a:7a:26:ef:bf:f4:99:b7:b1:e9:c7:10:01:53:
                    78:1d:08:83:f6:bb:51:7e:72:ca:a2:28:72:1f:e0:
                    57:b5:cc:06:e3:3b:e3:a7:d2:1d:a3:68:d4:d4:c8:
                    e0:6a:80:f5:f3:3f:13:3d:2a:47:6e:e6:10:79:3b:
                    60:18:5f:ff:8b:50:c7:a2:7a:b7:55:95:cf:ba:c2:
                    e9:3d:f3:71:96:85:8b:07:30:08:fd:75:a1:17:32:
                    96:dd:fb:4d:19:8e:46:a7:8f
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha256WithRSAEncryption
         4b:f8:cc:a8:4a:44:59:02:ea:49:62:2c:4f:0f:37:09:7a:b0:
         fa:84:c2:7d:ce:01:c0:ff:a3:46:e4:74:af:ff:65:75:1f:2d:
         be:6f:91:19:0b:c7:e2:68:4c:0d:53:ce:23:b1:f9:94:d9:62:
         02:7f:00:2c:b0:9c:01:53:72:f5:a3:96:d9:c2:b2:bc:5d:62:
         51:34:a5:53:67:f6:9c:ff:1e:55:0d:cf:19:4a:e7:a5:f5:2b:
         2d:20:f7:a7:0c:c1:33:7f:e4:01:fc:d2:ea:22:c9:80:f5:c5:
         6d:f2:f7:b8:0d:45:05:bf:c5:35:3c:0a:b0:07:4e:5c:15:9c:
         c8:1f

2016-12-22 12:00:32 ERROR Shibboleth.AttributeResolver.Query [4]: exception during SAML query to https://idp.local:443/idp/profile/SAML2/SOAP/AttributeQuery: CURLSOAPTransport failed while contacting SOAP endpoint (https://idp.local:443/idp/profile/SAML2/SOAP/AttributeQuery): SSL certificate problem: application verification failure
2016-12-22 12:00:32 ERROR Shibboleth.AttributeResolver.Query [4]: unable to obtain a SAML response from attribute authority
```

Según afirma Scott Cantor en este foro [
http://shibboleth.1660669.n2.nabble.com/Some-problems-about-the-certificate-td7495134.html](
http://shibboleth.1660669.n2.nabble.com/Some-problems-about-the-certificate-td7495134.html)

El certificado de  https utilizado por el Idp debe incluirse en el idp-metadata.xml como un certificado de firma más. 
Hay que añadirlo junto con los certificados dentro del elemento `<AttributeAuthorityDescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol urn:oasis:names:tc:SAML:1.1:protocol">`
del idp metadata.

Además de esto, el certificado debe contener en el subject un CN que se corresponda con el host del servicio. Por ejemplo, para los nauthilus que levantamos en local con docker
el Subject podría ser: `/C=ES/ST=Madrid/L=Madrid/O=BBVA/OU=BBVASecurity/CN=idp.local`

### AttributeQuery con NameId del tipo 'urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress'

La request de AttributeQuery se base en el NameId para el principal del que se quiere recibir la información. Cuando en NameId tiene el formatType de tipo `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`
en el log del Idp se muestra lo siguiente:

```
2016-12-21 12:33:35,967 - INFO [org.opensaml.saml.common.binding.security.impl.SAMLProtocolMessageXMLSignatureSecurityHandler:134][nAUTHilus-1.0.3-SNAPSHOT,IDP-1.0.3-SNAPSHOT] [id=620be396-68c1-4364-ad03-6c221116ce82]- Message Handler:  Validation of protocol message signature succeeded, message type: {urn:oasis:names:tc:SAML:2.0:protocol}AttributeQuery
2016-12-21 12:33:35,994 - ERROR [net.shibboleth.idp.authn.impl.SelectSubjectCanonicalizationFlow:78][nAUTHilus-1.0.3-SNAPSHOT,IDP-1.0.3-SNAPSHOT] [id=620be396-68c1-4364-ad03-6c221116ce82]- Profile Action SelectSubjectCanonicalizationFlow: No potential flows left to choose from, canonicalization will fail
2016-12-21 12:33:35,997 - WARN [org.opensaml.profile.action.impl.LogEvent:76][nAUTHilus-1.0.3-SNAPSHOT,IDP-1.0.3-SNAPSHOT] [id=620be396-68c1-4364-ad03-6c221116ce82]- An error event occurred while processing the request: SubjectCanonicalizationError
```

Para que el proceso de AttributeQuery pueda recuperar atributos, debe transformar el NameId en un principal. En este caso no tiene forma de encontrarlo, por eso muestra este error.
Según la siguiente [página](https://wiki.shibboleth.net/confluence/display/IDP30/SAML2AttributeQueryConfiguration), sólo la transformación para los NameId de tipo Transient está implementada. 
Por lo que habría que desarrollar el componente/proceso que permite resolver el caso de este formato.

He encontrado algo de información para extender el proceso en esta página [https://wiki.shibboleth.net/confluence/display/IDP30/SubjectCanonicalization](https://wiki.shibboleth.net/confluence/display/IDP30/SubjectCanonicalization)


### Interacción del UserInfoDataConector con AttributeQuery

En el momento de interactuar el flujo de AttributeQuery con el conector que resuelve los atributos, el log del Idp muestra el siguiente error:
```
2016-12-21 12:19:30,145 - INFO [com.igrupobbva.dyd.sl.nauthilus.shibboleth.attribute.resolver.userinfo.UserInfoDataConnector:61][nAUTHilus-1.0.3-SNAPSHOT,IDP-1.0.3-SNAPSHOT] [id=cebae2ed-2316-43ef-baf1-e28e39a248e1]- Requested for attribute: spOpenedSessions
2016-12-21 12:19:30,171 - ERROR [net.shibboleth.idp.saml.profile:-2][nAUTHilus-1.0.3-SNAPSHOT,IDP-1.0.3-SNAPSHOT] [id=cebae2ed-2316-43ef-baf1-e28e39a248e1]- Uncaught runtime exception
java.lang.NullPointerException: null
	at com.igrupobbva.dyd.sl.nauthilus.shibboleth.attribute.resolver.userinfo.UserInfoDataConnector.doDataConnectorResolve(UserInfoDataConnector.java:68)
2016-12-21 12:19:30,177 - WARN [org.opensaml.profile.action.impl.LogEvent:76][nAUTHilus-1.0.3-SNAPSHOT,IDP-1.0.3-SNAPSHOT] [id=cebae2ed-2316-43ef-baf1-e28e39a248e1]- An error event occurred while processing the request: RuntimeException
```

El error se debe a que UserInfoDataConector expera que el profile context contenga un subcontexto del tipo 'AuthenticationContext'. Esto ocurre así en el caso del flujo de autenticación
pero no cuando se le llama desde el flujo de AttributeQuery.
Este contexto es necesario para obtener el uid del usuario y los factores utilizados en la autenticación.
El profileContext tampoco tiene un subcontexto de tipo 'SessionContext' en el que se almacena la lista de SP's en los que se ha logado el usuario.
 