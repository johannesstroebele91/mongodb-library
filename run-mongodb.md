# Start MongoDB database server

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
   - Run: `mongod --dbpath ...\data`
   - Run: `net start MongoDB` (`net stop MongoDB` to stop the service)
3. Manual start via service program (all operation systems, example for windows)
   - Open Windows menu
   - Search for Service
   - Search for MongoDB server
   - Click on `start`
   - Installation: via setup wizard by un-checking "Install MongoD as a Service"

# Start client

Multiple clients exist to send commants (e.g. CRUD-operations) to the MongoDB database server:

1. Start shell client
   - identify where the `mongo.exe` file (also `mongosh.exe` possible) was installed
     - windows:
       1. option services -> mongodb server -> path to exe
       2. option task manager -> details -> mongod.exe -> general -> location
     - unix /macos: terminal -> `grep dbpath /etc/mongodb.conf`
   - start the shell by clicking on `mongo.exe`
