# Basics

Multiple methods exist

- to start the MongoDB database server
- as a service or process:

1. Automatic start via service (Windows only)
   - when the system is starting
   - Installation: via setup wizard by checking "Install MongoD as a Service"
2. Manual start as a process via terminal (all operation systems, example for windows)
   - identify the db path which can be everywhere, for example:
     - installation folder for mongodb e.g.`C:\Program Files\MongoDB\Server\4.4`
     - in the root folder `C:\data`
     - this can help
       1. option services -> mongodb server -> part of path to exe
       2. option task manager -> details -> mongod.exe -> general -> location
     - unix /macos: terminal -> `grep dbpath /etc/mongodb.conf`
   - Run: `mongod --dbpath ...\data` or in case auth should be used with `mongod --dbpath ...\data --auth`
   - Run: `net start MongoDB` (`net stop MongoDB` to stop the service)
3. Manual start via service (all operation systems, example for windows)
   - Open Windows menu
   - Search for Service
   - Search for MongoDB server
   - Click on `start`
   - Installation: via setup wizard by un-checking "Install MongoD as a Service"

# Configuration via mongod

The `mongod` command can be configured by adding options (`mongod --help` for more infos):

- where the **data for the databases** is `mongod --dbpath C:\...\data`:
- the folder needs to be created in advance
- for it to work
- where the **log data** is `mongod --logpath C:\...\logs\mongod.log`
  - besides needing to create the folder,
  - a `mongod.log` file needs to be created
- store each database in a **separate directory** `mongod --directoryperdb`
  - is advantages if working with many databases
  - which is often the case
- **Fork** a server process `mongod --fork --logpath C:\...\logs\mongod.log`
  - runs the MongoDB as a service in the background
    - so it is started as a child instead of a parent (foreground) process
    - so the terminal can be continued to be used for e.g. shell commands
  - the logpath is needed,
    - because the messages cannot be displayed in the terminal
    - and are logged to the log file
  - mac and linux (easy)
    - use the command above using `sudo`
    - which can be killed by
      - switching to the admin database `use admin` and
      - run db.shotdownServer()
  - windows (harder)
    - MongoDB needs to be installed as a service and
    - opening an terminal as an administrator and
    - the command `net start MongoDB` ran
    - which can be killed by
      - switching to the admin database `use admin` and
      - run db.shotdownServer() OR
        - opening an terminal as an administrator and
        - running `net stop MongoDB`
- Use **authentication** to control who can change the database `mongod --auth` (TODO)
- **SSL certification** is important for security (TODO)
- **Replication** is important for security (TODO)
  - a replica set is a group of mongod processes that maintain the same data set
  - it enables to provide a level of fault tolerance against the loss of a single database server
  - therefore making the database server more robust
