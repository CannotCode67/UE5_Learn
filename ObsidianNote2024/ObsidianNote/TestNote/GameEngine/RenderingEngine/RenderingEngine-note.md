Object can be opaque (in which case light cannot pass through its volume), transparent (in which case light passes through it without being scattered), or translucent (meaning that light can pass through the object but is scattered in all directions in the process, yielding only a blur of colors that hint at the objects behind it). 

While an accurate rendering of transparent and translucent object requires knowledge of the interior structure and properties to correctly reflect, refract, scatter, absorb light through object's volume, most game engines don't go to all that trouble. Therefore, it is safe to say that most game rendering engines are primarily concerned with rendering surfaces. 

Game developers model their surfaces using triangle meshes. Triangles are the polygon of choice for real time rendering because:
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage.png]]

Tessellation describes the process of dividing a surface up into a collection of discrete polygons. Triangulation is the tessellation of a surface into triangles. Ideally, surface can be tessellated based on the distance from the camera, so every triangle is less than one pixel in size. Game developers often attempt to approximate this ideality through level of detail (LOD). The first LOD often called LOD0 represents the highest level of tessellation, and it is used when object is very close to the camera. Subsequent LODs are tessellated at lower and lower resolution. As the object moves away from the camera, the engine switches from LOD0 to subsequent LODs. 

Let's take a closer look at single triangle polygon because many things of rendering is focus on the level of one triangle. 

A triangle is defined by the position vectors of its three vertices (P1, P2, P3). The edges of the triangle can be found by simply subtracting the position vectors of adjacent vertices. And we can get the normal vector through normalizing the cross product of any two edges. But remember, the direction of the cross product has two possibilities. So we need to define which side of the triangle is the front. This can be defined by specifying a winding order (clockwise or counterclockwise).
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (1).png]]

For information on what data structure is used to store vertice and maintain the corresponding relationship between vertices and triangles, read GameEngine Page 450. 

For meshes that are not static, vertices belong to them are usually specified relative to a local coordinate system called object space or model space. When rendering these meshes, we need to change their coordinate system from object space to world space, then from world space to view space, finally from view space to homogenous space. For static meshes, their vertices are specified in world space at the start. 

Light is also one of the key factor in rendering as it interacts with objects in very specific ways that any inaccurate simulation would raise our attention very easily. 

Despite all of its complexity, light can really only do four things:
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (2).png]]

As game rendering engineer, we only care about the first three behaviors. When light gets completely absorbed, we don't see it. But that's rarely happening. A more common situation is a portion of light gets absorbed. Like white light cast on red object, all light gets absorbed except the red light, it bounces back to our eye, so we see the object as red-color. Red light cast onto a white object would achieve the same result for our eyes.  

Reflection can be diffuse (scattered equally in all directions), specular (reflect directly or spread into a narrow cone), anisotropic (the way in which light reflects changes depending on the angle to view the surface). Light can also enter a semi-solid surface, bounce around and then exit the surface at a different point from the one at which it entered the surface, which is called subsurface scattering. 

The most commonly used color model in computer graphics is the RGB mode. It can be different formats, the most common one is the RGB888, 8bits per channel, for 24bits per pixel. The high dynamic range (HDR) lighting uses the log-LUV color mode. RGB color often has one more channel called alpha. This channel is also 8 bits, and it is used for opacity usually. 

Vertex attributes. 

The visual properties of a surface is based on the information carried by its corresponding vertices. And algorithms take those info as input to perform calculation as needed, and output the visual properties of the surface. 

A typical triangle mesh includes some or all of the following attributes at each vertex.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (3).png]]
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (4).png]]

With each vertex carries information like these, we can get the visual properties of the surface defined by those vertices, hopefully in a per-pixel basis. One of the simplest ways is through linear interpolation. When LERP is applied to vertex colors, it is known as Gouraud shading.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (5).png]]
LERP is routinely applied to all kinds of vertex attribute information as well, such as vertex normal, texture coordinates and depth.

Lighting calculation also performs calculations on a vertex basis, then the outputs get interpolated across the surface through Gouraud shading. Most lighting models determine how a ray of light will reflect from a point on surface based on the normal vector of that point. Therefore, the vertex normal has a significant impact on the final result.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (6).png]]

Given vertex basis attributes, we can perform rendering calculation across the surface, but those outputs are generally not customable. And we do want all kinds of info across the surface to be in control. One way to do it is to use texture mapping. That's why we need the vertex UV coordinate attribute. We are shrink wrapping the texture maps onto the surface of our mesh, and UV coordinate of each vertex is the info controlling which part of the map goes to which part of the surface. 

