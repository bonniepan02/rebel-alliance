In gRPC, a client applicatin can directly call a method on a server application on a different machine as if it were a local object, making it easier for you to create distributed applications and services.

As in many RPC sytems, gRPC is based around the idea of defining a service, specifying the methods that can be called remotely with their parameters and return types.

gRPC use protocol buffers as both its Interface Definition Language(IDL) and as its underlying message interchange format. Protocol Buffer is google's mature open source mechanism for serializing structured data.

Protocol buffer data is structures as messages, where each messge is a small logical record of information containing a series of name-value pairs called fields.

Unary RPCs where the client sends a single request to the server and gets a single response back, just like a normal functional call.

Server streaming RPCs
Client streaming RPCs
Bidirectional streaming RPCs

Reference:

1. https://grpc.io/docs/guides/
2. https://grpc.io/docs/tutorials/basic/python/
