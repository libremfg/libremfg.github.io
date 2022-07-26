+++
title = "The @default Directive"
description = "The @default directive is used to define values that should be used in place of nil values for add and update mutations"
weight = 2
[menu.main]
parent = "directives"
+++

Here's the GraphQL definition of the directives:

```graphql
directive @default(add: DgraphDefault, update: DgraphDefault) on FIELD_DEFINITION
input DgraphDefault {
  value: String
}
```
Syntax:
```graphql
type Type {
  field: FieldType @default(
    add: {value: "value"}
    update: { value: "value"}
  )
}
```
Where a value is not provided as input for a mutation, the add value will be used if the node is being created, and the update value will be used if the node exists and is being updated. Values are provided as strings, parsed into the correct field type by Dgraph.

The string $now is replaced by the current DateTime string on the server, ie:
```graphql
type Type {
  createdAt: DateTime! @default(
      add: { value: "$now" }
  )
  updatedAt: DateTime! @default(
    add: { value: "$now" }
    update: { value: "$now" }
  )
}
```

Schema validation will check that:

Int field values can be parsed strconv.ParseInt
Float field values can be parsed by strconv.ParseFloat
Boolean field values are true or false
$now can only be used with fields of type DateTime (could be extended to include String?)
Schema validation does not currently ensure that @default values for enums are a valid member of the enum, so this is allowed:
```graphql
enum State {
  HOT
  NOT
}

type Type {
    state: State @default(add: { value: "FOO"})
}
```

## Restrictions / Roadmap

Our default directive is still in beta and we are improving it quickly.  Here's a few points that we plan to work on soon:

* adding the ability to specify a query to get the default value
* adding additional expressions to default times other than ${now}
---
