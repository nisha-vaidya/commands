## How to create types, entities and delete in ATLAS using REST API v2

 - [x] 1. **Creating a type**
 - [x] 2. **Creating an entity**
 - [x] 3. **Removing the entity**
 - [x] 4. **Removing the type**
 - [x] 5. **Remove term**
  - [x] 6. **Script**
 
 **Json formatter** : https://jsonformatter.org/

*wget https://raw.githubusercontent.com/bhagadepravin/commands/master/atlas/atlas-custom-types-v2/type.json*
 
### 1. Create type
```json
curl -u admin:Welcome@12345 -ik -H 'Content-Type: application/json' -X POST 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/types/typedefs' -d @type.json


{"enumDefs":[],"structDefs":[],"classificationDefs":[],"entityDefs":[{"category":"ENTITY","guid":"5a51fe50-33f9-4739-86e3-add9f4d68a60","createdBy":"admin","updatedBy":"admin","createTime":1581157776362,"updateTime":1581157776362,"version":1,"name":"SomeTestEntity","description":"This is a test entity","typeVersion":"1.0","attributeDefs":[{"name":"TestEntity_1","typeName":"string","isOptional":true,"cardinality":"SINGLE","valuesMinCount":0,"valuesMaxCount":1,"isUnique":false,"isIndexable":false,"includeInNotification":false,"searchWeight":-1},{"name":"TestEntity_2","typeName":"string","isOptional":true,"cardinality":"SINGLE","valuesMinCount":0,"valuesMaxCount":1,"isUnique":false,"isIndexable":false,"includeInNotification":false,"searchWeight":-1}],"superTypes":["DataSet"],"subTypes":[],"relationshipAttributeDefs":[{"name":"schema","typeName":"array<avro_schema>","isOptional":true,"cardinality":"SET","valuesMinCount":-1,"valuesMaxCount":-1,"isUnique":false,"isIndexable":false,"includeInNotification":false,"searchWeight":-1,"relationshipTypeName":"avro_schema_associatedEntities","isLegacyAttribute":false},{"name":"inputToProcesses","typeName":"array<Process>","isOptional":true,"cardinality":"SET","valuesMinCount":-1,"valuesMaxCount":-1,"isUnique":false,"isIndexable":false,"includeInNotification":false,"searchWeight":-1,"relationshipTypeName":"dataset_process_inputs","isLegacyAttribute":false},{"name":"meanings","typeName":"array<AtlasGlossaryTerm>","isOptional":true,"cardinality":"SET","valuesMinCount":-1,"valuesMaxCount":-1,"isUnique":false,"isIndexable":false,"includeInNotification":false,"searchWeight":-1,"relationshipTypeName":"AtlasGlossarySemanticAssignment","isLegacyAttribute":false},{"name":"outputFromProcesses","typeName":"array<Process>","isOptional":true,"cardinality":"SET","valuesMinCount":-1,"valuesMaxCount":-1,"isUnique":false,"isIndexable":false,"includeInNotification":false,"searchWeight":-1,"relationshipTypeName":"process_dataset_outputs","isLegacyAttribute":false}]}],"relationshipDefs":[]}
```

### 2. Create Entity:

*wget https://raw.githubusercontent.com/bhagadepravin/commands/master/atlas/atlas-custom-types-v2/entity.json*

```json
curl -u admin:Welcome@12345 -ik -H 'Content-Type: application/json' -X POST 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/entity' -d @entity.json

{"mutatedEntities":{"CREATE":[{"typeName":"SomeTestEntity","attributes":{"qualifiedName":"MyEntityName@c2175"},"guid":"ea493b2f-8218-4263-b790-a5f0f6b739c3"}]},"guidAssignments":{"-1":"ea493b2f-8218-4263-b790-a5f0f6b739c3"}}
````

![sometestentity](https://github.com/bhagadepravin/commands/blob/master/atlas/atlas-custom-types-v2/sometestentity.png)


![sometestentity-1](https://github.com/bhagadepravin/commands/blob/master/atlas/atlas-custom-types-v2/sometestentity-1.png)


### Search Types 

```json
 cat search.json
{
  "excludeDeletedEntities": true,
  "includeSubClassifications": true,
  "includeSubTypes": true,
  "includeClassificationAttributes": true,
  "entityFilters": null,
  "tagFilters": null,
  "attributes": [],
  "limit": 25,
  "offset": 0,
  "typeName": "SomeTestEntity",
  "classification": null,
  "termName": null
}
```

#### Search by text for entity from types: Where entity is MyEntityName

```json
{
  "excludeDeletedEntities": true,
  "includeSubClassifications": true,
  "includeSubTypes": true,
  "includeClassificationAttributes": true,
  "entityFilters": null,
  "tagFilters": null,
  "attributes": [],
  "query": "MyEntityName",
  "limit": 25,
  "offset": 0,
  "typeName": "SomeTestEntity",
  "classification": null,
  "termName": null
}
```

```json
curl -iks  -u admin:Welcome@12345 -ik -H 'Content-Type: application/json' -X POST 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/search/basic' -d  @search.json


