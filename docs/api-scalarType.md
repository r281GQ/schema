---
id: api-scalarType
title: scalarType
sidebar_label: scalarType
---

[GraphQL Docs for Scalar Types](https://graphql.org/learn/schema/#scalar-types)

Nexus allows you to provide an `asNexusMethod` property which will make the scalar available as a builtin on the definition block object. We automatically generate and merge the types so you get type-safety just like the scalar types specified in the spec:

```ts
const DateScalar = scalarType({
  name: "Date",
  asNexusMethod: "date",
  description: "Date custom scalar type",
  parseValue(value) {
    return new Date(value);
  },
  serialize(value) {
    return value.getTime();
  },
  parseLiteral(ast) {
    if (ast.kind === Kind.INT) {
      return new Date(ast.value);
    }
    return null;
  },
});
```

## Example of Upload scalar

```
import { GraphQLUpload } from 'graphql-upload';

export const Upload = GraphQLUpload;
```

## Example of DateTime scalar

```
import { GraphQLDate } from "graphql-iso-date";

export const DateTime = GraphQLDate;
```

## Exposing scalar as method

If you have an existing GraphQL scalar and you'd like to expose it as a method on the builder, call `asNexusMethod`:

```ts
import { GraphQLDate } from "graphql-iso-date";

export const GQLDate = asNexusMethod(GraphQLDate, "date");
```

```ts
const SomeObject = objectType({
  name: "SomeObject",
  definition(t) {
    t.date("createdAt"); // t.date() is now available (with types!) because of `asNexusMethod`
  },
});
```

Check the type-definitions or [the examples](https://github.com/graphql-nexus/schema/tree/develop/examples) for a full illustration of the various options for `scalarType`, or feel free to open a PR on the docs to help document!
