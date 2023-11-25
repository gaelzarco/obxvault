### Mutual Exclusion (Mutex)

Allows one thread to access data at a time.
- A thread asks for access to the data by acquiring a lock.
	- A lock keeps track of who has access to the data.
- __A `Mutex` guards access via this lock system__

The main rules for `Mutex` are as follows:
- You must attempt to acquire the lock before you access the data.
- After accessing the data that is being guarded, you must lock the data so other threads can access it.
  
Here is a simple example of how `Mutex` works:
```rust
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);
    {
        // use the 'lock()' to acquire a lock
        // here, num is of type MutexGuard<i32>
        let mut num = m.lock().unwrap();
        // You must derefernce the value in order to mutate it
        // get inside the point using the '*' dereference
        // Assign it to a new value
        *num = 6;
    }

    // Now that the MutexGuard is out of scope, the Mutex is locked again
    // and can be accessed by other threads
    println!("m = {:?}", m);
}

// Output: m = Mutex { data 6, poisoned: false, .. }
// Implements debug trait by default
```

Here is `Mutex` shared between multiple threads:
```rust
use std::sync::Mutex;
use std::thread;

fn main() {
    let counter = Mutex::new(0);
    // Vectors are mutable arrays
    let mut handles = vec![];

	// Instead of creating a new scope, we instead use a for loop
	// However, there is an issue in this closure
	// We are attempting to take ownership of whats inside because
	// the counter does not implement the .clone() trait
    for _ in 0..10 {
    // Spawn threads
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        // Take that handle and push into vector
        handles.push(handle);
    }
    
    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

There is an issue with the code above. We already moved `counter` into the closure once and because we are spawning a new thread and looping through the code, on the next iteration, we are unable to move it into the next closure.

In order to lift up some of the ownership rules in Rust, we can use `Rc` (Reference Count).

```rust
use std::rc::Rc;
use std::sync::Mutex;
use std::thread;

fn main() {
    let counter = Rc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Rc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

However, the code above also produces an error which boils down to the fact that `Rc` is not thread safe.

The correct way to implement `Rc` is available through the sync module and that is known as `Arc` (Atomic Reference Count). `Arc` works similarly to `Rc` but is suited to function with `Mutex`.

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

This code will run as expected and is now error free. 

There are caveats:
- If you use Mutex, you are at risk of encountering deadlocks. 
	- Deadlocks occur when two or more threads are each waiting for a resource held by the other. With `Mutex`, this can happen if a thread tries to acquire locks in a different order than another thread.
	- Rust does not prevent deadlocks from occurring through the type system like some other languages. It is possible to write Rust code that deadlocks.
	- Using multiple `Mutex` within the same thread increases the risk, as the thread order in acquiring locks could lead to a deadlock.
	- Recursively locking the same `Mutex` (locking a `Mutex` while already holding its lock) will cause a deadlock.
	- High contention on `Mutex` with many threads constantly locking/unlocking increases the chances of a deadlock occurring.