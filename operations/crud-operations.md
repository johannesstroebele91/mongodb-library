# Basics

CRUD are typical operations executed against a database:

- Create
- Read
- Update
- Delete

# Explanation

Most of the commands e.g. `db.products.insertOne()` include

- `db` for referencing the current database
- `products` creates a collection on the fly or uses an existing one
- e.g. `insertUpdate()` query command, which might has these parameters
  - Filter: narrow down which documents to changes (`{}` selects all documents)
  - Data: describing the change
  - Options: configuration

# Example

For a flight

- a new flight can be scheduled (create)
- a flight information updated (update)
- a flight cancled (delete)
- flight information displayed (read)
