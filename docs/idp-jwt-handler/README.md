# Introduction

This module is designed to provide utilities in order to generate JWT Signed and Encrypted tokens.
The encryption and signing algorithms are based upon RSA.

## Configuration

You can include **NauthilusJwtConfiguration** class into your Spring 4 annotation-based configuration
 
```
  
  @Import(NauthilusJwtConfiguration.class)
  public class MyOtherConfigurationBean...
  
``` 
 
 or in an older fashion, you should define the following bean creation:
 
```
 
     <bean id="jwtConfiguration" class="com.igrupobbva.dyd.sl.nauthilus.jwt.conf.JwtCfg">
         <property name="expirationInMinutes" value="%{jwt.expirationInMinutes}"/>
         <property name="issuer" value="%{jwt.issuer}"/>
         <property name="keystoreAlias" value="%{jwt.keystoreAlias}"/>
         <property name="keystorePassword" value="%{jwt.keystorePassword}"/>
         <property name="keystoreResource" value="%{jwt.keystoreResource}"/>
     </bean>
     
     <bean id="jwtHandler" class="com.igrupobbva.dyd.sl.nauthilus.jwt.impl.JwtRsaOperationsImpl">
         <constructor-arg ref="jwtConfiguration"/>
     </bean>

```     

In any case you will need the following configuration parameters:

* _expirationInMinutes_: expiration time of the token expressed in minutes
* _issuer_: issuer of the token
* _keystoreAlias_: alias of the certificate inside the keystore
* _keystorePassword_: password for accessing the keystore data
* _keystoreResource_: classpath or filesystempath of the keystore file.

## Creation of jwt token

Once you have a jwtRsaOperations bean injected, you can create a jwt token based on the information
 contained in any bean this way:
 
```

    UserInfo userInfo = new UserInfo("icanon", "3444444", "test@eso.es", 40);
    final String jwt = jwtRsaOperations.buildJwtToken("test", new TokenInfo<>(userInfo));

```

## Validation of jwt token

You can check if the jwt token you received by other terms is valid, checking that it's signed with the keys you provide.
 This can be done this way
 
```

boolean validJwt = jwtRsaOperations.checkJwtSigned(jwt);

```

## Retrieve the encrypted private data
 
If you want to extract the private data field from the jwt token, you can do it this way
 
```

UserInfo userInfo = new UserInfo("icanon", "3444444", "test@eso.es", 40);
final String jwt = jwtRsaOperations.buildJwtToken("test", new TokenInfo<>(userInfo));

UserInfo resultUserInfo = jwtRsaOperations.<UserInfo>getTokenPrivatePayload(jwt).getPrivateData();

assertEquals("icanon", resultUserInfo.getUserId());
assertEquals("3444444", resultUserInfo.getMobile());
assertEquals("test@eso.es", resultUserInfo.getEmail());
assertEquals(40, resultUserInfo.getAge()); 
 
```

## Changelog

* v1.0.0.RELEASE: first version with no encryption
* v1.0.1.RELEASE: version with signing and encryption of jwt
* v1.0.2.RELEASE: fixed bugs and problems with naming
* v1.0.3.RELEASE: updated version of libraries 
* v1.0.4.RELEASE: updated readme