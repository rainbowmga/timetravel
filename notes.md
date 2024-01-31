# Terminology
Persistence: Each update creates a new version of the data structure.
All old versions can be queried and may also be updated.

Types of persistence
- Partial persistence
  - Updates operate only on latest version of structure
  - Queries can examine old versions
  - History is linear (sequence of operations forms a single timeline)
- Full persistence
  - Updates can be applied to any version
  - History forms a tree
(updating an old version creates a new branch)
- Confluent persistence
  - Updates can combine multiple versions (like git merge)
  - History forms a directed acyclic graph
# Requirements
## Scope
- Build a record system that can store different versions of a record and allow looking up the state of a record at a given timestamp
- Given the example of this problem, e.g. business owners change their physical address, partial persistence seems suitable as updates are likely to be serializable and a new update is likely to be applied on top of the most recent version
- Out of scope: support deleting a record or soft deleting a record version. Though these are important cases to handle, I think it's likely out of scope for this exercise.
## Requirements
### Functional
- F1 (out of scope): Support common data types: string, int, float,...
  - update: after reading the boilerplate, I can see that the data type is `string`. I'll mark this requirement as out of scope for now.
- F2: Users are able to create a record, insert more versions of the record
- F3: Users are able to retrieve a record at a given timestamp
- F4: Users are able to get a list of versions given a record
### Non-functional
- NF1: Persistent data storage with high availablity
- NF2: Consistency, patial persistence: the system is able to handle concurrent writes, keeping the linearability of versions of a record, i.e. a linear linked list without branches

# Acceptance criteria
1. F2: Able to create a record
- `POST /api/v1/record/{id}` will create a new record if the record ID is not present
2. F2: Able to modify an existing record (without losing its previous values)
- `POST /api/v1/record/{id}` will modify an existing record if the record ID is already present. Modifications include adding a new field, editing an existing field, and deleting a new field by setting that field's value to `null`
- subsequent `GET /api/v1/record/{id}` will return data that is the composition of the record's versions
2. F4: Users are able to get a list of versions given a record
- a new API endpoint `GET /api/v1/record/{record_id}/versions`
3. F3: Able to query for a particular record's values at different versions
- a new API endpoint `GET /api/v1/record/{record_id}/{timestamp}`, with timestamp being the parameter determining which versions decide the record's values at the time.
4. NF1: insert some records, turn off the computing units and turn them back on, should be able to retrieve previous records stored persistenly in SQLite.
5. NF2: after multiple insertions of different records and their versions, have a script to check the linearability of all records.


