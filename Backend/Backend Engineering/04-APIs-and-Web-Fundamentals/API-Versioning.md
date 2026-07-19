# API Versioning

## What is it?
Strategies for evolving an API's behavior/shape over time without breaking existing clients who depend on the current behavior.

## Why care?
Once an API has real external consumers (other teams, third-party developers, mobile apps you can't force-update instantly), you cannot simply change behavior — a breaking change silently breaks every integration relying on the old contract. Versioning is how you evolve safely.

## Common versioning strategies

### URL path versioning (most common, most explicit)
```
GET /api/v1/users/123
GET /api/v2/users/123
```
Pros: extremely clear and simple, easy to route at the infrastructure level (e.g., different load balancer rules per version). Cons: can lead to significant code duplication between versions if not managed carefully.

### Header-based versioning
```
GET /api/users/123
Accept: application/vnd.myapi.v2+json
```
Pros: keeps URLs clean/stable. Cons: less discoverable/obvious, harder to test casually via a browser URL bar.

### Query parameter versioning
```
GET /api/users/123?version=2
```
Less common in serious production APIs — easy to forget/omit, defaults can be ambiguous.

## What actually counts as a "breaking change"
```
Breaking:      Removing a field, renaming a field, changing a field's type,
               changing an endpoint's URL, changing required-ness of an input field,
               changing error response structure.

Non-breaking:  Adding a new OPTIONAL field to a response, adding a new endpoint,
               adding a new OPTIONAL request parameter.
```
A well-designed client should ignore unknown fields in a response — this convention ("clients must tolerate unknown fields") is what makes ADDING fields safely non-breaking.

## Deprecation strategy (versioning is only half the story)
1. Announce deprecation of the old version with a clear timeline (e.g., "v1 will be sunset in 6 months").
2. Add a `Deprecation` / `Sunset` HTTP header to responses from the deprecated version, so automated tooling/clients can detect it programmatically.
3. Monitor actual usage of the deprecated version before removing it — don't assume everyone has migrated just because you announced it.
4. Provide clear migration documentation (what changed, how to update client code).

## In backend engineering
- **Internal APIs (within one company, one team controls all clients)** can sometimes get away with looser versioning discipline (coordinate a synchronized deploy) — but this breaks down fast as an organization grows past a handful of teams.
- **Public/partner-facing APIs** need strict versioning discipline — you often can't even see all your consumers, let alone coordinate a synchronized change with them, making backward compatibility a hard requirement, not a nice-to-have.
- **GraphQL's approach differs**: instead of versioning the whole API, GraphQL favors evolving the schema by adding new fields and deprecating (but not immediately removing) old ones (`@deprecated` directive) — a more granular, field-level versioning philosophy.
