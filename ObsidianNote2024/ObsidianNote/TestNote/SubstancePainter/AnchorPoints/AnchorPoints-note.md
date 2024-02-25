![[SubstancePainter/AnchorPoints/GetImage.png]]

Tricks&Tips
![[SubstancePainter/AnchorPoints/GetImage (1).png]]

Anchor point by its simplest form is just a way of providing information for future referencing.  

When a layer with color information creates an anchor point, it is providing its color channel information. Another layer that's on top of it, can reference that color information by referencing that anchor point, so the color information in both layers is the same now. Of course, the top layer can then add whatever modifications to that color info to suit its own need, but without breaking the original info. One proper way of using it is to get details on symmetrical parts, so we don't need to worry if the size or the position doesn't line up. 

When a mask creates an anchor point, it is providing that mask information. Another material can be applied to the same area by referencing that anchor point to use that mask. Beware when referencing mask with anchor point, we only get the info on top of the anchor point.
![[SubstancePainter/AnchorPoints/GetImage (2).png]]

Or with more control, a paint layer with anchor point referenced by material mask, can provide white info by using white color information to paint wherever we want. So wherever we paint with that paint layer, the material would be applied. 

One of the main use is for normal/height details. We want those details to be treated as real geometry but without us doing the paintjob. Microdetails slot of generators can cover those details by referencing anchor points of those details. 

Another thing to remember is passthrough. One anchor point can be referenced by multiple things, but each of this thing can only reference one anchor point. However, we might want to reference multiple anchor points at the same time. The method is passthrough. A fill layer with all channel blend mode setting to passthrough and with all the output channels disabled, creates an anchor point. It basically reflects all the output channel data gathering from all the below layers. By referencing this fill layer, we get all the information of layers below it. Before apply overall effects, it is a good practice to create a passthrough.