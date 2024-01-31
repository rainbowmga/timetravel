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
- F1: Support common data types: string, int, float,...
- F2: Users are able to create a record, insert more versions of the record
- F3: Users are able to retrieve a record at a given timestamp
- F4: Users are able to get a list of versions given a record
### Non-functional
- NF1: Persistent data storage with high availablity
- NF2: Consistency, patial persistence: the system is able to handle concurrent writes, keeping the linearability of versions of a record, i.e. a linear linked list without branches

# Acceptance criteria
1. F1 + F2: Able to create records with different data type. For now, we won't allow changing data type of a record once it's been created, e.g. a float type record like insurance price wouldn't accept a subsequent record version of type string.
2. F4: Users are able to get a list of versions given a record
3. F3: Able to query for a particular record's value at a given time. If the record has been deleted or does not exist, return a sentinel value depending on the record's data type.
4. NF1: insert some records, turn off the computing units and turn them back on, should be able to retrieve previous records stored persistenly in SQLite.
5. NF2: after multiple insertions of different records and their versions, have a script to check the linearability of all records.


