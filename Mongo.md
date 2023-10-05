# MongoDB

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

| Command                | Description                                     |
| ---------------------- | ----------------------------------------------- |
| `net stop mongodb`     | Stop mongodb server                             |
| `mongod`               | Start mongodb server in default port '27017'    |
| `mongod --port xxxxx ` | Start mongodb server in port xxxxx              |
| `mongo`                | connect to the Server if port is set to default |
| `mongo --port xxxxx`   | connect to the Server if port is set to xxxxx   |

---

## Database & Collection Info

| Command                                                       | Description                                                             |
| ------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `show dbs`                                                    | Show all the databases present in Server                                |
| `use <database_name>`                                         | Use the respective database OR create new database with respective name |
| `db.stats()`                                                  | Show information on database                                            |
| `db.getName()`                                                | Show active database                                                    |
| `db.dropDatabase()`                                           | Drop current dataBase                                                   |
| `show collections`                                            | Show all the collections present in Database                            |
| `db.<collectionName>.drop()`                                  | Delete the collection                                                   |
| `db.<collectionName>.explain(executionStats).<some function>` | Show stats about the function                                           |

Notes: The database will be created in the fly if no such database exist

---

## Create

| Command                                                       | Description                                          |
| ------------------------------------------------------------- | ---------------------------------------------------- |
| `db.<collection_name>.insertOne({key:value})`                 | Create a new collection and Insert a doc into it     |
| `db.<collection_name>.insertMany([ {}, {}, {} ])`             | Create a new collection and Insert many docs into it |
| `db.<collection_name>.insertMany([ {key: {key: value}}, {}])` | Create a new collection and Insert nested docs       |

---

## Read

| Command                                                       | Description                                                            |
| ------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `db.<collection_name>.findOne()`                              | Show first doc in the collection & pretty doesnot work with findOne    |
| `db.<collection_name>.find().pretty`                          | Show first 20 docs in the collection                                   |
| `db.<collection_name>.find().forEach(doc =>{printjson(doc)})` | forEach method can be applied after find() to print all available docs |
| `db.<collection_name>.find().toArray()`                       | Show all docs present in the collection wrapped in an array            |
| `db.<collection_name>.find({...}).pretty`                     | Show particular doc present in the collection                          |
| `db.<collection_name>.find({key:{$gt: num}})`                 | Show all docs greater than num                                         |
| `db.<collection_name>.find({}, {name:1, _id:0})`              | Show specific fields that are specified in the 2nd object              |
| `db.<collection_name>.find({"key.key1": "value"})`            | nested filters                                                         |
| `db.<collection_name>.find({array: ['abc']})`                 | filter exact match like the array                                      |

## Filter Operators

| Operator | Command                                         | Description                                    |
| -------- | ----------------------------------------------- | ---------------------------------------------- |
| $in      | `db.<dbName>.find({runtime: {$in: [30, 50]}})`  | Filter doc where runtime is within 30 & 50     |
| $nin     | `db.<dbName>.find({runtime: {$nin: [30, 50]}})` | Filter doc where runtime is not within 30 & 50 |

| Operator | Example   | Description                         |
| -------- | --------- | ----------------------------------- |
| $or      | See Below | All of the condition should satisfy |

```
db.movies.find(
    {
        $or: [
            {
                'rating.average': {$gt: 9.3}
            },
            {
                'rating.average': {$lt: 5}
            }
        ]
    }
).pretty()
```

| Operator | Example   | Description                                          |
| -------- | --------- | ---------------------------------------------------- |
| $nor     | See Below | All of the condition satisfied should not be queried |

```
db.movies.find(
    {
        $nor: [
            {
                'rating.average': {$gt: 9.3}
            },
            {
                'rating.average': {$lt: 5}
            }
        ]
    }
).pretty()
```

| Operator | Example   | Description  |
| -------- | --------- | ------------ |
| $and     | See Below | AND operator |

```
db.movies.find(
    {
        $and: [
            {
                'rating.average': {$gt: 9.3}
            },
            {
                'genre': 'Horror'
            }
        ]
    }
).pretty()
```

| Operator | Example                                     | Description                |
| -------- | ------------------------------------------- | -------------------------- |
| $exists  | `db.<dbName>.find({name: {$exists: true}})` | If the field 'name' exists |

### Querying Arrays

#### Query Objects inside Arrays

```
hobbies: [
            {
                title: 'Swimming',
                frequency: 3
            },
            {
                title: 'Trekking',
                frequency: 4
            }
        ]

db.<dbName>.findOne({'hobbies.title': 'Swimming'})
```

| Operator   | Example                                                              | Description                                 |
| ---------- | -------------------------------------------------------------------- | ------------------------------------------- |
| $size      | `db.<dbName>.find({hobbies: {$size: 4}})`                            | find doc where user have 4 hobby            |
| $all       | `db.<dbName>.find({hobbies: {$all: ['cooking', 'dancing']}})`        | find doc where all listed items are present |
| $elemMatch | `db.<dbName>.find({hobbies: {$elemMatch : {title: 'a', freq: 3} }})` | query by matching from an object            |

## Cursor Sorting Skipping Limit

These can be used together and works in the following order -
Sort -----> Skip -----> Limit

| Operator | Example                                         | Description                   |
| -------- | ----------------------------------------------- | ----------------------------- |
| sort()   | `db.<dbName>.find().sort({name: 1, rating:-1})` | sort by order name and rating |
| skip()   | `db.<dbName>.find().skip(10)`                   | skip certain doc              |
| limit()  | `db.<dbName>.find().limit(10)`                  | limit the documents           |

