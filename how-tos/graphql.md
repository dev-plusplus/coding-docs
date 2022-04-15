# Graphql Code generator <-> 8base configuration

We use [Graphql code generator](https://www.graphql-code-generator.com/docs/getting-started/index) 
which is a tools that read the graphql schema and generate a file with all the types
for tables, queries and mutations

### How to use?



To use this library, following these instructions:

1. Install this library, and GraphQL Codegen and the relevant plugins:

```
npm i -D @graphql-typed-document-node/core @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-operations @graphql-codegen/typed-document-node
```

And if you don't already have a dependency for `graphql`, add it to your project:

```
npm i graphql
```

> Codegen is needed because we need to precompile `.graphql` files into `DocumentNode`, and burns the types in it to create `TypedDocumentNode` object.
2. Create GraphQL-Codegen configuration file (`codegen.yml`) and the root of the project, and point to your GraphQL schema and your `.graphql` operations files:

```yml
overwrite: true
schema: SCHEMA_FILE_OR_ENDPOINT_HERE
documents: "src/**/*.graphql"
generates:
  src/shared/types/generated.ts:
    plugins:
      - typescript
      - typescript-operations
      - typed-document-node
    config:
      avoidOptionals:
        field: true
      maybeValue: T
```
3. Then add the `npm script` to run the generator and build the types from the schema
```json
  "scripts": {
    "generate": "graphql-codegen --config codegen.yml"
  },
```
4. Run `npm run generate` and enjoy the types.

A video for a better explanation

https://user-images.githubusercontent.com/34176666/135188105-881386ca-41e9-4867-b087-7f22bc672855.mp4


### How to use the genrated `types` and `TypedDocumentNode`?


https://user-images.githubusercontent.com/34176666/135191668-4666fc35-8a68-4fed-ab7e-37aa0b3c7449.mp4

### How to use with React Apollo Hooks?
1. Install this library, and GraphQL Codegen and the relevant plugins:

```
npm i -D @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-operations @graphql-codegen/typescript-react-apollo @graphql-codegen/add
```
And if you don't already have a dependency for `graphql`, add it to your project:

```
npm i graphql
```

2. Create GraphQL-Codegen configuration file (`codegen.yml`) and the root of the project, and point to your GraphQL schema and your `.graphql` operations files:

```yml
overwrite: true
schema: SCHEMA_FILE_OR_ENDPOINT_HERE
documents: "src/**/*.graphql"
generates:
  src/shared/types/generated.ts:
    plugins:
      - add:
          content: '/* eslint-disable */'
      - typescript
      - typescript-operations
          onlyOperationTypes: true
            avoidOptionals:
              field: true
      - typescript-react-apollo
    config:
      maybe: T
      scalars:
        JSON: {}
        DateTime: string
        Date: string
        BigInt: string
        Time: string
```
Then follow the previos step from the 3



https://user-images.githubusercontent.com/34176666/135481943-b6858566-1a29-4628-8fe9-885fda5f4ee6.mp4




### How to used it




https://user-images.githubusercontent.com/34176666/135481926-dc6ae6ed-4496-47e2-9b45-706d1264f76d.mp4