{
  "queryType": "BASIC",
  "searchParameters": {
    "typeName": "SomeTestEntity",
    "excludeDeletedEntities": true,
    "includeClassificationAttributes": true,
    "includeSubTypes": true,
    "includeSubClassifications": true,
    "limit": 25,
    "offset": 0,
    "attributes": []
  },
  "entities": [
    {
      "typeName": "SomeTestEntity",
      "attributes": {
        "owner": "admin",
        "qualifiedName": "MyEntityName@c2175",
        "name": "MyEntityName",
        "description": "This is a description"
      },
      "guid": "ea493b2f-8218-4263-b790-a5f0f6b739c3",
      "status": "ACTIVE",
      "displayText": "MyEntityName",
      "classificationNames": [],
      "classifications": [],
      "meaningNames": [],
      "meanings": []
    }
  ]
}
```


###### Search by GUID

```json
curl -u admin:Welcome@12345  -ik -H 'Content-Type: application/json' -X GET 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/entity/guid/ea493b2f-8218-4263-b790-a5f0f6b739c3'

{"referredEntities":{},"entity":{"typeName":"SomeTestEntity","attributes":{"owner":"admin","replicatedTo":null,"TestEntity_1":"attr1","replicatedFrom":null,"qualifiedName":"MyEntityName@c2175","name":"MyEntityName","description":"This is a description","TestEntity_2":"attr2"},"guid":"ea493b2f-8218-4263-b790-a5f0f6b739c3","status":"ACTIVE","createdBy":"admin","updatedBy":"admin","createTime":1581158664906,"updateTime":1581158664906,"version":0,"relationshipAttributes":{"schema":[],"inputToProcesses":[],"meanings":[],"outputFromProcesses":[]}}}[root@c174-node3 ~]#
```

### 3. Removing the entity

```json
curl -u admin:Welcome@12345  -ik -H 'Content-Type: application/json' -X DELETE 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/entity/guid/ea493b2f-8218-4263-b790-a5f0f6b739c3'

{"mutatedEntities":{"DELETE":[{"typeName":"SomeTestEntity","attributes":{"owner":"admin","qualifiedName":"MyEntityName@c2175","name":"MyEntityName","description":"This is a description"},"guid":"ea493b2f-8218-4263-b790-a5f0f6b739c3","status":"ACTIVE","displayText":"MyEntityName","classificationNames":[],"meaningNames":[],"meanings":[]}]}}
```

![deleteentity-1](https://github.com/bhagadepravin/commands/blob/master/atlas/atlas-custom-types-v2/delete%20entity.png)


### 4. Remove type:
```java
curl -u admin:Welcome@12345 -ik -H 'Content-Type: application/json' -X GET 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/types/typedefs' 

curl -u admin:Welcome@12345 -ik -H 'Content-Type: application/json' -X DELETE 'http://c174-node3.squadron.support.hortonworks.com:21000/api/atlas/v2/types/typedefs' -d @type.json
```
!!!!IMPORTANT!!!!

If at this step, you encounter an error; `ATLAS-409-00-002`

This is because the entity was SOFT deleted, make sure atlas is set for hard deletions before you create any type, by ensuring the application properties in ambari have the following;

```
??? atlas.DeleteHandler.impl=org.apache.atlas.repository.graph.HardDeleteHandler
??? atlas.DeleteHandlerV1.impl=org.apache.atlas.repository.store.graph.v1.HardDeleteHandlerV1
```

The GUID is randomly generated by the previous step, so you???ll have to adjust accordingly.

Please also note that deleting Atlas is not something which is recommended as Atlas is used for the governance purpose. 

So, by default soft delete is enabled so that actual data is never deleted if any user by mistake deletes's something from Atlas. Plus the only way to delete is to completely clear the Atlas storage ( Hbase tables).


### 5. Remove term

Get guid from -> Atlas UI -> advanced search -> search for specific entity assign to term which you want to term, open develeper tool, you will see guid.
```json
{ 
   "queryType":"DSL",
   "queryText":"`hive_table` name = cds",
   "entities":[ 
      { 
         "typeName":"hive_table",
         "attributes":{ 
            "owner":"hive",
            "createTime":1581388788000,
            "qualifiedName":"sys.cds@c186",
            "name":"cds"
         },
         "guid":"342440d2-644e-4ec0-8de3-cbcc5ef73252",
         "status":"ACTIVE",
         "displayText":"cds",
         "classificationNames":[ 

         ],
         "classifications":[ 

         ],
         "meaningNames":[ 
            "us"
         ],
         "meanings":[ 
            { 
               "termGuid":"7d25132d-9d2f-4527-abbd-18273539fac6",
               "relationGuid":"f74e03c6-17ea-4ee7-a651-1467fee69b51",
               "displayText":"us",
               "confidence":0
            }
         ]
      }
   ]
}

