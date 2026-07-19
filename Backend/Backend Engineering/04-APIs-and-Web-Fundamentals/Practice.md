# Practice — APIs and Web Fundamentals

1. Use `curl` (or Postman) to manually construct and send a GET, POST, and DELETE request against any public test API (e.g., `jsonplaceholder.typicode.com`) — inspect the raw response headers and status codes.
2. Design (on paper) a RESTful set of endpoints for a simple blogging platform: posts, comments, and authors — including nested resources and pagination query parameters.
3. Build a minimal REST API (Express.js, Flask, or your framework of choice) with at least 3 endpoints (list, get-by-id, create), using correct status codes (200, 201, 404, 400).
4. Write a GraphQL schema (even without implementing resolvers) for the same blogging platform, and write one example query fetching a post with its author and comments in a single request.
5. Add basic input validation to your REST API's create endpoint, returning a clear 400 error with a consistent error shape for invalid input.
6. Design a versioning strategy (URL-based) for your API — implement a `/api/v1/` prefix, and sketch out what a hypothetical `/api/v2/` breaking change might look like.
7. Explain in your own words: when would you choose GraphQL over REST for a new project, and when would REST clearly be the simpler, better choice?

✅ Done? Move to `05-Authentication-and-Security`.
