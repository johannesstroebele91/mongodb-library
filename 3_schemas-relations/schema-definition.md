# Approach

1. In which format will the data be fetched (size of data fetched)
2. How often is data fetched and changed?
   - optimize for write or reads?
   - most often it is reads
   - when data is written "often",
     - duplicates like with nested documents
     - should be avoided
3. How much data is saved and how big is it?
   - e.g. if all citizens of New York City,
   - embedding is a bad choice
   - although the relation may be 1:n
4. How is the data related in general?
   - 1:1
   - 1:n
   - n:m
5. Are duplicates problematic?
   - requires the app many updates
6. Are data and storage limit hit?
   - not likely
   - but should be kept in mind
7. The concrete steps can be seen
   - in the `schema-validation.md`

# How to derive the data structure

The structure of data depends heavily on the use case, e.g.:

1. Which data is needed or needs to be generated?
   - results in the fields that are needed and how they should relate
   - e.g. user information
2. Where is the data needed?
   - results in collections and field groupings that are needed
   - e.g. welcome page needs these collections and those field groups
3. Which kind of data needs to be displayed?
   - results in which queries are needed
   - e.g. welcome page needs the product names
4. How often does the data need to be fetched?
   - results in if optimization for easier fetching is needed
   - e.g. for every page reload
5. How often should data be written or changed?
   - results in if optimization for easier writing is needed
   - e.g. orders needs to be written often
   - e.g. product data only rarely
