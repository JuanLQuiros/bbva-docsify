This project implements a MongoDB provider for Shibboleth. This enables the IdP to store sessions and cookies in a MongoDB.

The collection name is StorageRecords and stores MongoStorageRecord objects.

Database configuration is in application.yml file.

MongoStorageService implements AbstractStorageService methods that includes read and write methods. 
Database access is configured using these properties:

* idp.storageservice.mongo.uri: full mongodb uri 
