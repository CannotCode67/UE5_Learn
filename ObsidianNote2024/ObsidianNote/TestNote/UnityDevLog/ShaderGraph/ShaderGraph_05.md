The step to use shader graph to create material. First, in the project window, we need to create -> shader -> PBR graph. If other type of material is needed, we can use other graph, but they are just templates to help start. Drag imported maps into the shader graph window and connect them to the output node, similar to substance designer. After finishing our connecting and any modifying,  we press the "save asset" button at the top left corner. Then, the thing we finish is called shader. We can either create the material by right clicking the shader in project window and create -> material, or we can just right click at empty space in project window to create -> material like we would normally do. Then with the material selected, change the shader in the inspector window to the one we just finish, there we have a material ready to apply to meshes. 

Any change to the shader would not be updated to materials unless we press the "save asset" button at the top left corner. And no, "control + s" doesn't do this. 

A broken material shows magenta, but a broken shader shows pure white, so keep an eye for it. 

In order to use normal map correctly, we need to set Texture Type as normal map. Otherwise, we would get some artifacts when applying the material, but not shown up in the preview window in shader graph. 

Weird artifact can be caused not only by incorrect texturing or uv unwrapping, but also by softening or hardening the wrong edges. 

When mesh is imported into Unity, we can use FBX setting with export tangent and bionormals checked. Otherwise, Unity would generate mesh tangent by itself, and that sometimes can cause artifacts on texturing. 

Normal map brought into Unity should be marked as normal map in texture type in inspector. Otherwise, PBR shader would not use it correctly. Also when the normal map is brought into shader graph window, a sample texture 2D node is created automatically. Remember to set the texture type in this node to normal map as well. 

Normal map in Unity uses openGL convention, which means y+, and when normal map is generated in substance designer, remember to set it to openGL. Unity's shader graph use smoothness map which is just the inverted version of roughness map, so use a invert grayscale node in substance designer to convert a roughness map to smoothness map. 

Sometimes we need a mesh to have a gradient color, which means two colors blending into each other. We would use a lerp node with two color inputs and a mask input to achieve this in shader graph. However, remember the mask without modification would map itself to the whole 0 to 1 UV space. Therefore, the UV unwrap for the mesh should accomodate this gradient like direction or something. Otherwise, the gradient color would not show up properly. I am sure there is some ways to modificate the mask, because adjusting the UV could be difficult for certain meshes and it is not very efficient. 

A tiling and offset node is used to adjust the tiling and offset of a UV unwrap, similar to transform 2D node in substance designer. Then, for ease to use, we add an exposed parameter of Vector2 to control the x and y scaling of the UV by plugging it into the input slot of the tiling and offset node, then connect the output to the UV slots of all the related maps(sample texture 2D node). Back to scene view, with the object selected, we can adjust the exposed parameter to control the material tiling in the inspector window down to the shader area. 

When a mesh has multiple UV sets, we can expose a parameter in shader graph to select which UV set for the mesh with this material on. The parameter to match UV set is not vector1. It is Enum, and we need to use it in this way.![[UnityDevLog/ShaderGraph/GetImage.png]]

For roughness map to become smoothness map, we can use level node in designer to do that, just switch the output white handle and output black handle. But if we have to do it in shader graph, we can use one minus node, which use 1 to minus the input value. And, in this case, when roughness value get subtracted by 1, it would produce smoothness value, since smoothness value is just the inverted version of roughness value.