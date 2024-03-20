Introduction to the ways to organzise Rust crates and modules.

The crate root file: _src/lib.rs_ or _src/main.rs_ is the entry point for the program

When compiling crates:
- lib.rs_ = library crate
- _main.rs_ = binary crate
- Library crates do not need a main function to execute.
- Control access to a crate can be defined using keywords such as `pub` and `use`.
- Modules within crates can be defined using the `mod` keyword in the root file.

	__Modules__
	```rust
	// File: src/http.rs (can also be named src/http/mod.rs)
	mod protocol;
	```
	- Defined using `mod`.
	- The compiler searches for the module's code:
		1. Inline, within curly braces (replace semicolon).
		2. In the _src/protocol.rs_ file.
		3. In the _src/http/protocol.rs_ file
	
	__Submodules__
	Can be declared in any other file than the crate root.
	
	In this example, `mod ip;` is declared in the src/protocol.rs file. 
	
	The compiler will look for the submodule's code:
	- Inline, after following `mod protocol` in curly brackets instead of a semicolon.
	- In the _src/protocol/ip.rs_ file.
	- In the _src/protocol/ip/mod.rs_ file.

Once a module is a part of your crate, you can refer to it anywhere else in that same crate (as long as the privacy rules allow it) using the path to that part of the code.
	For example an `IPV4` type in the protocol ip module would be found at `crate::protocol::ip::IPV4`.

_Code in a module is private from parent by default._

Declare modules using `pub mod` instead of `mod` to make them public.

__Use Keyword__
Within a scope, the `use` keyword creates shortcuts to items to reduce repetition.

- `crate::protocol::ip::IPV4` can have `use` keyword attached to the beginning to then bring `IPV4` into scope.
	- `IPV4` could be called without the rest of the path then on after.

_Example binary crate_:
```rust
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

Modules enable the developer to organize code for easy re-use and control what is available to the code that uses them.

`cargo new <library_name> --lib` allows you to create modules.

Here is an example of a library crate:
```rust
mod ip {
    mod ipv4 {
        enum Ipv4 {
            Address(String),
        }
    }

    mod ipv6 {
        enum Ipv6 {
            Address(String),
        }
    }

    mod transfer {
        fn send_packet() {}

        fn receive_packet() {}
    }
}
```

The contents of any crate form a module called `crate` at the root of the crate's module structure.
- This is known as the __Module Tree__.
```rust
crate
 └── ip
     ├── ipv4
     │   ├── Ipv4
     └── ipv6
         ├── Ipv6
	 ├── transer
		 ├──send_packet
		 └── receive_packet
```

#### Paths
The path to call a piece of code from an item in a module tree.

A path can be:
- _Absolute_: using the specified crate's name or the current crate.
- _Relative_: starts at current module and uses _self_, _super_ or an identified in the current module.

Identifiers for paths are separated using `::` (double colons).

To demonstrate paths, we can use this example program to demonstrate:
```rust
mod ip {
    pub mod ipkind {
        #[derive(Debug)]
        pub enum IpKind {
            V4(String),
            V6(String),
        }
    }

    pub mod transfer {
        use super::ipkind::IpKind;

        pub fn send_packet(sending: IpKind, receiving: IpKind, message: String) {
            println!(
                "Message from sender: {:?} was sent to {:?}",
                sending, receiving
            );

            receive_packet(sending, receiving, message);
        }

        pub fn receive_packet(sending: IpKind, receiving: IpKind, message: String) {
            println!("{:?} has received a message from {:?}.", receiving, sending);
            println!("Message reads: {message}");
        }
    }
}

pub fn communicate() {
    use crate::ip::{ipkind::IpKind, transfer::send_packet};

    let sending = IpKind::V4(String::from("127.0.0.1"));
    let receiving = IpKind::V6(String::from("::1"));

    send_packet(sending, receiving, String::from("Hey!"));
}
```

In simple terms:
- When using a separate module under the same parent module, super can be used as a relative path to the module that is needed.
- Items, functions and other items can be called directly using the entire path (relative or absolute).
	- These can also be called by bringing them into scope with the `use` keyword.
- To use a module from a certain path, ensure the module and the items that need to be exposed are public using the `pub` keyword.
- Paths to modules that share parent modules can be brought under the same `use` instance by