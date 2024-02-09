A reference is like a pointer in that it’s an address we can follow to access the data stored at that address; that data is owned by some other variable. Unlike a pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference.

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```
_In the code above, we are passing a reference to caculate_length which explicitly states that it takes a reference `String` and returns a usize._

References are defined in Rust using the ampersand. References allow you to refer to some value without actually taking ownership of it.

This is what a reference looks like:
![[Pasted image 20240209001401.png]]
_Note: The opposite of referencing by using `&` is dereferencing, which is accomplished with the dereference operator, `*`_

```rust
    let s1 = String::from("hello");

    let len = calculate_length(&s1);
```
_From the example above, the &s1 syntax lets us create a reference that refers to the value of s1 but does not own it. Because it does not own it, the value it points to will not be dropped when the reference stops being used._

The action of creating a reference is known as __borrowing__.

References are immutable by default. to make them mutable, we must do the following:
```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
_Both the original value and the borrowed value must be declared as mutable._

Mutable references have one big restriction: if you have a mutable reference to a value, you can have no other references to that value. This code that attempts to create two mutable references to s will fail:
```rust
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2);

```

The restriction preventing multiple mutable references to the same data at the same time enables controlled mutation. The benefit is that Rust can prevent data races at compile time. A __data race__ is similar to a race condition and happens when these three behaviors occur:
- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data.

We can use curly brackets to create a new scope, allowing for multiple mutable references, just not simultaneous ones:
```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;
```

Rust enforces a similar rule for combining mutable and immutable references. This code results in an error:
```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    let r3 = &mut s; // BIG PROBLEM

    println!("{}, {}, and {}", r1, r2, r3);
```
_We also cannot have a mutable reference while we have an immutable one to the same value._

Users of an immutable reference don’t expect the value to suddenly change. However, multiple immutable references are allowed because no one who is just reading the data has the ability to affect anyone else’s reading of the data.

Note that a reference’s scope starts from where it is introduced and continues through the last time that reference is used. 

For instance, this code will compile because the last usage of the immutable references, the `println!()`, occurs before the mutable reference is introduced:
```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```
_The scopes of the immutable references r1 and r2 end after the `println!` where they are last used, which is before the mutable reference r3 is created. These scopes don’t overlap, so this code is allowed: the compiler can tell that the reference is no longer being used at a point before the end of the scope._