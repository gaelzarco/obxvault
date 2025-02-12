## Chapter 2
### 2.1 Architecture
The pipeline stages execute in parallel with each stage dependent upon the result of the previous stage.

Four main stages:
1. Application
2. Geometry Processing
3. Rasterization
4. Pixel Processing

Rendering speed expressed in __frames per second (FPS)__, __Hertz (Hz)__, or __milliseconds (ms)__.

#### Application Stage

Typically implemented in software running on CPUs.
- Process _threads_ of execution in parallel.

Tasks tun on CPU typically include:
- Collision detection
- Global acceleration algorithms
- Physics simulation
- Many others

#### Geometry Processing
- Transforms
- Projections
- Etc.

What is to be drawn, how, where.

Performed on the GPU.

__Graphics Processing Unit__:
- Contains programmable cores
- Fixed-operation hardware

#### Rasterization
Takes input verices to form a triangle and finds all pixels within that triangle.
- This is forwarded to the next stage.

Performed on the GPU.

#### Pixel Processing
Executes a program per pixel to determine color.

May perform depth testing to see if pixels are visible or not.

Performs per-pixel operations such as blending newly computed color with a previous color.

Performed on the GPU.

### 2.2 The Application Stage

Developer has full control since it executes on CPU.

Some work can be performed by GPU using a separate mode called __Compute Shader__.
- Treats GPU as a highly parallel general processor.
- Ignores special functionality specifically for graphics rendering.

Output of this stage is fed into geometry processing stage.
- This is made up of __Rendering Primatives__, i.e., points, lines, and triangles
- Most important task of this stage.

A consequence of software-based implementation is that it is not divided into substages.
- Unlike subsequent steps.

Performance is increased by executing processes in parallel on several processor cores.
- In CPU design, this is called a __Superscaler__ construction because it executes processes at the same time and in the same stage.

__Collision Detection__ is implemented in this stage.
- After objects collide, a response is returned to the colliding objects and a force feedback device.

Handles input from other sources such as keyboard, mouse, and/or head-mounted display.

Acceleration algorithms such as culling algorithms are also implemented here and whatever else the rest of the pipeline cannot handle.

_Since a CPU itself is pipelined on a much smaller scale, you could say that the application stage is further subdivided into several pipeline stages, but this is not relevant here._

### 2.3 Geometry Processing

Executes on the GPU for most per-triangle/per-vertex operation.

Divided into functional stages:

_Pipeline of functional stages_

#### Vertex Shading
