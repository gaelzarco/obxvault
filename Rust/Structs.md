
A struct, or structure, is a custom data type that lets you package together and name multiple related values that make up a meaningful group. If you’re familiar with an object-oriented language, a struct is like an object’s data attributes

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

Rust is also smart enough to fill in the gaps with JavaScript's spread operator equivalent when filling the gaps for other User instances (this is known as struct update syntax):
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

To get a specific value from a struct, we use dot notation. If the instance is mutable, we can change a value by using the dot notation and assigning into a particular field:
```rust
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
```
_Note that the entire instance must be mutable; Rust doesn’t allow us to mark only certain fields as mutable_

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

Also, Structs may be defined using the field init shorthand like this:
```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
```
_The name of the arg taken by the function must match the name of the field in the struct._

__In the User struct definition we used the owned String type rather than the &str string slice type. This is a deliberate choice because we want each instance of this struct to own all of its data and for that data to be valid for as long as the entire struct is valid.__

__It’s also possible for structs to store references to data owned by something else, but to do so requires the use of lifetimes.Lifetimes ensure that the data referenced by a struct is valid for as long as the struct is.__

### Tuple Structs

These are structs that borrow the `Struct` keyword to create structs that look like tuples. They take in nameless fields are and useful to create tuples of varying types where the field names would be redundant.

For example:
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```


### Unit Structs

Unit-like structs can be useful when you need to implement a trait on some type but don’t have any data that you want to store in the type itself.
```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

![[Pasted image 20240213225101.png]]