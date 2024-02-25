Sometimes, nodes can disappear from the library due to various reasons. However, designer can rebuild its own library by doing the following:
![[image.jpg]]


Simply adding output node won't connect the info to the 3D preview. We need to select the output node, then click AddItem under the usage, we need to specify the this added usage, is it an ambient occlusion / normal or what. To view it in the 3D preview, we also need to right click at empty space, click view outputs in 3D view.

A node with two inputs can have those two inputs exchanged with two steps: select those two inputs and press x key. A good example would be the blend node with its foreground input and background input because some blending modes require the inputs to have specific order to produce the expected result.