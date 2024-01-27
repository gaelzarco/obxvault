Rendering computer graphics with `Rust`, `Metal` and `Vulkan` on Apple hardware.

`Vulkan` is a graphics API built to integrate with modern and future graphics cards. APIs such as OpenGL were built for hardware from 25 years ago. OpenGL or Direct3D are better for game development as they provide higher abstractions and are less verbose whereas `Vulkan` is intended for high performance computer graphics.

To get started with Rust and `Vulkan` you need a graphics card that is compatible with the graphics API such as AMD, NVIDIA, or Intel. However, there exists `MoltenVK` that is directly compatible with Apple's `Metal` graphics framework. This enables `Vulkan` to run on all modern hardware including the latest Apple silicon chipsets.

### Table of Contents
- [Metal]()
- [MoltenVK]()

### Metal

To begin using Rust and Metal, you need a compatibility layer between the Rust programming language and Apple's Metal framework. This is because Metal is written with Objective-C and Swift. This is where the create [`metal-rs`](https://github.com/gfx-rs/metal-rs) comes in handy. This crate exposes unsafe Rust bindings for Metal.

Under the hood, `metal-rs` is powered by three main dependencies.

`rust-objc` and `rust-block` and `foreign-types`.

These dependencies are used to communicate with the Objective-C's runtime in order to allocate, de-allocate and call methods on the classes and objects that power Metal applications.

At the time of writing this, the documentation for this crate does not build and the Wiki provided is missing content to say the least. This will be revisited at a later date as the project contributors flesh out the documentation.

### MoltenVK

MoltenVK is a implementation of Vulkan graphics, that is built on Apple's Metal graphics for macOS, iOS, tvOS, and visionOS.

MoltenVK allows you to use Vulkan to develop graphical games and applications that run across many platforms, including macOS, iOS, tvOS, visionOS, Simulators, and Mac Catalyst on macOS 11.0+, and all Apple architectures, including Apple Silicon.

Metal uses a different shading language, the Metal Shading Language (MSL), than Vulkan, which uses SPIR-V.

The [Vulkan SDK](https://vulkan.org/) includes a MoltenVK runtime library for macOS.