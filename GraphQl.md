# GraphQL

## Table Of Contents

* [Creating Your Own GraphQL API](GraphQl.md#creating-your-own-graphql-api)
* [GraphQL creating custom Types](GraphQl.md#graphql-creating-custom-types)
* [Relational Data](GraphQl.md#relational-data)

***

## Creating Your Own GraphQL API

Install GraphQL Yoga from NPM

```
npm i graphql-yoga
```

Running GraphQL server

```javascript
import { GraphQLServer } from 'graphql-yoga'

// Type definitions (schema)
const typeDefs = `
    type Query {
        hello: String!
    }
`

// Resolvers
const resolvers = {
    Query: {
        hello() {
        return 'This is my first query!'
        }
    }
}

const server = new GraphQLServer({
    typeDefs,
    resolvers
})

server.start(() => {
    console.log('The server is listening on port 4000!')
})
```

***

## GraphQL creating custom Types

```javascript
const typeDefs = `
    type Query {
        product: [Product!]!
        greeting(name: String): String!
    }

    type Product{
        title: String!,
        price: Float!
    }
`

const resolvers = {
    Query: {
        product(){
            return [
                {
                    title: 'Product1',
                    price: 20.00
                }
            ]
        },

        greeting(parent, args, ctx, info){
            return `Hello ${args.name}`
        }
    }
}
```

***

## Relational Data

Setting up Association between 2 Schema

```graphql
type User{
    id: ID!,
    name: String!,
    age: Int!
}

type Post {
    id: ID!,
    body: String!,
    authorId: ID!,
    author: User!
}

const resolver = {
    Query: {

    },

    Post: {

        author(parent){
            
            const authorId = parent.authorId
            const user = data.users.filter(c=>c.id===authorId)

            return user
        }
    }
}
```
