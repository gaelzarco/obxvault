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