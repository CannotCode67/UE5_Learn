We studied connection before, and it maps the attribute of the driving object to those of the driven object in an absolute 1 to 1 fashion. We already discuss the drawback of this as it cannot hold a relative relationship in translation. But for rotation, since both the control and joint have zero rotation value at the start, it seems connection is viable. Still it is somewhat limited. We would use an example to show the limitation, and after that, we introduce the set driven key as the solution to the problem. 

In the example below, we want to use the rotation x value of the control to curl the finger through affecting the rotation x values of each joint. A direct connection is applied, and it works. However, each joint has exactly the same rotation value, but we want the joint further down the line to have bigger rotation value as this would better represent the reality.

![[SDK&UtilityNode (1).png]]

Connection simply cannot do that, so we use set driven key. To open the tool, we need to set the menu set to animation. And it is under the key menu -> set driven key -> set. We then load the driving object and the driven objects, select the corresponding attribute by clicking it.

![[SDK&UtilityNode (2).png]]

Next, we make sure the control and the joints have zero rotation x value, then press key button. This would produce a start point where we tell maya that when control has zero rotation x value, joints should have zero rotation x values as well. Now, we can work on the end point where we tell maya when control has rotation x value equal to 40, the first joint has that of 20, the second joint has that of 50, and the third joint has that of 70. After we set all those values, we press the key button again. Now, the control has affect on those joints, but to a different degree. 

We can use the graph editor to further edit this motion. The graph editor is under the windows->animation editors->graph editor. With the graph editor open, we select all those joints, and we can see the animation curve. By default, maya uses the ease in and ease out graph, which slows down the motion in the beginning and the end, giving a natural feeling, just like how we as human perform the movement.

![[SDK&UtilityNode (3).png]]

Changing the animation curve to linear tangent means the movement maintains the same speed all the way, and this gives a rigid feeling, just like a robot or machine. In addition, we can change the keyed value directly in the graph editor, saving us trouble of resetting the key. 

This animation is not bound to the timeline obviously. However, a rig should not be touched by an animator, and if the effect we set in the graph editor is not what animator wants, then the animator can either alter this animation curve ending up altering the rig, or accepts this and move on. So we can see, this is somewhat rigid. A more flexible solution is the utility node, and that is what we are going to study. 

In this example, we have a foot roll system. In this system, the control has a custom attribute called roll, and essentially we want the joints to tilt back when roll is smaller than zero, and we want the joints to do a roll action when roll is bigger than zero.

![[SDK&UtilityNode (4).png]]
![[SDK&UtilityNode (5).png]]

Obviously, we can achieve this with SDK as well, but let's use utility node instead. With the node editor open, bring the control and the joint into it. Pressing tab in the empty space of node editor would initial a search, very much like what the space key does in shader graph or substance designer. And the utility node we use here is condition node. Click the node, and we can see the condition attribute in attribute editor. Basically, it works like a if statement, comparing the first term and second term, and returning a value based on the relationship between both terms.

![[SDK&UtilityNode (6).png]]

Remember we want the foot to tilt back if roll is smaller than zero, so basically, it means if roll is < 0, then the roll value is mapped into the rotation x value of the joint. We need to plug the roll into either the first term or second term, then give the other term a value of zero. Set the operation to less than or bigger than depending on the order. Here we plug the roll into the first term, so second term is zero, meaning the operation being less than would return a true. Finally, we set two output values, one is the roll value for returning true, and the other is zero for returning false (zero means no effect).

![[SDK&UtilityNode (7).png]]

As we can see, it works but the rotation is going the wrong way. So we need to inverse the value. Another utility node called multiplyDivide is used here to multiply (selected operation) the input1 (output from condition node) by the input2 (set to -1 by us in the attribute editor). Those values can be set in the channel box as well.

![[SDK&UtilityNode (8).png]]

Now, we work on the part where roll is bigger than zero. Bring the ball joint into the editor. Use one more condition node to perform the logic, and outputting it to the multiplyDivide node, but this time the input y because we need to inverse this value as well. Alternatively, we can use a brand new multiplyDivide node, it doesn't really matter.

![[SDK&UtilityNode (9).png]]

Lastly, we work on the toe part, because in roll action, toe would start to rotate when ball is rotated to certain degree. So, the condition for toe is if roll is bigger than some positive value (10 here), it is true to output the roll value.

![[SDK&UtilityNode (10).png]]

But setting up this way, the starting rotation value of toe would always be 10, but we want it to rotate from zero. So it comes another utility node called plusMinusAverage. It does plus or minus or averaging operation upon command. This node has a bug where the input slot is now shown until we use plugged value. So we plug our roll into input 1D[0] and input1D[1], then unplug the input1D[1] so we can type a custom value. We also need to select the operation type, subtract is the one here.

![[SDK&UtilityNode (12).png]]

So far, the foot roll system is in shape, but the ball should not be rotated further once it has rotated to certain degree. This example is for demonstrating utility node, and later a complete foot roll system will be explained in detail. 

The beauty of this method over the SDK, is all those custom values we type in can be driven by attributes. And the example below shows a set of custom attributes that are used to define or modify those values. And animator can use those values to achieve flexibility while maintaining the integrity of the rig.

![[SDK&UtilityNode (11).png]]

One important thing to know is when we load select objects into the node editor, if we don't need them in the graph anymore, we need to use the remove selected nodes from graph button in the node editor. Because if we simply delete the node in the graph, the node is actually deleted as well, not just removed from the graph.