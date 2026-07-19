# gRPC

## What is it?
gRPC is a high-performance Remote Procedure Call (RPC) framework built on HTTP/2 and Protocol Buffers (a compact binary serialization format) — designed for fast, efficient service-to-service communication, particularly common in microservices architectures.

## Why care?
While REST/JSON dominates public-facing APIs, gRPC is extremely common for INTERNAL service-to-service communication in larger backend systems, because of its performance and strongly-typed contracts — worth knowing, especially for microservices-heavy roles.

## Protocol Buffers (protobuf) — the contract
```protobuf
syntax = "proto3";

service UserService {
  rpc GetUser (GetUserRequest) returns (User);
}

message GetUserRequest {
  string user_id = 1;
}

message User {
  string id = 1;
  string name = 2;
  string email = 3;
}
```
This `.proto` file is compiled into strongly-typed client and server code in virtually any language (Go, Python, Java, C#, etc.) — the contract is defined once, and code generation keeps client/server in sync, catching mismatches at compile time rather than runtime.

## Why gRPC is faster than REST/JSON
- **Binary format (protobuf)**: much more compact and faster to parse than text-based JSON.
- **HTTP/2**: supports multiplexing multiple requests over a single connection, and built-in streaming — REST/JSON traditionally rides on HTTP/1.1 (though HTTP/2 REST/JSON is possible too, gRPC is designed around HTTP/2 from the ground up).
- **Streaming support**: gRPC natively supports client-streaming, server-streaming, and bidirectional streaming — not just simple request/response, useful for real-time data feeds or large file transfers.

## REST vs gRPC — when to use which
| | REST/JSON | gRPC |
|---|---|---|
| Human-readable | Yes (JSON) | No (binary) |
| Browser support | Native | Limited (needs grpc-web proxy) |
| Performance | Good | Better (binary, HTTP/2) |
| Contract enforcement | Manual (OpenAPI optional) | Built-in (protobuf schema) |
| Best for | Public APIs, browser clients | Internal service-to-service calls, performance-critical paths |

## Example server (Python, simplified)
```python
class UserServicer(user_pb2_grpc.UserServiceServicer):
    def GetUser(self, request, context):
        user = db.get_user(request.user_id)
        return user_pb2.User(id=user.id, name=user.name, email=user.email)
```

## In backend engineering
- **Microservices communication** (`11-Microservices/Service-Communication.md`): gRPC is a very common choice for internal service-to-service calls specifically because of the strongly-typed contract (reduces integration bugs between teams) and performance benefits at scale.
- **Not typically exposed directly to browsers/public clients** — most systems use REST or GraphQL at the public "edge" and gRPC internally between backend services, translating at the boundary if needed (via an API Gateway, `11-Microservices/API-Gateway.md`).
