What is it?
- a remote procedure call framework
- allows us to expose functionality in our app to other apps
- action-oriented (GetUser(id)) VERSUS RESTful resource-oriented GET /users/id
- HTTP2
- Agnositc definitions via protocol buffers
- code generation for wide-variety of languages via protoc

How does it benefit our systems?
- gRPC is great for service-to-service communication within a microservices architecture where low latency and high throughput are crucial. Its use of HTTP2 and protocol buffers lead to faster serialization and deserialization and smaller message sizes compared to REST with JSON
- Support for bidrectional streaming, making it ideal for apps requiring real-time data exchange (chat, live dashboard, IoT devices)
- REST is more for public-facing APIs (like the Spotify API, etc) 


**Key Differences Summarized:**

| Feature               | gRPC                                    | REST                                       |
| --------------------- | --------------------------------------- | ------------------------------------------ |
| **Protocol**          | HTTP/2                                  | HTTP/1.1 (primarily), HTTP/2 supported     |
| **Data Format**       | Protocol Buffers (binary)               | JSON (primarily), XML, others              |
| **Communication**     | RPC (Remote Procedure Call), Streaming  | Resource-based, Request-Response           |
| **Performance**       | Generally higher                        | Depends on implementation, can be lower    |
| **Browser Support**   | Limited (requires gRPC-Web)             | Excellent                                  |
| **Code Generation**   | Built-in                                | Often requires external tools              |
| **Contract**          | Strong (via `.proto` files)             | Convention-based                           |
| **Human Readability** | Low                                     | High (for JSON/XML)                        |
| **Use Cases**         | Microservices, real-time, internal APIs | Public APIs, web applications, simple CRUD |
When to use it vs RESTful API?
- 


in Go



##### .proto files
- Protocol buffer definition files used in gRPC to define the structure of data and the services (RPC methods) that can be called. 
- They serve as the contract between the client and server in a gRPC system. 

```
syntax = "proto3" // specifies the protobuf version (proto3 is the latest)

service = "MyService" { // defines a gRPC service named "MyService"
	rpc GetRate(RateRequest) returns (RateResponse); // RPC method with request and response types
}

message RateRequest { // Defines the structure of the request message.
	string Base = 1; // Field 1: Base currency (USD, etc)
	string Destination = 2; // Field 2: Destination currency (EUR, CAD, etc)

}

message RateResponse { // Defines the structure of the response message.
	float Rate = 1; // Field 1: Exchange rate as a floating-point number
}
```