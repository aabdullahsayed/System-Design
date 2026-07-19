# GraphQL

## What is it?
GraphQL is a query language and runtime for APIs where the CLIENT specifies exactly what data it needs in a single request — a fundamentally different model from REST's fixed, server-defined endpoint shapes.

## Why care?
GraphQL solves two very real REST pain points: **over-fetching** (getting more data than you need) and **under-fetching** (needing multiple round-trips to assemble a full view) — a genuinely common real-world API design choice you'll encounter, especially at companies with complex, nested data needs (Facebook/Meta created it for exactly this reason).

## A GraphQL query
```graphql
query {
  user(id: "123") {
    name
    email
    orders(limit: 5) {
      id
      total
      items {
        productName
      }
    }
  }
}
```
One request retrieves exactly this nested shape — no more, no less — where the equivalent REST approach might require 3 separate round-trips (`GET /users/123`, `GET /users/123/orders`, `GET /orders/.../items` per order) or a custom, bespoke "backend for frontend" endpoint.

## Schema-first design
GraphQL APIs are defined by a strongly-typed schema:
```graphql
type User {
  id: ID!
  name: String!
  email: String!
  orders: [Order!]!
}

type Order {
  id: ID!
  total: Float!
  items: [OrderItem!]!
}

type Query {
  user(id: ID!): User
}
```
This schema is both documentation AND a runtime contract — client and server agree on exact types, and tools can auto-generate client code and documentation from it.

## Resolvers (the server-side implementation)
```javascript
const resolvers = {
  Query: {
    user: async (_, { id }) => await User.findById(id),
  },
  User: {
    orders: async (user) => await Order.findByUserId(user.id),
  },
};
```
Each field has a "resolver" function — GraphQL calls only the resolvers needed to satisfy the specific fields requested in a given query.

## REST vs GraphQL — the real trade-off
| | REST | GraphQL |
|---|---|---|
| Data shape | Fixed per endpoint | Client-defined per request |
| Over/under-fetching | Common issue | Solved by design |
| Caching | Simple (HTTP caching works naturally) | Harder (needs specialized client-side caching, e.g. Apollo Client) |
| Learning curve | Lower, more universally understood | Higher, more moving parts (schema, resolvers, N+1 resolver problem) |
| Best for | Simple, well-defined resource APIs | Complex, nested data needs; multiple client types (web/mobile) with different data needs |

## The GraphQL N+1 problem (a real, common pitfall)
Naively resolving `orders` for each of 100 users individually triggers 100 separate database queries — solved via a **DataLoader** pattern, which batches and deduplicates these into efficient bulk queries, directly analogous to the SQL N+1 problem in `03-Databases/SQL-Fundamentals.md`.

## In backend engineering
GraphQL isn't a strict "better than REST" — it's a different trade-off, best suited to complex, client-driven data needs (e.g., a mobile app and web app needing different subsets of the same underlying data). Many real systems use REST for simple CRUD services and GraphQL specifically for aggregating/orchestrating data across multiple internal services for frontend consumption.
