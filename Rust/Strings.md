There are two main types of strings in Rust. The first type are string literals and the second are `String`.
- String literals are fixed in size and are pushed onto the heap. They cannot be mutated because their size in specified during compilation.
- The `String` type can be mutated and changed during runtime because they are not fixed in size
	- They are stored in the heap and can be mutated with functions such as `push_str`.

There are two things that we need to do in order to allocate memory when using the `String` type.
1. The memory must be requested from the memory allocator at runtime.
2. We need a way of returning this memory to the allocator when we’re done with our String.

The first step is handled by us by calling `String::from`, its implementation requests the memory it needs.

The second part is handled by Rust's borrow checker.

Beyond this, there are also string slices which are referenced to part of a string.
- We create slices using a range within brackets by specifying [`starting_index..ending_index`]
	- Under the hood, slices contain a pointer to the byte at the index of the starting position with a length of the difference between the ending_index & starting_index.
- Take this example:
```rust
 let s = String::from("hello world");
 
let hello = &s[0..5];
let world = &s[6..11];
```

the `world` variable would look something like this under the hood:
![[Pasted image 20240211191029.png]]

Using Rust's range syntax, these values are equivalent:
```rust
let slice = &s[0..2];
let slice = &s[..2];
```

```rust
let slice = &s[3..len];
let slice = &s[3..];
```

```rust
let slice = &s[0..len];
let slice = &s[..];
```

### Understanding slices know, we can understand string literals. 

The type of s here is &str: it’s a slice pointing to that specific point of the binary. This is also why string literals are immutable; &str is an immutable reference.
```rust
let s = "Hello, world!";
```

Knowing that you can take slices of literals and String values leads us to one more improvement to string functions, and that’s their signature:
```rust
fn first_word(s: &String) -> &str {
```

A more experienced Rustacean would write the signature shown in the listing below instead because it allows us to use the same function on both `&String` (string reference) values and `&str` (string slice) values:
```rust
fn first_word(s: &str) -> &str {
```
_Improving the first_word function by using a string slice for the type of the s parameter If we have a string slice, we can pass that directly. If we have a String, we can pass a slice of the String or a reference to the String. __

Defining a function to take a string slice instead of a reference to a String makes our API more general and useful without losing any functionality:
```rust
fn main() {
    let my_string = String::from("hello world");

    // `first_word` works on slices of `String`s, whether partial or whole
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
    // `first_word` also works on references to `String`s, which are equivalent
    // to whole slices of `String`s
    let word = first_word(&my_string);

    let my_string_literal = "hello world";

    // `first_word` works on slices of string literals, whether partial or whole
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);

    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}
```
