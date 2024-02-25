Understand these two layers in a way that paint layer provides nothing until you paint info to the mesh. Fill layer provides all the info from every enabled output channel to the whole mesh, so it often needs a mask to filter the info so the info appears at the places we want them to  be. 

That's why tillable texture often needs a fill layer while a custom specific texture like logo would often use a paint layer, though it also works with a fill layer too. 

Fill layer has a parameter called projection. When creating a new fill layer, the default setting is UV projection. This setting is here because the way we do UV unwrap. Sometimes, we do UV unwrap with maximizing UV space as a primary purpose and minimizing UV distortion as secondary. And different projection settings might help minimizing the distortion without making changes to the UV unwrap. 

UV projection means putting the 2D texture on to the UV unwrap in 2D space. 

Like example below, we can clearly see the direction of the texture is wrong as in not how it would be in reality. Since the UV unwrap for the back and the side of the mat ball is just one single planar projection from the back view, this result is expected.
![[SubstancePainter/PaintLayer&FillLayer/PaintLayer&FillLayer-images/GetImage (1).png]]

Tri-planar projection is to do a planar project from each of the thee axis, like we normally do in maya. Or we are using tri-planar method to do our UV unwrap in maya. So with the way we do our UV, using UV projection setting is the result like using tri-planar setting. 

Example below is using the tri-planar method. The issue is with this geo and this texture, the automatically created seams for the planar projection of y-axis is also different from what we perceive in reality.
![[SubstancePainter/PaintLayer&FillLayer/PaintLayer&FillLayer-images/GetImage.png]]

Planar projection is one single projection of z-axis. It would work best for this mesh. 

For details, watch this.
![[SubstancePainter/PaintLayer&FillLayer/PaintLayer&FillLayer-images/GetImage (2).png]]