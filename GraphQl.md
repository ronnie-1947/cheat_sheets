# GraphQL

## Table Of Contents


____

## Creating Your Own GraphQL API

Install GraphQL Yoga from NPM
```
npm i graphql-yoga
```

Running GraphQL server
```
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