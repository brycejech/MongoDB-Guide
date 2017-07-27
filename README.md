# MongoDB-Guide
Quick start guide for installing and configuring MongoDB

## Install MongoDB on Windows
https://www.mongodb.com/download-center

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


In mongo.cfg (YAML file), add the following:
```YAML
security:
    authorization: enabled
```

[Config File Options](https://docs.mongodb.com/v3.0/reference/configuration-options/)
