## Uris

### Idp Initiated
- https://<idp.hostname>:<idp.port>/idp/profile/SAML2/Unsolicited/SSO?providerId=<url-encoded spEntityId>
- https://idp.local/idp/profile/SAML2/Unsolicited/SSO?providerId=https%3A%2F%2Fsp2.local%3A8444%2Fshibboleth-sp

### Admin resolver test endpoint
- https://<IDP_HOST>/idp/profile/admin/resolvertest?principal=<USER>&requester=<SP_ENTITY_ID>&saml2

NOTE: set Basic Authentication Header:
- Local env: Authorization: Basic YWRtaW46MTIzNA==
- Deployed env: compose base64 user:pass, using credentials found in Vault  