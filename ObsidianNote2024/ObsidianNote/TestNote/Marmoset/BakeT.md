Baking shapes(highpoly detail) to a plane(lowpoly mesh) to generate decal texture(normal maps, height maps, ao maps, etc), so we can use substance painter to paint the texture on any lowpoly mesh we want. 

For creating the mesh, we use maya. 

For baking maps, we use marmoset. 

For painting it, we use substance painter. 

To use marmoset for baking, we need to do these steps: 

1.  Create a baker project by pressing the bread icon button. 
    
2.  Drag lowpoly and highpoly meshes to the scene. 
    
3.  Drag those meshes under the low and high hierarchy created by the baker project. 
    
4.  Adjust the cage range to capture all the highpoly details, it is a parameter of the low hierarchy.  
    
5.  Configure the maps we want to bake, it is a parameter of the baker project. 
    
6.  Specify the output location and file name. 
    
7.  Press bake. 
    
8.  Hide the highpoly mesh by pressing the eye icon button in the hierarchy. 
    
9.  To preview the maps, we fist need to create a new material by pressing + on the right side of the marmoset material window. 
    
10.  Assign the maps by dragging them from the folder into the slots in marmoset, the slots locate in the right side of the marmoset window, and will be there when a material is selected. 
    
11.  After assigning the maps, press the P icon button, it locates right next to the bake button, so it would only show up when baker is selected. 
    

Things to watch out: 

1.  Baking a good height map is somewhat difficult. As a perfect height map should be all grey with the middle point at perfect grey (RGB:128, 128, 128). The middle point is the flat surface the height map where darker grey means lower in height and lighter grey is higher in grey. We adjust the inner distance and outer distance by clicking the gear icon next to the height map configuration. If the map has area that is too bright, increase the outer distance. If the map has area that is very close to black, we  should decrease the inner distance. To get the perfect grey, we need to use photo shop for help. Watch modular art of sci-fi lab, No.53 video, (9th min to 11th min).