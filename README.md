# Tellurium Library for .NET 4.7.1 version 1.5.4
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2F0xF6%2FTellurium.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2F0xF6%2FTellurium?ref=badge_shield)


A simple, easy to use, strongly-typed wrapper around .NET named pipes.

# Features

*  Create named pipe servers that can handle multiple client connections simultaneously.
*  Send strongly-typed messages between clients and servers: any serializable .NET object can be sent over a pipe and will be automatically serialized/deserialized, including cyclical references and complex object graphs.
*  Messages are sent and received asynchronously on a separate background thread and marshalled back to the calling thread (typically the UI).
*  Supports large messages - up to 300 MiB.

# Requirements

Requires .NET 4.7.1 full.


# TODO

* Security managmend + attrubutes strong security
* Security and Access Rights
* Rijdael support
* .NET Core support (Linux NamedPipe and Mac OS)
* JSON, YAML and custom serializer

# Usage

Server:

```csharp
var server = new NamedPipeServer<SomeClass>("MyServerPipe");

server.ClientConnected += delegate(NamedPipeConnection<SomeClass> conn)
    {
        Console.WriteLine("Client {0} is now connected!", conn.Id);
        conn.PushMessage(new SomeClass { Text: "Welcome!" });
    };

server.ClientMessage += delegate(NamedPipeConnection<SomeClass> conn, SomeClass message)
    {
        Console.WriteLine("Client {0} says: {1}", conn.Id, message.Text);
    };

// Start up the server asynchronously and begin listening for connections.
// This method will return immediately while the server runs in a separate background thread.
server.Start();

// ...
```

Client:

```csharp
var client = new NamedPipeClient<SomeClass>("MyServerPipe");

client.ServerMessage += delegate(NamedPipeConnection<SomeClass> conn, SomeClass message)
    {
        Console.WriteLine("Server says: {0}", message.Text);
    };

// Start up the client asynchronously and connect to the specified server pipe.
// This method will return immediately while the client runs in a separate background thread.
client.Start();

// ...
```

# MIT License

Named Pipe Wrapper for .NET is licensed under the [MIT license](LICENSE.txt).


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2F0xF6%2FTellurium.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2F0xF6%2FTellurium?ref=badge_large)