# Basics

Before the shell can be used

- the MongoDB database needs to be started
- which can be done as follows:

# 1.Start MongoDB database server

Multiple methods exist to start the MongoDB database server via the service:

1. Automatic start via MongoD (Windows only)
   - MongoD is a service that
   - can automatically spin up the database server
   - when the system is starting
   - Installation: via setup wizard by checking "Install MongoD as a Service"
2. Manual start via terminal (all operation systems, example for windows)
   - identify the db path which can be everywhere, for example:
     - installation folder for mongodb e.g.`C:\Program Files\MongoDB\Server\4.4`
     - in the root folder `C:\data`
     - this can help
       1. option services -> mongodb server -> part of path to exe
       2. option task manager -> details -> mongod.exe -> general -> location
     - unix /macos: terminal -> `grep dbpath /etc/mongodb.conf`
   - Run: `mongod --dbpath ...\data` or in case auth should be used with `mongod --dbpath ...\data --auth`
   - Run: `net start MongoDB` (`net stop MongoDB` to stop the service)
3. Manual start via service program (all operation systems, example for windows)
   - Open Windows menu
   - Search for Service
   - Search for MongoDB server
   - Click on `start`
   - Installation: via setup wizard by un-checking "Install MongoD as a Service"

# 2. Start client

## 2.1. Start new shell (might need to be installed)

Several methods exist to start the new shell

1. Search for mongosh in the windows apps and start it
2. Open terminal > `mongosh`
3. Open terminal > `...\mongosh\mongosh.exe` (installation folder)

## 2.2. Old shell `mongo.exe` (default)

Multiple methods exist to send commants (e.g. CRUD-operations) to the MongoDB database server:

1. Open terminal > `mongo`
2. Open terminal > `C:\Users\stroebele\AppData\Local\Programs\mongosh`
3. Open installation folder of mongo > `mongo.exe`
   - identify where the file (also `mongosh.exe` possible) was installed
     - windows:
       1. option services -> mongodb server -> path to exe
       2. option task manager -> details -> mongod.exe -> general -> location
     - unix /macos: terminal -> `grep dbpath /etc/mongodb.conf`
