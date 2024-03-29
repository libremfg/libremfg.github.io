+++
title = "GraphQL Directives"
description = "The list of all directives supported by Rhize's GraphQL implementation. Full details linked within for all directives available with GraphQL."
categories = "reference"
weight = 200
draft = true
[menu.main]
  name = "Directives"
  identifier = "directives"
  parent = "how-to-query"
+++

<!-- vale off -->

The list of all [directives](https://www.apollographql.com/docs/apollo-server/schema/directives/) supported by Rhize's implementation of Dgraph.

### @auth

`@auth` allows you to define how to apply authorization rules on the queries/mutation for a type.

Reference: [Auth directive](/graphql/authorization/directive)

### @cascade

`@cascade` allows you to filter out certain nodes within a query.

Reference: [Cascade](/graphql/queries/cascade)

### @custom

`@custom` directive is used to define custom queries, mutations and fields.

Reference: [Custom directive](/graphql/custom/directive)

### @default

The `@default` directive allows you to specify values that should be used when nil values are received for either `add` mutations or `update` mutations

Reference: [Default directive](/graphql/schema/default)

### @deprecated

The `@deprecated` directive lets you mark the schema definition of a field or `enum` value as deprecated, and also lets you provide an optional reason for the deprecation.


### @dgraph

`@dgraph` directive tells us how to map fields within a type to existing predicates inside Dgraph.


### @generate

The `@generate` directive is used to specify which GraphQL APIs are generated for a type.

Reference: [Generate directive](/graphql/schema/generate)

### @hasInverse

`@hasInverse` is used to setup up two way edges such that adding a edge in
one direction automically adds the one in the inverse direction.

Reference: [Linking nodes in the graph](/graphql/schema/graph-links)

### @id

`@id` directive is used to annotate a field which represents a unique identifier coming from outside
 of Dgraph.

Reference: [Identity](/graphql/schema/ids)

### @include

The `@include` directive can be used to include a field based on the value of an `if` argument.

Reference: [Include directive](/graphql/queries/skip-include)

### @lambda

The `@lambda` directive allows you to call custom JavaScript resolvers. The `@lambda` queries, mutations, and fields are resolved through the lambda functions implemented on a given lambda server.

Reference: [Lambda directive](/graphql/lambda/overview)

### @primary-key

The `@primary-key`  allows you to specify a list of fields where the concatenation of values of those fields must be unique in the database

Reference: [Primary Key](/graphql/schema/primarykey)

### @remote

`@remote` directive is used to annotate types for which data is not stored in Dgraph. These types
are typically used with custom queries and mutations.

Reference: [Remote directive](/graphql/custom/directive/#remote-types)

### @remoteResponse

The `@remoteResponse` directive allows you to annotate the fields of a `@remote` type in order to map a custom query's JSON key response to a GraphQL field.

Reference: [Remote directive](/graphql/custom/directive/#remote-response)

### @search

`@search` allows you to perform filtering on a field while querying for nodes.

Reference: [Search](/graphql/schema/search)

### @secret

`@secret` directive is used to store secret information, it gets encrypted and then stored in Dgraph.

Reference: [Password Type](/graphql/schema/types/#password-type)

### @skip

The `@skip` directive can be used to fetch a field based on the value of a user-defined GraphQL variable.

Reference: [Skip directive](/graphql/queries/skip-include)

### @withSubscription

`@withSubscription` directive when applied on a type, generates subsciption queries for it.

Reference: [Subscriptions](/graphql/subscriptions)

### @lambdaOnMutate

The `@lambdaOnMutate` directive allows you to listen to mutation events(`add`/`update`/`delete`). Depending on the defined events and the occurrence of a mutation event, `@lambdaOnMutate` triggers the appropriate lambda function implemented on a given lambda server.

Reference: [LambdaOnMutate directive](/graphql/lambda/webhook)


### @default

The `@default` directive provides default values to be stored when not supplied in a mutation (`add`/`update`).  The directive can be used with the current DateTime (via `$now') to allow timestamping of mutation events.

Reference: [Default directive](/graphql/schema/default)
