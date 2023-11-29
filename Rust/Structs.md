### IN PROGRESS
### [RUST: STRUCTS (5:30)](https://www.youtube.com/watch?v=MDT9vNjtGsY&list=PLAJ-sYO1aGdxQ_skPPtJ7PlSAjTXM-atv&index=7)

Common data structure that allow you lto group up related values that represent something meaningful in your programs.

Structs are similar to Tuples
- Tuples have multiple elements that may be different types.
- Structs have multiple elements that may be different types and that posses different names for them.

Here is an example `User` struct:
```rust
// Similar to JavaScript Objects/Classes
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
// Can be delcared as mutable
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("xmpl1"),
        active: true,
        sign_in_count: 1,
    };
}
```

Here is a helper function to create a new user in a streamlined way:
```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

Rust is also smart enough to fill in the gaps with JavaScript's spread operator equivalent when filling the gaps for other User instances:
```rust
fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("xmpl1"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@eaxmple.com"),
        ..user1
    };
}
```
_Be careful as to not switch the order of the property you want to change and the rest of the fields being filled in. This will end up replacing the previously populated fields from the `..user1`._

If you attempted to run this code:
```rust
fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("xmpl1"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@eaxmple.com"),
        ..user1
    };

    println!("user1: {}", user1.username);
    println!("user1: {}", user1.email);
}
```

The borrow checker in Rust will give you the error that the `user1.username` value was borrowed after it had already been moved; `user2` now has ownership of that value.

However, in a sneaky way, Rust `bool` and `u64` types implement the copy trait which means those values in `user1`, after being used to populate `user2`, can still be accessed from the `user1` var.