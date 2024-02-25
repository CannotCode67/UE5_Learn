Generators are substances that generate a mask or a material based on the mesh topology using the additional maps setup in the TextureSet Settings. 

From <[https://substance3d.adobe.com/documentation/spdoc/generator-109608968.html](https://substance3d.adobe.com/documentation/spdoc/generator-109608968.html)>  

We often see the generator to be applied to the mask instead of the layer directly. 

We cannot directly use a grayscale map as our mask even though a mask is just a 2D image with grayscale value. A way to workaround is add fill to a mask, then the fill would accept a grayscale map to apply to the mask. 

However, watch out that a mask with a list of modifications would only take into consideration of the top modification only. But with one exception, that is add paint. With add paint being the top modification, a mask would take into consideration of the paint and the modification below it. 

Example below, the grunge dirt scratched fill modification is overwriting the dirt generator below.
![[SubstancePainter/Generators/GetImage.png]]

Then the blending options come into play. Switching the blend mode of the fill to multiply, we can see an effect that we normally see in substance designer or photoshop.
![[SubstancePainter/Generators/GetImage (1).png]]