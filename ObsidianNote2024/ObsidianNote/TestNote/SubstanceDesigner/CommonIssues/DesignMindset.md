There are all kinds of map outputs, but from the perspective of the most basic ingredients, there are only greyscale map and color map. Most output maps are composed of greyscale map, and albedo map uses a color map.

Always do the height/normal map first. Think of height map like a geometry, except it is not until we use tessellation to bring the height scale up. And normal map is using that tessellated geo info to alter the lighting info. Therefore, once we have a height map, it is easy to get a normal map, and vice versa. In addition, ambient occlusion map and curvature map are using the same info as well.

Recall when we bake high-poly mesh info to low-poly mesh, we have all kinds of maps as options to bake. That high-poly mesh info is height map in the context of texture. So maps that we can bake from a high-poly mesh are also able to be created from a height map. That is the logic why we should do the height map first.

With AO map, height map and normal map out of the way, we still have albedo map, roughness map and metallic map to define. Now, those info may or may not relate to the geo info(aka the height map). They can be related in a sense of like, a piece of geo sticking out would have more wear and tear for it is easier to be hit in a real-world environment. A wear and tear might result in a color bleak(albedo map info) and it might be rougher(roughness map info). However, if this material is all new and untouched by the real world, then the above logic doesn't stand.

For metallic map, we just need a greyscale map to define which part is metal. On top of that, we still need to assign the metallic color in the albedo map. Remember there is a node(PBR_metal_reflectance) to provide the precise albedo info for different kinds of metals.

Since height map is so important, how do we go about making the height map?

For greyscale value, black is 0, white is 1, to be accurate, in LDR(low dynamic range). For height map, black is inward, white is outward.

To define shapes with greyscale map, we can start with shape node if the shape is somewhat regular. Irregular shape usually are noise-like patterns, in this case we have all kinds of generators or procedurally generated noise pattern. If we are lucky, we might find what we need. If not, we should try to twist the noise pattern to get what we want. Using shape node to produce irregular shape is always our last choice.