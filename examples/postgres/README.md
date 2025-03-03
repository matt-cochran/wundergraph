# WunderGraph example with PostgreSQL

WunderGraph is not just able to manage existing APIs, like REST and GraphQL,
but can also act as an ORM replacement.
In this scenario, WunderGraph allows you to directly speak GraphQL to your Database.

We support PostgreSQL, MySQL, SQLite, SQLServer, MongoDB, CockroachDB and Planetscale.
In this scenario, we're using a local PostgreSQL database via docker-compose.

## Getting started

Install the dependencies and run the application.

```shell
npm install && npm start
```

## Checkout the generated Schema

Once the introspection is complete,
you can have a look at the generated GraphQL Schema:

`examples/postgres/.wundergraph/generated/wundergraph.app.schema.graphql`

We've pre-defined two GraphQL Operations in the operations directory for you:

`examples/postgres/.wundergraph/operations`

## Interacting with the API

You've got multiple options to use the API.
Let's first have a look at GraphiQL.
To do so, head over to http://127.0.0.1:9991/app/main/graphql.

### GraphiQL

Paste the following two Queries into the GraphiQL IDE:

```graphql
query Users {
  db_findManyusers {
    id
    name
    email
  }
}

query Messages {
  db_findManymessages {
    id
    message
    users {
      id
    }
  }
}
```

If you hit the play button, you can execute each of the queries.

This is great for Development or when you're trying to explore the API,
but what about a production use-case?

You're probably already familiar with WunderGraph's JSON-RPC interface.
If not, here's a quick refresher:

- when running `wunderctl up`, we're looking into the `.wundergraph/operations` directory for `*.graphql` files
- each file should contain exactly one GraphQL Operation
- each file will be compiled into a JSON-RPC endpoint
- the name of the endpoint is determined by the file name

Now, let's query our JSON-RPC API:

### Get the Users

```shell
curl -X GET http://localhost:9991/app/main/operations/Users
```

### Get the Messages

```shell
curl -X GET http://localhost:9991/app/main/operations/Messages
```

## Wrap up

That's it! You've learned the most important aspects of using WunderGraph as an ORM to speak to your Database.

If you want to clean up the environment, run the following code.

```shell
npm run cleanup
```
