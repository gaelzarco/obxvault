#rasterizer #3dengine #tga
_There are 10 key components to building a game engine_

1. Rendering Engine
	- Responsible for drawing and displaying graphics each frame. It communicates with the GPU to turn #3D  models into #2D images.
		_Key tasks include:_
		1. Scene management - Organizing models, lights, and objects to be rendered.
		2. Vertex transformations – Converting 3D model coordinates into 2D screen positions.
		3. Vertex shading – Running shader programs on vertices to transform them.
		4. Rasterization – Converting vertices into pixels on the screen.
		5. Fragment shading – Adding colors and effects to pixels with fragment shaders.
		6. Lighting – Calculating how scene lights affect object colors.
		7. Post-processing – Applying visual effects to rendered images like blur or bloom.
		8. Managing render state – Configuring GPU modes for different rendering operations

It is impossible to begin learning how to program games without first understanding what a  Rasterizer is.

```javascript
A raster graphic represents a two dimensional picture as a rectangular matrix or grid of pixels, viewable via a computer display, paper or other medium.
```

Technically, a 

We can build an example Software Rasterizer using `TGA` files. It is one of the simplest formats that support images with rgb/rgba/black and white formats. 

We will only focus on having the functionality to set the color of one pixel (_besides loading and saving images_).
- There are also no functions for drawing segments and triangles. We will have to do it by hand.

##### Rust is our programming language of choice

##### However, we have to start small

###### Let's draw a line:


