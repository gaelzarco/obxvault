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

Although this accepts incoming requests, we are not actually sending anything to the client. For that, we need more functionality.

```rust
use std::{
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();

    let status_line = "HTTP/1.1 200 OK";
    let contents = fs::read_to_string("hello.html").unwrap();
    let length = contents.len();

    let response =
        format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");

    stream.write_all(response.as_bytes()).unwrap();
}
```
_Here we use `BufReader` to convert our `TcpStream` into a string that we can then read/write to. From there, we call the `lines` method to create an iterator that we can map over. On each iteration, we create a closure that takes in a `result` and returns if it is not an error. We do this because there could be a problem reading the stream, such as the data not being UTF-8 encoded. 
`map` returns an iterator of text that we then call `take_while` to map over again. We then create a closure takes elements from the iterator until the closure `|line|` `!line.is_empty` returns `false`. In other words, it stops taking elements when it encounters an empty line.
This function returns a closure and finally, we call `collect` because it collects the elements into a `Vec<_>`, which is a vector of elements with an inferred type._ 

We are now passing each stream, now that we verified that they are not empty, to the `handle_connection` function. This function takes in a `TcpStream` and returns an OK `HTTP `response to the client. This response include an `HTML` file along with a specified `Content-Length` and `HTTP` status. This is all formatted as `CRLF` (carriage return, line feed).