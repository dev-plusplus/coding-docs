# Naming convention for 8base Database modeling

As a general rule, we use lower camel case (lowerCamelCase) for naming any field in the database. 

### General Rules

- Table names should be Camel Case (CamelCase) and always singular
- Do not use multiple fields for attributes, these are not searchable yet (Dec, 2020)
- Do not use Many to Many relationships, use a separate table always. This make the application easier to be modified
- CAREFUL: Use Mandatory fields for relationships having in consideration that, where it is required a cascade effect will be forced. Mandatory fields force the data to be erased when the related object is deleted

### Naming relationships: 

8base allow naming reverse relationship fields, and for that, we have a convention:

#### For the Many to One relation:

- For the reference field: `<entityName>`
- For the reverse relationship field: `<entityName><relatedTableName>Relation`

Example: Given EntityA and EntityB as Tables, a field on EntityA, that makes a reference to EntityB, must be called:

- `entityB` for the reference
- `entityBEntityARelation` for the reverse reference

#### For the One to One relation

- For the reference field: `<entityName>`
- For the reverse relationship field: `<entityName>`

Example: Given EntityA and EntityB as Tables, a field on EntityA, that makes a reference to EntityB, must be called:

- `entityB` for the reference
- `entityA` for the reverse reference

## Values on SWITCH fields

- Values on switch fields should always be UPPER_SNAKE_CASE
