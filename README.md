## What is GraphQL?

GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

![preview](https://github.com/hemerajs/graphql-hemera/blob/master/media/preview.PNG)

## What is Hemera?
[Hemera](https://github.com/hemerajs/hemera) is a microservice toolkit based on the [NATS](https://nats.io/) Server. You can use efficient pattern matching to have the most flexibility in defining your RPC's. It doesn't matter where your server or client lives. With Hemera you can create microservices without to worry about all the different aspects in a distributed system like routing, load-balancing, service-discovery, clustering, health-checks ...

This setup demonstrate how to use Hemera for resolving your GraphQL queries. Because of the flexibility of GraphQL you have to deal with many resolvers hemera can provide you a way to manage this in a very simple and flexible way. Combine GraphQL with the power of pattern matching.

- The [User Service](src/user-service) is provided by Hemera
- The [Resolvers](src/graphql/resolvers.js) are fullfilled by Hemera
- The [payload](src/user-service/index.js) is validated by Hemera

## Show me
Resolver
```js
const resolvers = (hemera) => ({
  Query: {
    getUserById (root, { id }) {
      return hemera.act({
        topic: 'user',
        cmd: 'getUserById',
        id
      })
    },
    ...
```
Implementation: Can be hosted anywhere
```js
  hemera.add({
    topic,
    cmd: 'getUserById',
    id: Joi.number().required()
  }, function (req, reply) {
    const matchedUser = users.filter(x => x.id === req.id)
    reply(null, matchedUser.length ? matchedUser[0] : null)
  })
```

## Getting started

```js
npm install
npm run start
```

## GraphiQL Dashboard

```
http://localhost:3000/graphiql
```

## GraphQL endpoint

```
http://localhost:3000/graphql
```
