# Authentication and Authorization

_User can be Created and given roles_

1. Create a user via `createUser()` with a
   - username and password
   - on a database (e.g. admin),
     - which does not limit the authentication
     - to this one database
2. Assign at least one role to the user to give privileges
3. Update the users if needed

# Login users

_Users can loggin by_

1. Staring the MongoDB server with the
   - `--auth parameter`
   - e.g. `mongod --dbpath C:\mongodb\data --auth`
2. Connect from a client (mongo, mongosh) application
3. Loggin via these two options if a users exists
   1. option: `mongo -u username - p password`
   2. option:
      1. switch to the respective db e.g. `use admin`
      2. authenticate `db.auth('username', 'password)'`
4. MongoDB allows to create a single user
   - if there are NO users for the database AND
   - if you are connected to a local database by
   1. switch to the admin database `use admin`
   2. add a role e.g. `db.createUser({user: "admin", password: "admin", roles: ["userAdminAnyDatabase"]})`
