Prepare step: 

Lowpoly mesh and highpoly mesh should share the same position. 

-Painter provides two ways of matching the highpoly mesh and lowpoly mesh: always and by mesh name. Always means it uses the world position to match, while by mesh name means it uses the name to match. However, either way, we need to keep the highpoly mesh and lowpoly mesh at the same position as the essence of baking is to capture the normal information, and position is a very critical factor in this process. 

-To use by mesh names, we need to rename the lowpoly mesh and highpoly mesh properly. 

Whether to triangulate the lower mesh before bringing in to substancePainter. 

-In older version of painter, it automatically triangulates the imported mesh if the mesh isn't already. This doesn't bother us as the gaming artists because the mesh is eventually triangulated anyway in game engine. 

-painter 2020 and later stops providing this triangulation process, but it seems to work fine as long as the faces are quads. It leaves the users to decide whether and when to triangulate the mesh. 

-One situation where Painter 2020 would still triangulate the imported mesh, is when the mesh has nonmanifold geometry. Painter would triangulate the whole object which contains nonmanifold geometry, not just the broken part. 

Test bake (trial and fix problem) step: 

The way we go about test bake is to bake it with only normal map and see if the result has any artifacts. 

-Don’t need to worry about how the lowpoly mesh looks before baking. The lowpoly mesh with baked normal map on is the one we care. 

Elements that can affect the baking result are: 

1.UV unwrapping 

-UV needs to be checked if it is overlapped, not properly unfolded, or packed too close to each other. 

-Painter can apply dilation to texture, meaning each UV shell would have some padding texture around them, if we pack UV too tight, then the padding texture might get into other UV shells. 

-In cases where we try to maximize the UV space efficiency, we often delete mirrored or identical objects and leave only one piece for its kind. And this piece would have more UV space. Then we copy this piece around. But remember the copy should have its UV moving out of the 0 to 1 UV space. Otherwise, painter would not bake correctly. 

2.Harden/Soften edges 

-We define hard edges for lowpoly mesh so it can look correct in Maya. However, when the lowpoly mesh goes into painter for baking, its hard edges have to be back up by UV seams. It means hard edges need to be UV seams, or it would cause artifacts. If we want a hard edge looking but it is not UV seam, then we would need to apply bevel for that edge. However, this alters the geometry too much, which is not practical. So we can either make the hard edges UV seams or we soften hard edges inside UV shell. 

-Soften all edges would get a good result in Painter, however, the normal map generated this way would not work with change in resolution. Unluckily, mip-mapping technique used in game would alter the resolution of texture, so this approach doesn't work with game art. 

3.Geometry 

-Maintain good geometry for lowpoly mesh is important, triangle and quad are good. We might not need to do the same for the highpoly mesh. As mentioned, nonmanifold geometry can cause Painter to forcedly triangulate your mesh. Maya's judgement for nonmanifold geometry is invalid sometimes, so the mesh might be able to perform UV layout and UV unfold, but still be nonmanifold.  

-Poor geometry would introduce artifact in baking. Sometimes, keeping faces to be triangle and quad is not enough, and we need to provide a good way of connecting the geometry. 

-For flat surface to have curve shapes to connect, we need to provide information about the curve shapes to the flat surface. Two ways to do it, bevel the connecting part or do an insert operation for the flat surface. 

4.Highpoly and lowpoly 

-Sometimes, highpoly mesh and lowpoly mesh are at the same position, but parts of them just don't match due to the smooth operation. And that is normal, but if the mismatch is too much, it can also introduce artifact for making it impossible to capture normal information. 

5.Position 

-Painter provides front distance and rear distance parameters to control how far from the front and the back of a face we should capture the normal information. However, a good distance might be only good for some parts of the mesh, but bad for others. The way we can fix it is to use a cage, or placing different parts apart so that they don't influence each other. 

6.Materials (To lowpoly and highpoly) 

-UDIMs workflow can offer UV tiling in a way where each rectangular UV space is different, in oppose to the default way where all rectangular UV spaces are identical. 

-Painter 2020 offers build-in UDIMs workflow option, but how to use exactly? Need more research. 

-An old way of getting similar result is to assign different materials to the lowpoly mesh. Parts of the mesh share the same material would be read by Painter in the same UV space, and they share the same texture set. So, if the lowpoly mesh has two different materials applied, we would have two normal maps, two color maps, two roughness maps, etc. 

-If we assign different materials to highpoly mesh, this information enables Painter to bake the ID map, which is useful for filtering during the later texturing process. 

When baking, we can set the output resolution of those baked maps. However, in painter, we need to set the display resolution in the texture set setting as well. Otherwise, painter would downsize or upsize the baked maps in order to match the display resolution we set. Be careful though, the baked map resolution is the most accurate. Upsizing the baked map resolution is a 2D treatment apply to the pixels on the baked maps, and it might not be the most accurate result.