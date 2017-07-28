# MongoDB Quick Start Guide
Quick start guide for installing and configuring MongoDB

## Install MongoDB on Windows
[Download Center](https://www.mongodb.com/download-center)

Simplify the path in the installer, I used C:\Program Files\MongoDB

### Initial Setup

Create the following dirs:
* C:\Program Files\MongoDB\data\db - to house the databases
* C:\Program Files\MongoDB\log - to house log files

Create a config file:
* C:\Program Files\MongoDB\mongo.cfg

 To install as a service
```cmd
mongod.exe --directoryperdb --dbpath "C:\Program Files\MongoDB\data\db" --logpath "C:\Program Files\MongoDB\log\mongo.log" --logappend --config "C:\Program Files\MongoDB\mongo.cfg" --rest --install
```

### Enable Authentication

[Enabling Authentication](https://docs.mongodb.com/manual/tutorial/enable-authentication/)

To enable authorization, connect to server and add user to admin database
```mongo
use admin
db.createUser({
    user: <username>,
    pwd: <plaintext password>,
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
    });
```

[More Info on MongoDB Roles](https://docs.mongodb.com/manual/reference/built-in-roles/)


In mongo.cfg (YAML), add the following:
```YAML
security:
    authorization: enabled
```

Restart the MongoDB service
```
net stop MongoDB
net start MongoDB
```

[Config File Options](https://docs.mongodb.com/v3.0/reference/configuration-options/)


# Using Mongoose

[Mongoose Docs](http://mongoosejs.com)

## Installation

`npm install --save mongoose`

## Connecting

```js
const mongoose = require('mongoose');

// Connect to db

// Pattern
//mongoose.connect('mongodb://<username>:<password>@host/<dbname>', {<options>});

// Example
mongoose.connect('mongodb://brycejech:myPassword@localhost/test', {useMongoClient: true, keepAlive: 1});
// Note that special chars in username/password should be URL percent-encoded
```

## Schema

With Mongoose, everything is derived from a Schema. Schemas map to a collection and defines what documents in that collection look like. Each key in a Schema defines a property that will be cast to it's associated SchemaType.

SchemaTypes:
* String
* Number
* Date
* Buffer
* Boolean
* Mixed
* ObjectId
* Array

Example:

```js
const Schema = mongoose.Schema;

let animalSchema = new Schema({
    name:       String,
    species:    String,
    actions:    [String], // Array of strings
    numLegs:    Number,
    isAlive:    Boolean,
    born:       Date
});
```

## Models

To use the Schema definition, have to convert our Schema into a Model. Pass the schema to `mongoose.model(modelName, schema);`.

Example:

```js
var animal = mongoose.model('animal', animalSchema);
```

Instance of `Models` *ARE* documents.
