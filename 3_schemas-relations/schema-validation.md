# Basics

1. Data is passed via e.g. insertOne(), updateOne() to change a collection
2. MongoDB will validate (accepts or rejects)
   - the passed data
   - based on the validation schema

MongoDB enables to define

- schema validation
- validation level
- validation action

# Validation level

Configuration possibilities:

- which kinds of operations get validated
- the documents which get validated
- the valuation level:
  - strict: all insert and updates are checked
  - moderate: all inserts are checked,
    - but updates only for documents
    - which were valid before

# Validation action

Configuration possibilities:

- what to do with validation errors
  - error: throw error and deny insert or update
  - warning: so proceed to change data, but log a warning
