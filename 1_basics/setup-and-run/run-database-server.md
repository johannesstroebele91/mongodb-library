# Basics

The MongoDB server can be started

- as a service and
- as a process

Multiple methods exist to start the MongoDB database server via the service:

1. Automatic start via MongoD (Windows only)
   - MongoDB is a service that
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

# Configuration via mongod

The `mongod` commandy can be configured by adding options:

- specify where the data for the databases is `mongod --dbpath C:\...\data`:
  - the folder needs to be created in advance
  - for it to work
- specify where the log data is `mongod --logpath C:\...\logs\mongod.log`
  - besides needing to create the folder,
  - a `mongod.log` file needs to be created
