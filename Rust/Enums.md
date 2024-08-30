_Enums_ got their name because they are the enumeration of all possible variants.

IP addresses are a great example of when to use Enums because code needs to account for both IPV4 addresses and IPV6 addresses when handling them in network communication.
- They should fundamentally be treated the same.

Example IP enum:
```rust
enum IpAddrKind {
 V4,
 V5,
}
```

Enums also allow you to insert data directly into each enum variant and can save you the trouble of implementing a struct and defining an address type:
```rust
// Enums are cool[[NeoVim]]
#[derive(Debug)]
enum IpAddrKind {
    V4(String),
    V6(String),
}

fn route(ip_type: IpAddrKind) {
    println!("{:?}", ip_type);
}

fn main() {
    let four = IpAddrKind::V4(String::from("127.0.0.1"));
    let six = IpAddrKind::V6(String::from("::1"));

    route(four);
    route(six);
}
```