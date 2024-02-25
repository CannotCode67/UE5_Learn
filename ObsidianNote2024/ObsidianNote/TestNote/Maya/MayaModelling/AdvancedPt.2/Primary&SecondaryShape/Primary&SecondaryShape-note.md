Again, a not too complex shape like this.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage.png]]

Obviously, the primary shape is the big UFO-like platform, and the secondary shape is the circular cutout in it. Remember we always tackle the primary shape first. 

Here, we can take care of the center cutout with extrude faces. The real geo amount preplan is the cutout on the outer rim. We use boolean operation on this, and we normally use 8-side cylinder for round shape. So, we need a center line, and two diagonal lines for one circular cutout but each diagonal line will be shared by two circular cutouts. We get 1(center line) + 2*0.5(shared) = 2 lines for one circular cutout. In total of ten cutouts, we need a 2*10 = 20 subdivisions for the big cylinder shape.![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (1).png]]
Once we determine the number of subdivision, we can obtain the UFO-like shape with a couple of extrude, offset, scale, transform operations.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (2).png]]

We offset the bottom of circular cutout, and delete every other edge to obtain quads.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (3).png]]
Since the shape is repeating itself circularly, we can just model one single unit and duplicate it.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (4).png]]
First delete all but the unit piece, the do the boolean oppertaion.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (5).png]]
Same method to obtain quads.![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (6).png]]
Clean up and connect the geo, then add the bevel, it is circular and just one bevel, so it should be fine.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (7).png]]
Use the shif-d to duplicate around.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (8).png]]
Now, we handle the bevel of the primary shape. When there is a straight line going to another, it is a good scenario to use fencing because fencing introduces less roundness, and it is fast. The drawback is if the bevel turns out too wide or too tight, then we have to adjust them one by one, which is slow.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (9).png]]For area like this where a vertice is connected by more than 4 edges, we can't do a fencing.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (10).png]]
For these two edge loops, we need to use bevel.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (11).png]]
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (12).png]]
So there are really goods and drawbacks for both bevels and fencing. 

The result is good except there is some minor artifact at the rim due to the fact that the vertice got pulled back a little bit more by two edges from the circular cutouts, resulting in some minor unevenness of the highline.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (13).png]]
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (14).png]]
In this case, it is so minor that we should be able to get away with this. 

If we have more geo amount, we should be able to get rid of this using such geo.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (15).png]]
Manually adding this geo in the end requires the roundness info, which we might be able to solve using edge flow tool. But if we can preplan it, it would have less trouble.
![[Maya/MayaModelling/AdvancedPt.2/Primary&SecondaryShape/Primary&SecondaryShape-images/GetImage (16).png]]