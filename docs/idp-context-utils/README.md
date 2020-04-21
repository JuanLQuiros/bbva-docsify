idp-context-utils module
TODO: completar
==========================
> SAML Request Extensions extractor.

Nauthilus requirements specify some information received from Armadillo that could be used while authentication process. That information
must be sent to LUX-IAM.

Armadillo information will be received in the AuthnRequest message as SAML extensions where a XML document will be included. 

## Collected information

Next information is collected from extensions.

### resource
Protected resource accessed by the client application.

### action
HTTP Verb used by the application to access to the resource.

### clientId
Id of client application that access to the protected resource.

### transactionId
Transaction Id assigned by armadillo to the client access.

## Extensions format
Next document schema show the xml information document structure.


```
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema" 
           xmlns:tns="http://dyd.igrupobbva.com/AccessInfo.xsd" 
           targetNamespace="http://dyd.igrupobbva.com/AccessInfo.xsd" 
           elementFormDefault="qualified">
 <xsd:element name="AccessInfo" type="tns:AccessInfoType"/>
 <xsd:complexType name="AccessInfoType">
  <xsd:sequence>
   <xsd:element name="resource" type="xsd:string" minOccurs="1" maxOccurs="1"/>
   <xsd:element name="action"   minOccurs="1" maxOccurs="1">
	  <xsd:simpleType>    
	      <xsd:restriction base="xsd:string">  
		<xsd:enumeration value="GET"/> 
		<xsd:enumeration value="POST"/> 
		<xsd:enumeration value="PUT"/>
		<xsd:enumeration value="PATCH"/> 
		<xsd:enumeration value="DELETE"/> 
	      </xsd:restriction>  
	  </xsd:simpleType>
   </xsd:element> 
   <xsd:element name="clientId" type="xsd:string" minOccurs="1" maxOccurs="1"/>
   <xsd:element name="transactionId" type="xsd:string" minOccurs="1" maxOccurs="1"/>
  </xsd:sequence>
 </xsd:complexType>
</xsd:schema>
```

Next document shows an extensions example:

```
<md:Extensions xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata">
    <attr:AccessInfo xmlns:attr="http://dyd.igrupobbva.com/AccessInfo.xsd">
        <attr:clientId>armadillo</attr:clientId>
        <attr:transactionId>5c598ebce4fe55908e86f584073476aa2dfdebdd4346965fd836850cb6c595e7</attr:transactionId>
        <attr:resource>/token/authorize</attr:resource>
        <attr:action>GET</attr:action>
    </attr:AccessInfo>
</md:Extensions>
```
