# Basics

Import one or multiple documents in a collection by

- opening the terminal
- go to the respective folder that has the file that needs to be imported
- execute the import command (e.g. `mongoimport example-import-data.json -d movieData -c movies --jsonArray --drop`)
  - full path to the file name (e.g. `example-import-data.json`)
  - specify into which database to impoort data `-d movieData`
  - specify into which collection to impoort data `-c movies`
  - specify if the document is
    - an array: `--jsonArray`
    - or not (default is nothing)
  - specify what to do if the document already exists `--drop`
    - specifies that
      - if the collection already exists
      - it will be dropped completely and
      - the new data added
    - otherwise it will append the data to the existing collection
