# Authentication and Authorization

_User can be Created and given roles_

1. Create a user via `createUser()` with a
   - username and password
   - on a database (e.g. admin),
     - which does not limit the authentication
     - to this one database
2. Assign at least one role to the user to give privileges
3. Update the users if needed

# Login

_Users can loggin by_

1. Staring the MongoDB server with the
   - `--auth parameter`
   - e.g. `mongod --dbpath C:\mongodb\data --auth`
2. Connect from a client (mongo, mongosh) application
3. Loggin via these two options if a users exists
   1. option: `mongo -u test -p test --authenticationDatabase test`
   2. option:
      1. switch to the respective db e.g. `use someDB`
      2. authenticate `db.auth('username', 'password)'`
4. MongoDB allows to create a single user
   - if there are NO users for the database AND
   - if you are connected to a local database by
   1. switch to the admin database `use admin`
   2. add a role e.g. `db.createUser({user: "admin", password: "admin", roles: ["userAdminAnyDatabase"]})`
   3. Loggin via these two options if a users exists
      1. option: `mongo -u test -p test --authenticationDatabase admin`
      2. option:
         1. switch to the respective db e.g. `use admin`
         2. authenticate `db.auth('username', 'password)'`

# Create new user

A user can be created by

1. login in with a user that has the rights `e.g. mongo -u root -p root --authenticationDatabase admin`
2. Switching to the database for which a user should be created e.g. `shop`
3. Creating the user e.g. `db.createUser({user: 'appdev', pwd: 'dev', roles: ["readWrite"]})`

PS the role `readWrite`

- is a build in role
- which is specified in the file `roles.md`

# Working with a user

Users that have the role e.g. `readWrite`

- cannot use operations
- such as `insert()`

You CANNOT log into **MULTIPLE USERS**

- so you have to logout with the current user first AND
- login with the respective other user
- before you can work with the another user
- using
  - `db.logout()`
  - or quitting the shell
