# Basics

1. Download community edition: https://www.mongodb.com/try/download/community
2. Look into documentation for futher steps: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/
3. Install downloaded package using custom
   1. Check: server, client, router, and tools
   2. Check: Install MongoD as a Service
   3. Specify data and log directory:
      - data: specifies where MongoDB stores the data on your system
      - log: where messages and information is looged
      - compas: either directly install it or add it later via `https://www.mongodb.com/try/download/compass`
4. Install new Mongo shell
   1. Download it via `https://www.mongodb.com/try/download/shell`
   2. Install the shell (everything default)
5. Add MongoDB server to the PATH
   - Windows:
     - search via Windows menu for `system environmental variables`
     - click on `environmental variables`
     - in user variables section click on `path` and `edit`
     - add or change the path for the `bin` folder of the MongoDB
     - e.g. `C:\...\MongoDB\Server\4.4\bin`
6. Change the database and log directory if needed and adapt it for the config
   - database: create respective folders e.g. `C:\...\data`
   - logs: create respective folder and file e.g. `C:\...\logs\mongod.log`
   - adapt configuration based on changes
     - copy the paths into respective properties into
     - of the `mongod.cfg` file in the `bin` folder (e.g. `C:\...\MongoDB\Server\4.4\bin`)