The sphere on the right has triangles that appear in relatively big size. There is only so much info there (four vertice normals) to calculate the lighting for an area that is simply too big. However, with texture map (a normal map in this case), the surface would have one unit normal vector for each texel of that part of the normal map projected onto this surface, and even better, they are highly customable in DCC (digital content creation) package.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (7).png]]

The term texel refers to one pixel of the texture map, to be distinguished from pixel of our screen. Texture maps are composed of pixels just like our monitor screen, and they typically have the resolution of powers of two, so 256 by 256, 512 by 512, 1024 by 1024, etc. And because each texel represents a value, a texture map sometimes is used as a stand-alone data table or lookup table. In RGBA model, with all channels together, we have 32bits to define one value, which is a lot. More often, we use just one channel of one value, and another channel to retrieve another value, in this case, one texel can contain 4 values. 

The vertex UV coordinate specifies the location of the vertex in a 2D space called texture space. Here, u is what we normally call x, and v is y, to be distinguished from other spaces. Texture is mapped to this space as background, and the surface defined by a couple of vertices' UV coordinates, overlaps with a portion of the background. The overlapped part of the background is what we see on that specific surface in 3D view. This texture space only accounts for a square ranging from (0, 0) to (1, 1). UV that are outside of this area, can be treated in following ways based on the selected mode.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (9).png]]
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (8).png]]
All textures for game are bitmaps, and the common texture formats include Targa(.tga). Portable Network Graphics (.png), Windows Bitmap (.bmp), and Tagged Image File Format (.tif). We don't use psd file because it is too large, we don't need the access to all kinds of layers or masks or adjustments info. 

Another concept is texel density, which refers to the ratio of texel to pixels. For example, a texture of 1024 by 1024, and the window viewing it takes up the portion of the screen that is also 1024 by 1024. Then, we say the texel density is one, for one texel matches one pixel on the screen. This is the ideal situation we want to achieve because if it is greater than one, multiple texel rendered in one pixel would raise little more value. What is worse would be if the texel density is lower than one, player starts to see the jaggedness artifact of the texture. With that being said, it is so much harder in game to achieve 1 to 1 texel density, because the virtual camera moves closer and further to a textured surface while playing. This texel density is bound to change, unless we have a method to change the resolution of the texture map based on the view distance from camera, like a LOD, but for texture. This technique is called mipmapping. In this technique, we create a sequence of lower-resolution textures by taking one-half of the width and one-half of the height of the predecessor, and each of them is called mipmap or mip-level.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (10).png]]

Once we have our texture mimapped, the graphics hardware would auto select the appropriate mip level according to the distance from the camera, in an attempt to maintain a texel density close to one. Options like trilinear filtering can be chosen to sample two adjacent mip levels and blend the results if a resolution in the middle of the existed mip levels is needed. However, this kind of filtering is not entirely for problem above. In game, the graphics hardware samples the texture map by considering where the pixel center falls in texture space, and further the virtual camera often views things at a angle. So it is usually not a clean one-to-one mapping. In the cases where the pixel center lands on the boundary between multiple texels, the graphic hardware usually samples multiple texels and blend the results to output the value for that pixel. The options control how to do it are called texture filtering. Trilinear filtering is one of the options. Here are the options and what they do.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (11).png]]

A material is a complete description of the visual properties of a mesh or a part of the mesh. Texture maps are the input data along with the vertex attributes carried in the mesh. The algorithms or calculations of the input to yield outputs is the shader program. A material usually includes both the shader and the input data. 

The term shading is often used as a loose generalization of lighting plus other visual effects. 

Inside rendering engine, mathematical models used for light-surface and light-volume interactions are called light transport model. The ones that only account for direct lighting are called local illumination models. The ones that account for both direct lighting and indirect lighting are called global illumination models. Global illumination is described completely by a mathematical formulation known as the rendering equation or shading equation. 

The most common local lighting model employed by game rendering engines is the Phong reflection model. For details, read GameEngine Page 471. 

Light sources used in game, read GameEngine Page 475. 

