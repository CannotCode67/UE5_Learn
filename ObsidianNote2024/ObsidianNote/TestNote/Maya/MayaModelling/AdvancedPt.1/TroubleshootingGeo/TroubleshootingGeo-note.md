Sometimes, a mesh with weird shading might not have to do with incorrect normal information, it can be caused by a corrupted geo. Geo can have all kinds of weird problems that are arise from numerous reasons like mirroring and merging, Boolean operation. This corrupted geo can be fixed by troubleshooting in following steps. 

1.  Click into one of the vertice not drag-select, and move it to see if edges are connected properly or is there another vertice or a face hiding underneath. 
    
2.  With the object selected, press ctrl-A to enter attribute editor. Under the mesh component display, we can display border with bold highlight to locate unmerged vertices.  We can display normal to see if the face is reversed. We can display center to see if there is a face where it shouldn't be. 
    
3.  If above methods do not help, then one possibility is overlapping face. We can click and delete a face to see if there is another one underneath. 
    
4.  And if we can't figure it out, then just delete the geo and use append polygon tool to add it back. It might solve the problem much faster than figuring it out. 
    
5.  In the top left corner of viewport, there is a panel called lighting. We can check and uncheck a option called two-sided lighting underneath it. We are normally with this option unchecked for the back-end of a face is unlit and we can tell if a face is reversed or nor right away. 
    
6.  Bonus tip is pressing 3 to preview the smooth version of the mesh, with enough experience, we should tell if the mesh is corrupted or not.