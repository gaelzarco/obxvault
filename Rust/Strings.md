There are two main types of strings in Rust. The first type are string literals and the second are `String`.
- String literals are fixed in size and are pushed onto the heap. They cannot be mutated because their size in specified during compilation.
- The `String` type can be mutated and changed during runtime because they are not fixed in size
	- They are stored in the heap and can be mutated with functions such as `push_str`.

There are two things that we need to do in order to allocate memory when using the `String` type.
1. The memory must be requested from the memory allocator at runtime.
2. We need a way of returning this memory to the allocator when we’re done with our String.

The first step is handled by us by calling `String::from`, its implementation requests the memory it needs.

The second part is handled by Rust's borrow checker.