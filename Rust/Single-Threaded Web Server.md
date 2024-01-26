The two main concepts involved in web servers are _TCP (Transmission Control Protocol)_ and _HTTP (Hypertext Transfer Protocol)_. Both are _request-response_ protocols.

TCP is the lower-level protocol that describes the details of how information gets from one server to another but doesn’t specify what that information is. HTTP builds on top of TCP by defining the contents of the requests and responses. It’s technically possible to use HTTP with other protocols, but in the vast majority of cases, HTTP sends its data over TCP. We’ll work with the raw bytes of TCP and HTTP requests and responses.

Here is an example of single-threaded HTTP server:
```rust
use std::net::TcpListener;

fn main() {
    println!("Simple HTTP server built for Rust from scratch.");
    const HOST: &str = "127.0.0.1";
    const PORT: &str = "5001";

    let endpoint: String = HOST.to_owned() + ":" + PORT;

    // Listner for incoming TCP streams
    let listener = TcpListener::bind(endpoint.clone()).expect("Endpoint is not valid");

    for stream in listener.incoming() {
        let stream = stream.expect("Incoming stream dropped");

        println!("{endpoint} pinged!");
    }
}
```
_In networking, we use the term bind because we are connecting to a port to listen to. Otherwise, the bind function in the code above creates a new instance of TcpListener_

