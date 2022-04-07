# 1. Setup authentication and authorization by creating the first user

If you decided to

- secure your database via
- authentication and authorization AND
- now users exist until now
- you need to follow the following steps:

1. Staring the MongoDB server locally with the
   - `--auth parameter`
   - e.g. `mongod --dbpath C:\mongodb\data --auth`
2. Connect from a client (mongo, mongosh) application via these options:
   1. option `mongo`
   2. option `mognosh`
   3. option`mongo`
3. Switch to the admin database `use admin`
4. Add a user
   - with username and password AND
   - a role (group of privileges)
   - for one, multiple, or all databases (e.g. admin),
   - e.g. `db.createUser({user: "admin", password: "admin", roles: ["userAdminAnyDatabase"]})`
5. Loggin via these two options if a users exists
   1. option: `mongo -u test -p test --authenticationDatabase admin`
   2. option (especially if you use mongosh):
      1. switch to the respective db e.g. `use admin`
      2. authenticate `db.auth('username', 'password)'`
6. Update the users if needed

# 2. Login after setup

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

# 3. Create additional users

A user can be created by

1. login in with a user that has the rights `e.g. mongo -u root -p root --authenticationDatabase admin`
2. Switching to the database for which a user should be created e.g. `shop`
3. Creating the user e.g. `db.createUser({user: 'appdev', pwd: 'dev', roles: ["readWrite"]})`

PS the role `readWrite`

- is a build in role
- which is specified in the file `roles.md`

# 4. Implications of assigning a role

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

# 5. Updating the rights of a user

It can be updated by

1. switching to a user with admin rights (logout before)
2. Updating the user by passing the parameters
   - the username in string `test`
   - assign new roles (replaces the old roles)
   - e.g. `db.updateUser("appdev", {roles: ["readWrite", {role: "readWrite", db: "blog"}]})`

# 6. Find out the rights of a user

`db.getUser("someUsername")`