curl -ik -u admin:hadoop123 -H 'Content-Type: application/json' -H 'Accept:application/json' -d '{"guid":"342440d2-644e-4ec0-8de3-cbcc5ef73252","relationshipGuid":"f74e03c6-17ea-4ee7-a651-1467fee69b51"}' http://c186-node3.coelab.cloudera.com:21000/api/atlas/v2/glossary/terms/7d25132d-9d2f-4527-abbd-18273539fac6/assignedEntities




Ex:
Request URL: http://c186-node3.coelab.cloudera.com:21000/api/atlas/v2/glossary/terms/b901805d-3608-4cf0-a532-39151b2657a4/assignedEntities  ===>  termGuid

[{"guid":"ceb6b9f7-9a32-4138-94a8-f19b0791820f","relationshipGuid":"a0f81e2a-7a4f-4447-a0dc-6aeb346eb5ce"}]  => "guid" "relationGuid":"

```

### 6. Script

```bash
wget https://raw.githubusercontent.com/bhagadepravin/commands/master/atlas/atlas-custom-types-v2/type.json
wget https://raw.githubusercontent.com/bhagadepravin/commands/master/atlas/atlas-custom-types-v2/entity.json

curl -u admin:admin  -ik -H 'Content-Type: application/json' -X POST 'http://c374-node4.supportlab.cloudera.com:21000/api/atlas/v2/types/typedefs' -d @type.json
curl -u admin:admin -ik -H 'Content-Type: application/json' -X POST 'http://c374-node4.supportlab.cloudera.com:21000/api/atlas/v2/entity' -d @entity.json





for i in {10..700000}; do \
  curl -s -u admin:admin -ik -H 'Content-Type: application/json' -X POST -d \
    '{"entity":{"typeName":"SomeTestEntity","guid":"-1","attributes":{"outputs":[],"owner":"m039629","bdpObjectInfo":"{\"inputs\": [{\"name\": \"afw300_multi_record_file\", \"region\": \"operation\", \"partition\": \"day\", \"application\": \"afw300\", \"multiRecordType\": \"true\", \"location\": \"sor\"}], \"outputs\": [{\"name\": \"afw300_multi_record_dataset_r\", \"region\": \"operation\", \"partition\": \"day\", \"application\": \"afw300\", \"multiRecordType\": \"R\", \"location\": \"raw\"}, {\"name\": \"afw300_multi_record_dataset_f\", \"region\": \"operation\", \"partition\": \"day\", \"application\": \"afw300\", \"multiRecordType\": \"F\", \"location\": \"raw\"}, {\"name\": \"afw300_multi_record_dataset_c\", \"region\": \"operation\", \"partition\": \"day\", \"application\": \"afw300\", \"multiRecordType\": \"C\", \"location\": \"raw\"}, {\"name\": \"afw300_multi_record_dataset_d\", \"region\": \"operation\", \"partition\": \"day\", \"application\": \"afw300\", \"multiRecordType\": \"D\", \"location\": \"raw\"}, {\"name\": \"afw300_multi_record_dataset_p\", \"region\": \"operation\", \"partition\": \"day\", \"application\": \"afw300\", \"multiRecordType\": \"P\", \"location\": \"raw\"}]}","inputs":[],"qualifiedName":"afw300_multi_record_file_process_'$i'@operation@b01_bdp_Process2","description":"multi record file ingestion test process","loadRegion":null,"userName":"m039629","transmissionInfo":"{\"frequency\": \"daily\", \"mode\": \"batch\", \"intermediary\": \"\"}","actionInfo":"{\"action\": \"multi_record\", \"multiRecord\": {\"regex\": null, \"position\": {\"start\": 0, \"width\": 1}, \"method\": \"position\"}}","name":"afw300_multi_record_file_process_'$i'","dbConnectionInfo":"","sorObjectInfo":"{\"targetType\": \"multi-record\", \"fileStore\": {\"dataFileNameRegex\": \"afw300_multi_record_file_\\\\d{4}_\\\\d{2}\\\\.csv\", \"extractTimestampRegex\": \"\\\\d{4}_\\\\d{2}\", \"extractTimestampFormat\": \"%Y_%m\", \"ctlFileNameRegex\": \"\"}, \"rdbms\": {\"dataTableName\": \"\", \"ctlTableName\": \"\", \"ctlSchemaName\": \"\", \"dataSchemaName\": \"\"}}"}}}' "http://c374-node4.supportlab.cloudera.com:21000/api/atlas/v2/entity"; \
done 
```

ex:
```
$ curl -X GET -u admin:admin --header 'Accept: application/json;charset=UTF-8' 'http://c374-node4.supportlab.cloudera.com:21000/api/atlas/v2/search/basic?classification=atos'

$ curl -ik -X GET -u admin:admin -H "Content-Type: application/json" -H"Cache-Control: no-cache" "http://c374-node4.supportlab.cloudera.com:21000/api/atlas/admin/metrics"

```
