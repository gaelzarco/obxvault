Thread pools are the foundation of multi-threaded Rust applications. A _thread pool_ is a group of spawned threads that are waiting and ready to handle a task.

When the program receives a new task, it assigns one of the threads in the pool to the task, and that thread will process it. The remaining threads in the pool are available to handle any other tasks that come in while the first thread is processing. Thread pools are concurrent by nature.

You should always limit the amounts of threads available to protect against DOS (Denial of Service) attacks. If a program created a new thread for each request it receives, it leaves itself open to a malicious user opening thousands of request at once. This would bring our server's response time to a halt.

Limiting the number of threads is just one of the many ways to improve a web server. Other ways of improving a web server include _fork/join model_, the _single-threaded async I/O model_, or the _multi-threaded async I/O model_.

In this example, we will use compiler-driven development. We’ll write the code, and then we’ll look at errors from the compiler to determine what we should change next to get it to work. Before we do that, however, we’ll explore the technique a separate technique first.

First, let’s explore how our code might look if it did create a new thread for every connection:
```rust
fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        thread::spawn(|| {
            handle_connection(stream);
        });
    }
}
```
_This piece of code creates a new thread for each request. This is unsafe and leaved us vulnerable to attacks._

To implement a finite number of threads, we'll create a struct, `ThreadPool`, to define a specific number of available threads

Then edit main.rs file to bring ThreadPool into scope from the library crate by adding the following code to the top of src/main.rs:

Filename: src/main.rs
```rust
use hello::ThreadPool;
```
This code still won’t work, but let’s check it again to get the next error that we need to address:

```rust
$ cargo check
    Checking hello v0.1.0 (file:///projects/hello)
error[E0599]: no function or associated item named `new` found for struct `ThreadPool` in the current scope
  --> src/main.rs:12:28
   |
12 |     let pool = ThreadPool::new(4);
   |                            ^^^ function or associated item not found in `ThreadPool`

For more information about this error, try `rustc --explain E0599`.
error: could not compile `hello` due to previous error
```