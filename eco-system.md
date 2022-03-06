# Basics

1. MongoDB database
   - self-managed & enterprise solution: community server (free), enterprise server (for security & config)
   - cloud manager (ops)
   - atlas (cloud)
   - compass: graphical UI which allow to connect to database an look into data more conveniently
   - BI connectors: allow to connect to different BI tools
   - MongoDB charts: visualize data
2. Stitch (serverless backend solution)
   - gives serverless query API to query data efficiently from inside client apps (e.g. React)
   - provide serverless functions (like Google cloud functions, AWS Lambda)
     - to exectue code in cloud on demand
     - which can be totally unrelated to the databases
     - but enables to process data e.g. from database directly
   - database trigers: service that allows to
     - listen to events in a database (e.g. document added) and
     - execute a function in response (e.g. send an email that a document was added)
   - real-time sync: for syncing data accross multiple devices