---

## Update

| Command                                                                         | Description                                                                  |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `db.<collection_name>.updateOne({...}, {$set: {key: "new_data"}})`              | Update the value of the respective key                                       |
| `db.<collection_name>.updateMany({...}, {$set: {key: "new_data"}})`             | Update many values which follows same condition                              |
| `db.<collection_name>.updateMany({}, {$set:{"key.subKey.subsubKey": "value"}})` | update nested values , won't work without the "" in the key.subkey.subsubkey |
| `db.<collection_name>.updateMany({...}, {$set:{key: "value"}}, {upsert: true})` | if filter not found than create new doc by upserting                         |
| `db.<collection_name>.updateMany({...}, {$unSet:{key: 1}})`                     | remove specific fields                                                       |
| `db.<collection_name>.updateMany({...}, {$inc:{key: num}})`                     | increment the value by num                                                   |
| `db.<collection_name>.replaceOne({...}, {key: "value"})`                        | replace a document by a new document                                         |

### Field Update Operators

| Name           | Description                                                                                                                                  |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `$currentDate` | Sets the value of a field to current date, either as a Date or a Timestamp                                                                   |
| `$inc`         | Increments the value of the field by the specified amount                                                                                    |
| `$min`         | Only updates the field if the specified value is less than the existing field value                                                          |
| `$max`         | Only updates the field if the specified value is greater than the existing field value                                                       |
| `$mul`         | Multiplies the value of the field by the specified amount                                                                                    |
| `$rename`      | Renames a field                                                                                                                              |
| `$set`         | Sets the value of a field in a document                                                                                                      |
| `$setOnInsert` | Sets the value of a field if an update results in an insert of a document. Has no effect on update operations that modify existing documents |
| `$unset`       | Removes the specified field from a document.                                                                                                 |

### Updating Arrays

#### Updating particular documents inside Array

```
db.<dbName>.updateMany(
    {
        hobbies: {
            $elemMatch: {
                title: 'Sports',
                frequency: {$gte: 3}
            },
        }
    },
    {
        $set: {
            'hobbies.$.frequency': 10
        }
    }
)
```

#### Updating all Array Elements

```

```

---

## Delete

| Command                                      | Description                                 |
| -------------------------------------------- | ------------------------------------------- |
| `db.<collection_name>.deleteOne({key: val})` | Delete one selected doc from the collection |
| `db.<collection_name>.deleteMany({})`        | Delete every item from collection           |

---

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

### Group data

group is used to group data with same fields. In Below example , we are grouping people from same state and printing the total. See https://docs.mongodb.com/manual/reference/operator/aggregation/group/#pipe._S_group for more functions like $sum

```
 db.<local_table-name>.aggregate(
     [
         {
             $group: {
                _id: {state : key from collection to group (eg. 'state.location')},
                total: {$sum: 1}
            }
        }
    ]
).pretty()
```

### Sort data

```
 db.<local_table-name>.aggregate(
    [
        {
             $group: {
                _id: {state : key from collection to group (eg. 'state.location')},
                total: {$sum: 1}
            }
        },
        {
            $sort: {
                total: 1
            }
        }
    ]
).pretty()
```

### Project

Project can be used to output any data. See aggregation 12th video of Max for details

```
 db.<local_table-name>.aggregate(
    [
        {
             $group: {
                _id: {state : key from collection to group (eg. 'state.location')},
                total: {$sum: 1}
            }
        },
        {
            $project: {
                state: '$_id.state',
                _id: 0,
                sentence: {$concat: ['we are from ', '$_id.state']},
                totalPersons: 1}
        }
    ]
).pretty()
```

---

## Indexes

### Create Index

| Command                                                                                | Uses                                         |
| -------------------------------------------------------------------------------------- | -------------------------------------------- |
| `db.<dbName>.getIndexes()`                                                             | See all available indexes                    |
| `db.<dbName>.createIndex({FieldName: 1})`                                              | Command for creating index in specific Field |
| `db.<dbName>.createIndex({FieldName: 1, FieldName: -1})`                               | Compound Index creation                      |
| `db.<dbName>.createIndex({FieldName: 1}, {unique: true})`                              | Unique index creation                        |
| `db.<dbName>.createIndex({FieldName: 1}, {partialFilterExpression: {age: ${gt: 50}}})` | Partial Index                                |
| `db.<dbName>.createIndex({FieldName: 'text'})`                                         | Create Text Index                            |
| `db.<dbName>.find({$text: {$search: 'keyword'}})`                                      | Search Keywords with text index              |
| `db.<dbName>.createIndex({FieldName: 1}, {background: true})`                          | Create Index in the background               |

---

## Importing a collection

Open Shell in the respective Folder. And run command

```
 mongoimport
    <fileName.json>
    -d <databaseName>
    -c <collectionName>
    --jsonArray
    --drop
```

| Command             | Uses                                                  |
| ------------------- | ----------------------------------------------------- |
| `mongoimport`       | Default command for importing json data into Database |
| `-d dbName`         | On which database to be inserted                      |
| `-c collectionName` | On which collection to be inserted                    |
| `--jsonArray`       | If it's an array with multiple doc                    |
| `--drop`            | drop any previous collection                          |

---

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

### Editing a defined collection

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
