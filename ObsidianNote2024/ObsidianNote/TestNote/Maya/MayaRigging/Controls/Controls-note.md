A control is used to manipulate either directly the joints or some middle systems that further control the joints. And control is usually a nurbs object because they have a shape node which gives us the ability to customize the shape of this object. Also, nurbs object does get all values zeroed out when freeze transform. 

As we study joints before, we know joints often have local orientation that is different from the world. And control by default uses the world orientation. Adding a constraint between a joint and a control that don't share the same orientation can produce many problems. The change on one attribute of control can produce changes on all attributes of the joint, and that means all those changes in the joint is the need to maintain the relative bond between one single change in the control. And sooner or later, the joint would find itself cannot maintain this bond unless flipping its axis, causing all kinds of artifact when animating. The idea way is put both the control and the joint into the same orientation. Two ways to give th e control the orientation of the joint. One is to use parent constraint without maintain offset. The other is to put the control under the joint as a child, then zeros its translation and rotation values. At the end of both methods, we need to unparent them or delete the constraint. But now the control has non-zero translation and rotation values expressed in world-space coordinate, and if we freeze transform, the values are zeroed out, but the orientation is reset to that of world space. So we need one more thing, the group node. 

We know the child object's translation and rotation values are in the parent object's model space, if the child object hasn't been freeze transform. And if both objects are in the same position with the same rotation in the world space, the child objects would have zero for all attributes because zero means no difference between itself and its parent object. We can utilize this observation to give the control's attributes all zero values. 

First create the control, then with the control selected, ctrl g would automatically create a group with the pivot at the origin of the world space, and put the control under the group as a child object. Now, if the control hasn't been moved or rotated, the control and the group is in the same position with the same rotation. If the control has been moved, we just need to select the group and press center pivot, the group's pivot would use all the children's pivots location and take an average of the translation values, and go there. In this case, the only child is the control, so it goes directly to the control's pivot. And we have the group and the control at the same location and have the same rotation. If the control has been rotated, we have to manually zero out the rotation value of the control. Now, we are good. Next, we put a parent constraint without maintain offset to the group node and the joint. That effectively moves and rotates the group node and its child (the control) to the joint's location and rotation. The group node would have all kinds of values in attributes,  but its child would have all zeros. By now, we have the control with zero values for all its attributes, shares the orientation of the joint, and doesn't lose this orientation upon freeze transform. The last step is to protect the group node from being changed by locking and hiding all its attributes. We can also use parenting relationship, then zero the group node's attributes to move to where the joint is, lastly unparenting. It is the same thing. A good habit is to name the group node as the name of the control plus _offset, so that we know it is a group node for this purpose and the control it is corresponding to. 

One thing to know is even with this method, when applying a change in one attribute of the control, it is still possible for the joint to have non-zero values in attributes other that the same one of the control. It is just most of the time, the value is so small like 0.005, that there is no effect on the rigs. The reason for that is the limited accuracy of the parent constraint tool, but we don't need to worry about it. 

To customize the shape of the control, with the control selected, right click to choose edit point or control vertex, then select and move the one you want to customize the shape. It is for the visual effect, so don't worry this will break anything. By default, the nurbs object has 8 sections, so you get 8 edit points/control vertex. If you need more, we can adjust the sections attribute under inputs in channel box.

![[Controls (1).png]]

Also we can use curve tool to create control, because curve tool is also nurbs object. It is even more flexible, but needs more effort. 

Sometimes, to correctly represent the geometry, the corresponding control would need two nurbs objects to get the shape. However, we only need one control. In this case, parenting is not a good choice. Although this would make two nurbs objects to move together, we might accidently click and select the child nurbs object. Remember we mention nurbs object has two nodes: a transform node and a shape node. The transform node holds the values useful for controlling the joints, but the shape node just defines the visual look of the control in this usage case. Therefore, we can have one transform node and two shape nodes in one nurbs object to achieve what we need.  

First, we expose the shape node in the outliner, by right click and tick shapes in the outliner.

![[Controls (2).png]]

Then, we see the nurbs object is a shape node nested under the transform node. But dragging the shape node directly under another nurbs object's transform node won't work, this automatically moves the transform node and shape node together. So we need to use script to do it. We select the shape node first, then the transform node, then run this script.

![[Controls (3).png]]

Now, we have two shape nodes under one transform node. And we can unexpose the shape node in the outliner.

![[Controls (4).png]]

We can add attribute to our control, so it can utilize this value to further manipulate the geometry.

![[Controls (5).png]]

![[Controls (6).png]]

To further customize the control, we can change its color by enabling the drawing overrides like this.

![[Controls (7).png]]

Sometime, we deliberately design a control to be under the affect of other controls. Like example below, the overall mouth control is under the affects of left and right mouth corner controls (3 and 4) plus the upper and lower lip controls (1 and 2).

![[Controls (8).png]]

The overall mouth control can either be just a visual representation for the ease of work or be really skinned to the mouth geometry. Either way, it is fine. But the question here is how exactly can we make the overall mouth control to be partially affected by other controls? Because obviously parenting would not work here. We can use the cluster tool under the deform menu. Simply select the control vertex and apply cluster, then we would have a cluster handle object in the center of those selected control vertices, and those vertices would follow along this cluster handle object. The last step is to put the cluster handle object under the corresponding control as a child, and hide the cluster handle object in case of clicking and selecting by mistake. There we have the overall mouth control to be partially affected by other controls.

![[Controls (9).png]]

Controls are nurbs objects in our usage case, so maya treats them as nurbs objects underneath really. However, maya can tag them as control, to gain some benefits. In maya, when pressing the up key, it would automatically select the parent object, and it is called pick walk or cycle walk. You might think by pressing the down key, it would go into the child object, but it wouldn't. So this pick walk thing only works going upward, at least by default. Imagine in a control hierarchy, we can use this technique to speed up our work. However, we have the group nodes in between.

![[Controls (10).png]]

We can just press the up twice to select the upper control, or we can tag our nurbs objects as control.

![[Controls (11).png]]

When we tag the nurbs object as control, the nurbs object gets one more node, and we can see it in the attribute editor. Visibility option can use show on mouse proximity, which hide the control until the mouse is near it, making the animator easier to see the mesh. Disabling it by choose not overridden.

![[Controls (12).png]]

Now, in order to use pick walk, we need to do one more thing, and that is set the parent controller. Select the child control, then shift select the parent control, then click this parent controller. By doing this, we can not just go to the upper control by pressing up key, but also go down to the child control by pressing down key, ignoring the hierarchy in the outliner.

![[Controls (13).png]]

By doing this, we also get a benefit which maya can use GPU to speed up the playback, but this is relatively minor. 

All these tagging work are manually heavy, we need to find a script that can automatically do that for us. Unfortunately, it is not provided in the tutorial, we need to either write it our own or find it in the internet.