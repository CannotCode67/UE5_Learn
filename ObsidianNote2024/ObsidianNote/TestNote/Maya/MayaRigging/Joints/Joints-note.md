Skeleton is made of joints. 

When create joints, snapping tool is also available. 

When create joints, we can turn on symmetry option to speed up the process. Then, when finished with creating joints, we can delete the constrains. 

A joint's scale value only affects the geometry that is skinned to this joint. The radius value of joint is just a visual representation for the ease of seeing. Globally setting joint size (visual representation), we go to display->animation->joint size. 

When create joints in a consecutive fashion, the previous created joint would automatically have its object orientation pointing to the next created joint because the new joint would be automatically parented to the previous joint. This is a desirable situation as this is how we expect the joint to work. And it matches the description where the child joint should only have x translate value (in maya default rotation order, x is the axis pointing to the child joint). If the child joint needs to move in more than one direction, the way to do it is rotate the parent joint, then freeze transform to clear out the rotation value, then do the x translation for the child joint. Remember joint should have zero rotation values, and freeze transformation to a joint would not zero out the joint's translation value, that's why we need controller. 

However, joints created in a nonconsecutive fashion, then parented together manually, often have their object orientation equaling to the world orientation. To fix this, maya provides a  function called orient joints. We can find it under skeleton menu, with menu set set to rigging instead of modelling.
![[Joints-0.png]]


The primary axis is the one used to point to the child joint. The secondary axis is the axis that would have the same direction as the axis indicated in the secondary axis world orientation area. In the example above, local y axis would align itself to the world y axis and share the same direction. If we want an opposite direction, we can change the + sign to - sign. With orient children of selected joints turned on, we can only select the parent joint and apply this function, all its children and itself would have the orientation set, except for the last one (the one which doesn't have a child to point to). This end joint should have  the orientation like its parent joint, and the way to get this is using orient joint function, but turn on orient joint to world and apply. 

With joints selected, we go to display->transform display->local rotation axis, this would show the selected joints' local rotation axis in viewport, which helps us to see if there is any orientation problem going on. 

Wrist joint should have the same orientation as the elbow joint, even though there are five fingers as children of the wrist joint. Doing it this way would give us the effect which we expect from a real skeleton structure. 

When curling fingers, we want all five fingers to curl about the same axis so that we can control the hand to curl with one rotation value. They naturally do except for the thumb. In the picture below, all fingers except for the thumb curl about z axis (this is a custom setup, not a maya default setup). If we curl the thumb about z axis, the thumb bends in the wrong direction like what we see in the picture.

![[Joints-1.png]]

One way to fix this is to use orient joint tool if we can find a solution. It means y axis is used to point to the child joint, and it would stay that way, leaving us only two possibilities (local x aligns to world z, or local z aligns to world z). And since local z aligns to world z is proved to be wrong, the only option left is local x aligns to world z. If it gets what we want, great, but there is no guaranty. In cases where we fail with aligning axis or we just want more control instead of just aligning axis, we can do this.

![[Joints-2.png]]

The left red-circle button is called the select by component type. Click it, then right click the question mark (right red-circle) to choose local rotation axis. If you cannot find the question mark, click the red underlined button to unhide this specific set of buttons. With all that done, we can select the local rotation axis in the viewport and rotate them to have precise control. In maya 2018, there is a bug causing we cannot select the local rotation axis in the viewport. Maybe we should upgrade it to higher version. However, there is a problem. We manually change the local rotation axis, now it doesn't match the transform axis.

![[Joints-3.png]]

And the red underlined value is the result, which should be zero. It can cause problem when exporting animation to game engine. The fix is this, using a script. With a single joint selected, run this script. It seems the script only works with one joint at a time.

![[Joints-4.png]]

A script to change rotation order for all selected joints. Because we can only change one joint's rotation order in the attribute editor, and doing it for all the joints again and again is not desirable. I am not sure how this list is getting its content, by type = "joint", does it mean getting all the joints in the scene into this list, even without us selecting them? Need more research.

![[Joints-5.png]]

When using mirror joint tool, the mirror function can be either behavior or orientation. The behavior option would alter the mirrored joints' orientations, so that the same value would produce the mirror motion for both left and right sides. So a piece of animation designed for the left side skeleton would also work for the right side skeleton, and that's usually what we want. For the orientation option, we might need to twist the value to get the effect.

![[Joints-6.png]]

For each joint, there is a function called segment scale compensate that we can turn on or off. What it does is, if the parent joint has a scale value other than 1, this child joint would automatically input the negative scale value underneath to prevent itself from scaling.

![[Joints-7.png]]

In the picture below,  all the index finger joints have their segment scale compensate turned off. And the index finger mesh is scaling along with the scale value of wrist joint.

![[Joints-8.png]]

Knowing this would help us work with maya rigging in a more predictable manner. 
The previous script with a little bit of change can be used to turn on or off this function for all the joints with one click.

![[Joints-9.png]]

One setting of the joint creation tool is called projected centering. Turning it on would allow us to create joint automatically at location where maya detects the mesh and calculates the position of the center.

![[Joints-10.png]]

When creating finger joints, because we want our wrist joint to have the same orientation of the elbow joint, we create those finger joints separately. As a result, finger joints rarely have the correct orientation by default. Moreover, we learned that thumb and other fingers need to curl correctly with the same axis. However, the orient joint tool is using the world axis as reference to place the local axis, so we cannot achieve that for the thumb and other fingers using the same orient joint setting. To make it worse, even joints in the same finger would have slightly different orientation with the same orient joint setting. This minor difference is enough to make the finger curl in a wrong way.

![[Joints-11.png]]

We know how finger curls. It would make sense when each finger curl, it should use the mesh as a visual reference to tell if the finger is curling the correct way. If it is not, adjust the rotation, then freeze transformation to zero out the rotation value. Usually the second last joint of the finger is the easiest to align. And if the upper joints of the finger need to align their orientations to it, we can use the same trick, where orient joint tool with orient joint to world turned on, would make the child joint align its orientation to that of the parent joint. So, we often unparent the second last joint, make the upper joint be its child, apply the trick, then restore the original parent-child relationship. This trick is not just for finger joints, any parent joint that needs to follow the orientation of its child joint can use this trick. 

In the end, the orient joint tool is not very helpful here, and we still need to hand adjust the orientation of each finger, and make them all curl correctly about the same local axis (if it is the default setting where x point to child joint, y point upward, then fingers would curl about z axis).