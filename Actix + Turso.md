
`Actix Web` is a back-end framework for `Rust`. Here is an example of a simple Rust back-end:

```rust
use actix_web::{get, post, web, App, HttpResponse, HttpServer, Responder};

#[get("/")]
async fn hello() -> impl Responder {
    HttpResponse::Ok().body("Hello world!")
}

#[post("/echo")]
async fn echo(req_body: String) -> impl Responder {
    HttpResponse::Ok().body(req_body)
}

async fn manual_hello() -> impl Responder {
    HttpResponse::Ok().body("Hey there!")
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
            .service(echo)
            .route("/hey", web::get().to(manual_hello))
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
``` 

### Turso is edge-hosted, distributed-database based on the open-source SQLite alternative, libSQL

- `libSQL` is an open-source fork of `SQLite`.
	- designed to minimize query latency for applications where queries come from anywhere in the world.
	- Pairs well with edge functions provided by platforms such as CloudFlare, Netlify, Vercel, etc.
- Runs on cheap commodity hardware for cost-savings and to be easily replaceable.
	- A.K.A. 'Far Edge Hardware'.
- Adds server mode `sqld` with HTTP and web-socket access.
	- SQL Daemon: _Daemon is program that runs continuously as a background process and wake up to handle periodic service requests which often come from remote processes. The daemon program is alerted to the request by the operating system; it either forwards that request to another program or responds to it itself._
- Adds replication between `sqld` instances
	- 1 primary database and `n` replicas.
	- Adds automatic configuration and management of `sqld` instances.
- Client libraries for `JavaScript/TypeScript`, `Rust`, `Python`
		- Adds `WASM` user defined functions.
- Adds a CLI for working with `sqld` instances.
- Adds managed, token-based authentication.