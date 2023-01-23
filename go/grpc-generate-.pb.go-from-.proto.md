---
description: Generate .pb.go automatically using .proto file in Windows
---

# \[gRPC] Generate .pb.go from .proto

For example, let's assume that we have following .proto written

```protobuf
syntax = "proto3";
package v1.k8s;
option go_package = "./nodes";

service Nodes {
  rpc GetNodes(GetNodesRequest) returns (GetNodesResponse);
}

message NodeInfoMessage {
  string name = 1;
  string version = 2;
  string address = 3;
  string osImage = 4;
  string ready = 5;
}

message GetNodesRequest {}

message GetNodesResponse {
  repeated NodeInfoMessage nodes = 1;
}
```

In order for us to use this `nodes.proto`, we need to have `nodes.pb.go` file. We can achieve that by following instruction under Windows 10 environment:

> I am assuming that you have your GOPATH and all PATH environment variables set correctly.

### 1. Install Protocol Buffers

1. Download your Protocol Buffers from [https://github.com/protocolbuffers/protobuf/releases](https://github.com/protocolbuffers/protobuf/releases). Look for Windows binary, for example it looks like `protoc-21.12-win64.zip`.&#x20;
2. Extract `.zip` and set `PATH.`



### 2. Install Plugins

We need some things like `protoc-gen-*` to generate grpc stuff

```bash
$ go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
$ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```



### 3. Generate .pb.go

When you have set your `go_package` as following:

```protobuf
option go_package = "./nodes";
```

This will generate your `nodes_grpc.pb.go` file under `./nodes` directory. To do this, execute following command in the same directory where your `nodes.proto` resides in:

```
protoc -I . --go-grpc_out=. nodes.proto
```

This will generate the `.pb.go` file for gRPC

<details>

<summary>Compare</summary>



</details>

\
