Rust does not abstract away the stack and the heap.

- The stack functions based on last-in, first-out basis (LIFO)
	- Adding (pushing) and removing (popping) data is sequential, and all stack data must have a known, fixed size.
- Data with unknown or dynamic size is stored on the heap.
	- The heap involves allocating space and receiving a pointer in return. The pointer's fixed size allows storage on the stack, but to access data, you follow the pointer.

Accessing heap data is slower due to pointer traversal, and processors perform better when working on data close together (stack). Function calls involve pushing values onto the stack, and when the function ends, these values are popped off.

Ownership in Rust addresses issues like tracking heap data usage, minimizing duplicates, and cleaning up unused data to prevent space exhaustion.

 In other programming languages, a _Garbage Collector_ keeps track of and cleans up memory that isn’t being used anymore. There are also programming languages that require the programmers to clear unused memory on their own explicitly. Rust does neither of these things.

Rust instead frees memory after the variable falls out of scope. When a variable goes out of scope, Rust calls `drop`. Its where the author of `String` can put the code to return the memory. Rust calls drop automatically at the closing curly bracket.

	In C++, this pattern of deallocating resources at the end of an item’s lifetime is sometimes called Resource Acquisition Is Initialization (RAII).

In C++, this pattern of deallocating resources at the end of an item’s lifetime is sometimes called Resource Acquisition Is Initialization (RAII).

```rust
    let s1 = String::from("hello");
    let s2 = s1;
```

In the code above, it appears as though the variable called s2 takes a cloned variable from s1 and assigns it to s2, however that is not what is happening.

A String is made up of three parts.

On the left: a pointer to the memory that holds the contents of the string, a length, and a capacity. This group of data is stored on the stack. 

On the right is the memory on the heap that holds the contents.

![[assets/Pasted image 20240121160756.png]]

The length is how much memory, in bytes, is used by `String`. The capacity is the total amount of memory, in bytes, that the `String` has received from the allocator. These are important distinctions

When we assign s1 to s2, the `String` data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack. We do not copy the data on the heap that the pointer refers to.
![[Pasted image 20240208211914.png]]
_This what memory would look like if Rust instead copied the heap data as well. If Rust did this, the operation s2 = s1 could be very expensive in terms of runtime performance if the data on the heap were large. Take a look below:_

![[Pasted image 20240208212233.png]]
_In regards to the image before this, to avoid any memory errors, Rust automatically calls the drop function on s1 because it moves the value to s2 which prevents any double free memory errors that could occur if two instances of the same heap memory were attempted to be cleared simultaneously from both s1 and s2 falling out of scope._

# THEY DON'T UNDERSTAND THE FOLLOWING LIKE I DO:

If you’ve heard the terms shallow copy and deep copy while working with other languages, the concept of copying the pointer, length, and capacity without copying the data probably sounds like making a shallow copy. But because Rust also invalidates the first variable, instead of being called a shallow copy, it’s known as a move. In this example, we would say that s1 was moved into s2. So, what actually happens is shown in Figure 4-4.
![[Pasted image 20240208213644.png]]

Rust never automatically creates deep copies of data. To manually create one, you must call the `.clone()` method.

```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```
_Heap data is copied in the figure above._