The virtual camera consists of an ideal focal point (origin of the view space) with a rectangular virtual sensing surface (near plane) called the imaging rectangle floating some small distance in front of it. The imaging rectangle consists of a grid of square or rectangular virtual light sensors, each corresponding to a single pixel on screen. Rendering can be thought of as the process of determining what color and intensity of light would be recorded by  each of these virtual sensors.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (12).png]]
The far plane and the near plane above define the region of space that the virtual camera can see, and this region of space is called the view volume. Picture above shows the view volume of a perspective projection. The view volume of a orthographic projection is shown below.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (13).png]]
Remember we said when rendering a object, its vertices are transformed from object space to world space, then from world space to view space. So, we have object to world matrix, and world to view matrix, and often these two matrices are concatenated together to speed up the rendering process. 

There is actually one more space to transform into, and it is called homogeneous clip space. Because we might use different kinds of projection, and the screen has different resolution and aspect ratio. To map things in view space into the homogeneous clip space is the process to convert the view volume into one that's independent of all those factors.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (14).png]]
The view volume planes are axis-aligned, making it convenient to clip triangles to the view volume in this space. As we will learn, clipping triangles is one of the process of rendering, and it is essentially the process of determining which or which part of triangle is outside of the screen, and need not to be rendered. The converting is also through a matrix multiplication. For details, read GameEngine Page 482. 

Screen space is the two dimensional coordinate system whose axes are measured in terms of screen pixels. The x axis typically goes from left to right, and the y axis goes from top to bottom. And the aspect ratio is the ratio between the width and the height, the two prevalent aspect ratio is 4:3 and 16:9.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (15).png]]

Rendering triangles expressed in homogeneous clip space by simply drawing their x and y coordinates and ignoring the z. But our homogeneous clip space is a square, so scale-and-shift operation known as screen mapping is needed before we do. 

The final rendered image is stored in a bitmapped color buffer known as the frame buffer. And the monitor reads from the buffer. Rendering engine usually maintain two frame buffers, so when one is being scanned by the monitor, the other one can be updated by the engine. This is called double buffering. The swapping between two buffers happens during the vertical blanking interval (the period during which the monitor electron gun is being reset to the top-left corner of the screen). Double buffering ensures the monitor always reads a complete frame, preventing a tearing effect where the upper part of the screen displays a new frame, and the lower part of the screen displays the previous frame. Some rendering engines use three frame buffers, known as triple buffering. The rendering engine can start working on a new frame (buffer A) after it finishes the current frame (buffer B), but the display is scanning the previous frame (buffer C). 

Frame buffer is one of many render targets which is a general name for any buffer the rendering engine draws graphics into. There are depth buffer, stencil buffer, and all kinds of other buffers used to store intermediate rendering results. 

To produce an image of a triangle on screen, we need to fill in the pixels it overlaps. The process is known as rasterization. During this process, the triangle is broken into many pieces called fragment. Each fragment corresponds to a single pixel on the screen. (When multi-sample antialiasing is applied, a fragment only corresponds to a portion of a pixel. A fragment needs to pass a number of tests before it is written into frame buffer or blended with the already existing value.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (16).png]]

When two fragments correspond to the same pixel, and one is closer to the camera than the other, we need some way of ensuring the closer fragment will appear on top. When one fragment gets covered, we say it is occluded. To implement occlusion properly, render engines use a technique known as depth buffering or z buffering. The depth buffer typically contains 24-bit integer depth information for each pixel in the frame buffer. (The depth buffer is usually stored in a 32-bits-per-pixel format, with 24-bit depth value and 8-bit stencil value packed together.) Every fragment has a z-coordinate that measures its depth "into" the screen. (This z coordinate is found by interpolating the z coordinates of the corresponded triangle's vertices.) When a fragment is written into a frame buffer, its depth is stored into the corresponding pixel of the depth buffer. When another fragment is drawn into the same pixel, their depth values are compared, and the one with bigger depth value (further away from camera) gets discarded. 

This technique is perfect if we have infinite precision for the depth buffer, but it only has 24-bit, so fragments that are very close to each other would have the same depth value, causing a noisy artifact called z-fighting. And the precision of clip space z depths are not evenly distributed across the volume. The relationship between the z distance between two planes in view space (PVz) and the z distance between two planes in homogeneous clip space (PHz) is shown below.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (17).png]]

This relationship is the nature between homogeneous clip space and view space. And we see when near the camera we have large delta z depth in homogeneous space, and further it gets away from the camera, the delta z depth gets smaller. As a result, z-fighting becomes rapidly more prevalent as objects get farther away from the camera. However, z coordinates in view space vary linearly with distance from the camera, so it would be a better choice for measuring the depth as it achieves uniform precision across the view volume. This technique has a name called w-buffering because the view space z coordinate conveniently appears in the w-component of our homogeneous clip space coordinates. It is more expensive as depths in homogenous space needed to be inverted back to view space to do linear interpolation, then re-inverted back to homogeneous space to being stored in w buffer. 

