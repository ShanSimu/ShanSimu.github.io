## Protocol Buffers

Google’s mature open source mechanism for serializing structured data

By default, gRPC uses [protocol buffers](https://developers.google.com/protocol-buffers) as the Interface Definition Language (IDL) for describing both the service interface and the structure of the payload messages. It is possible to use other alternatives if desired.

也可以用JSON替代

举例：

```protobuf
message Person {
  string name = 1;
  int32 id = 2;
  bool has_ponycopter = 3;
}
```

protocol buffer编译器可以按定义生成不同语言的类，方便开发者使用

gRPC可以将protocol buffer message定义为参数和返回值

```protobuf
// The greeter service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

## gRPC

### Service definition

Like many RPC systems, gRPC is based around the idea of defining a service, specifying the methods that can be called remotely with their parameters and return types.

gRPC lets you define four kinds of service method:

```protobuf
rpc SayHello(HelloRequest) returns (HelloResponse);
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse);
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse);
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse);
```

stream means a sequence of messages

### Using the APIs

gRPC users typically call these APIs on the client side and implement the corresponding API on the server side.

### Synchronous vs. asynchronous

The gRPC programming API in most languages comes in both synchronous and asynchronous flavors.