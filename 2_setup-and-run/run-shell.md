# Basics

Before the shell can be used

- the MongoDB database server
- needs to be started
- which provides the databases

Several MongoDB shells exists

- which can be used
- as shown below

# 1. Start new shell `mongosh` via `MongoDB Compass` (might need to be installed)

1. Open MongoDB Compass
2. Click on `mongosh` at the bottom of the UI

# 2. Start new shell `mongosh` manually (might need to be installed)

Several methods exist to start the new shell

1. Search for mongosh in the windows apps and start it
2. Open terminal > `mongosh`
3. Open terminal > `...\mongosh\mongosh.exe` (installation folder)
4. Open terminal > `mongosh mongodb://127.0.0.1:27017/` (installation folder)

# 3. Start old shell `mongo.exe` (default)

Multiple methods exist to send commants (e.g. CRUD-operations) to the MongoDB database server:

1. Open terminal > `mongo`
2. Open terminal > `C:\Users\stroebele\AppData\Local\Programs\mongosh`
3. Open installation folder of mongo > `mongo.exe`
   - identify where the file (also `mongosh.exe` possible) was installed
     - windows:
       1. option services -> mongodb server -> path to exe
       2. option task manager -> details -> mongod.exe -> general -> location
     - unix /macos: terminal -> `grep dbpath /etc/mongodb.conf`

# Configuration

Configuration information

- can be accessed using
- `mongo --help`

The `mongo` command can be configured by adding options:

- Non-default **port**: e.g. `mongo --port 27018`
- Non-default **host**: e.g. `mongo --host xxxxx`
- **Authentication** is important for security (TODO)
  - User: `mongo -u someUser`
  - Password: `mongo -p someUser`
- **SSL** is important for security (TODO)
