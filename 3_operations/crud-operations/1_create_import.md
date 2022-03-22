# Basics

Import one or multiple documents in a collection by

- starting the MongoDB server
  - opening the terminal
  - mongod -config "C:\Program Files\MongoDB\Server\4.4\bin\mongod.cfg"
- import the data
  - e.g. `mongoimport example-import-data.json -d movieData -c movies --jsonArray --drop`
  - full path to the file name or navigate into the directory (e.g. `example-import-data.json`, `C:\...\Downloads\boxoffice.json`)
  - which database to impoort data `-d movieData`
  - which collection to impoort data `-c movies`
  - specify if the file is
    - one document (nothing needs to be added) OR
    - an array of documents `--jsonArray`
    - or not (IMPORTANT! default is just import of one document!!!!)
  - specify what to do if the document already exists `--drop`
    - specifies that if the collection already exists
      - it will be dropped completely and
      - the new data added
    - otherwise it will append the data to the existing collection
