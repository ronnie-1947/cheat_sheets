MongoDB
============

## Table of Contents

- [MongoDB Server Initialization](#MongoDB-Server)
- [Database & Collection Info](#Database-&-Collection-Info)
- [Create](#Create)
- [Read](#Read)
- [Update](#Update)
- [Delete](#Delete)
- [Aggregates](#Aggregates)
- [Defining a Collection](#Defining-a-Collection)

## MongoDB Server

| Command | Description |
| ------- | ----------- |
| `net stop mongodb` |Stop mongodb server|
| `mongod` |Start mongodb server in default port '27017'|
| `mongod --port xxxxx ` |Start mongodb server in port xxxxx|
| `mongo` |connect to the Server if port is set to default|
| `mongo --port xxxxx` |connect to the Server if port is set to xxxxx|
______________________________________

## Database & Collection Info

| Command | Description |
| ------- | ----------- |
| `show dbs` |Show all the databases present in Server|
| `use <database_name>` |Use the respective database OR create new database with respective name|
| `db.stats()` |Show information on database|
| `db.getName()` |Show active database|
| `db.dropDatabase()` |Drop current dataBase|
| `show collections` |Show all the collections present in Database|
| `db.<collectionName>.drop()` |Delete the collection|


Notes: The database will be created in the fly if no such database exist

______________________________________

## Create

| Command | Description |
| ------- | ----------- |
| `db.<collection_name>.insertOne({key:value})` |Create a new collection and Insert a doc into it|
| `db.<collection_name>.insertMany([ {}, {}, {} ])` |Create a new collection and Insert many docs into it|
| `db.<collection_name>.insertMany([ {key: {key: value}}, {}])` |Create a new collection and Insert nested docs|


______________________________________

## Read

| Command | Description |
| ------- | ----------- |
| `db.<collection_name>.findOne()` |Show first doc in the collection & pretty doesnot work with findOne|
| `db.<collection_name>.find().pretty` |Show first 20 docs in the collection|
| `db.<collection_name>.find().forEach(doc =>{printjson(doc)})` |forEach method can be applied after find() to print all available docs|
| `db.<collection_name>.find().toArray()` |Show all docs present in the collection wrapped in an array|
| `db.<collection_name>.find({...}).pretty` |Show particular doc present in the collection|
| `db.<collection_name>.find({key:{$gt: num}})` |Show all docs greater than num|
| `db.<collection_name>.find({}, {name:1, _id:0})` |Show specific fields that are specified in the 2nd object|
| `db.<collection_name>.find({"key.key1": "value"})` |nested filters|

______________________________________

## Update
| Command | Description |
| ------- | ----------- |
| `db.<collection_name>.updateOne({...}, {$set: {key: "new_data"}})` |Update the value of the respective key|
| `db.<collection_name>.updateMany({...}, {$set: {key: "new_data"}})` |Update many values which follows same condition|
| `db.<collection_name>.updateMany({}, {$set:{"key.subKey.subsubKey": "value"}})` |update nested values , won't work without the ""  in the key.subkey.subsubkey|
| `db.<collection_name>.updateMany({...}, {$set:{key: "value"}}, {upsert: true})` |if filter not found than create new doc by upserting|
| `db.<collection_name>.updateMany({...}, {$unSet:{key: 1}})` |remove specific fields|
| `db.<collection_name>.updateMany({...}, {$inc:{key: num}})` |increment the value by num|
| `db.<collection_name>.replaceOne({...}, {key: "value"})` |replace a document by a new document|

### Field Update Operators
| Name | Description |
| ------- | ----------- |
| `$currentDate` |Sets the value of a field to current date, either as a Date or a Timestamp|
| `$inc` |Increments the value of the field by the specified amount|
| `$min` |Only updates the field if the specified value is less than the existing field value|
| `$max` |Only updates the field if the specified value is greater than the existing field value|
| `$mul` |Multiplies the value of the field by the specified amount|
| `$rename` |Renames a field|
| `$set` |Sets the value of a field in a document|
| `$setOnInsert` |Sets the value of a field if an update results in an insert of a document. Has no effect on update operations that modify existing documents|
| `$unset` |Removes the specified field from a document.|
______________________________________

## Delete
| Command | Description |
| ------- | ----------- |
| `db.<collection_name>.deleteOne({key: val})` |Delete one selected doc from the collection|
| `db.<collection_name>.deleteMany({})` |Delete every item from collection|

______________________________________

## Aggregates

### lookup
lookup is used to merge tables. The local Field should be an array
```
 db.<local_table-name>.aggregate(
     [
         {
             $lookup: {
                from: "foreign_table-name", 
                localField:"local_field" , 
                foreignField:"foreign_field", 
                as: "Alias-name"
            }
        }
    ]
).pretty()
```
______________________________________

## Defining a Collection

```
db.createCollection('collection_name', {
    validator: {
        $jsonSchema: {
            bsonType: 'object',
            required: ['title', 'types'],
            properties: {
                title : {
                    bsonType: 'string',
                    description: 'must be a string and is required',
                },
                types: {
                    bsonType: 'array',
                    description: 'must be an array',
                    items: {
                        bsonType: 'object',
                        required: ['a', 'b'],
                        properties: {
                            a: {
                                bsonType: 'string',
                                description: 'Must be a string'
                            }
                        }
                    }
                }
            }
        }
    }
})
```
Editing a defined collection
```
db.runCommand({
    collMod: 'doors',
    validator: {
        $jsonSchema: {
            bsonType: 'object',
            required: ['title'],
            properties: {
                title : {
                    bsonType: 'string',
                    description: 'must be a string and is required',
                },
                types: {
                    bsonType: 'array',
                    description: 'must be an array',
                    items: {
                        bsonType: 'object',
                        required: ['a', 'b'],
                        properties: {
                            a: {
                                bsonType: 'string',
                                description: 'Must be a string'
                            }
                        }
                    }
                }
            }
        }
    },
    validationAction: 'warn'
})
```