The rendering pipeline. 

A pipeline is just an ordered chain of computational stages, each with a specific purpose, operating on a stream of input data items and producing a stream of output data. Each stage of a pipeline can typically operate independently, so it lends itself extremely well to parallelization. The throughput of a pipeline measures how many data items are processed per second overall. The latency of a pipeline measures the amount of time it takes for a single data element to make it through the entire pipeline. 

The high-level stages in rendering pipeline are:
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (18).png]]

The first two stages are tagged offline, meaning the process of these two stages is finished before run-time. And they are referring to the process of producing assets and making them ready to be used in game. 

The GPU is not quite a general-purpose microprocessor. It achieves its high processing speeds by utilizing hundreds or even thousands of parallel arithmetic units to process streams of data. However, programming a GPU to perform non-graphics-related tasks can be done and is known as general-purpose GPU computing. Non-graphics computations are performed by special shader known as compute shader. To achieve high performance, this is one of the technical keys. 

Virtually all GPUs break the pipeline into the substages described below.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (19).png]]
We'll examine each stage in details. 

Vertex shader. 
This stage is responsible for transformation and shading/lighting of individual vertices. The input to this stage is a single vertex in one thread, many vertices are processed in parallel. The vertex shader handles transformation from model space to view space via the model-view matrix. Perspective projection is also applied, as well as per-vertex lighting and texturing calculations and skinning for animated characters. The output of this stage is a fully transformed and lit vertex, whose position and normal are expressed in homogeneous clip space. On modern GPUs, the vertex shader has full access to texture data, a capability that used to be available only to the pixel shader. Vertex shader accesses texture data mainly for heightmaps (used for vertex displacement) or data look-up table. 

Geometry shader. 
This optional stage operates on entire primitives (triangles, lines and points) in homogeneous space. It is capable of culling (not to render) or modifying input primitives, and it can even generate new primitives. Example usage read GameEngine Page 497. 

Stream output. 
Some GPUs permit the data that has been processed up to this point in the pipeline to be written back to memory. From there, it can then be looped back to the top of the pipeline for further processing. This feature is called stream output. 
A example is hair rendering. The GPU performs the physics simulation on the control points of hair splines. The geometry shader tessellates the splines. The new tessellated vertices are then written back to memory, then piped back into the top of the pipeline to be lit and further rendered. CPU used to do the physics simulation and tessellation work, but obviously this kind of work can be done much faster in GPU. 

Clipping. 
The clipping stage chops off those portions of the triangles that straddle (sit or stand with one leg on either side of) the frustum. Clipping is done by identifying vertices that lie outside the frustum and then finding the intersection of the triangle's edges with the planes of the frustum. These intersection points become new vertices that define one or more clipped triangles.

Screen mapping. 
Screen mapping simply scales and shifts the vertice from homogenous clip space into screen space. 

Triangle set-up. 
During triangle set-up, the rasterization hardware is initialized for efficient conversion of the triangle into fragments. 

Triangle traversal. 
During triangle traversal stage, each triangle is broken into fragments (rasterized). Usually one fragment is generated for each pixel. This stage also interpolates vertex attributes in order to generate per-fragment attributes to be processed along with the fragments in pixel shader. 

Early z-test. 
Doing a early z test at this point can prevent any occluded fragment from being processed in pixel shader stage which is relative very expensive. 

Pixel Shader. 
Pixel shader's job is to shade (light and otherwise process) each fragment. The pixel shader can also discard fragments, for example because they are deemed to be entirely transparent. The input to this stgae is a collection of per-fragment attributes and fragments. The output is a single color vector describing the desired color of the fragment. 

Merging / Raster Operations stage. 
It is responsible for running various fragment tests including the depth test, alpha test (reject fragments based on pixel's alpha channels and fragments' values), and stencil test. If a fragment passes all the tests, its color is blended (merged) with the color already in the frame buffer. The way in which blending occurs is controlled by the alpha blending function.
![[GameEngine/RenderingEngine/RenderingEngine-images/GetImage (20).png]]

Programmable Shaders. 
Modern shader models support for high-level C-like shader language such as Cg (C for graphics), HLSL (High-Level shading language), and GLSL (OpenGL shading language). And all three programmable shader (vertex shader, geometry shader, pixel shader) support roughly the same instruction set and have roughly the same set of capabilities. 

Shader Registers. 

To be continued…