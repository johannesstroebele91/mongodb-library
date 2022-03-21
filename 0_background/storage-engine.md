# Basics

The storage engine (WiredTiger)

- handels the requests form the MongoDB to
- read and write data to database files directly (slow)
- read and write data in memory and later in database files (fast)

Reading and writing data in memory is faster because

- the storage engine loads chunks of data
- which is often used into memory
- so requests can be handled faster
