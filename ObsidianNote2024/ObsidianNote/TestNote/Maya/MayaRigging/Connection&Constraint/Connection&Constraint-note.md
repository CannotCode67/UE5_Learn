Because when freeze transform of a joint, its translation values don't get zeroed out, we use a nurbs shape as a control to manipulate our joints. A nurbs shape with a freeze transform does get all the values zeroed out. The bonding or connection between a nurbs shape and a joint is what we are going to study. 

There are two ways to go about building this kind of bond. One is called connection, and the other is called constraint. They are different at  

One way to create connection is to use connection editor.

![[Connection&Constraint (1).png]]

The way to use it is: first select the driver object and press reload left, and select the driven object and press reload right; second click the attribute in the left side to choose the driving attribute, and click the attribute in the right side to choose the driven attribute. If successful, we can see there will be yellow highline next to the connected attributes in the channel box.

![[Connection&Constraint (2).png]]

The second way is to use the node editor.

![[Connection&Constraint (3).png]]

When we open the node editor, it is empty, and we need to manually add our selected objects into the editor by pressing the button shown below.

![[Connection&Constraint (4).png]]

They looks like this. We don’t need the nurbs-circle-shape node because it holds information that is not relevant to this example. A nurbs object contains two nodes: the shape node contains the shape-related info, the other one is transform node (the icon is the same as a transform info). Joint only has one node because there is only a little extra info in it besides the transform information. Think of it as like components in game object of Unity.

![[Connection&Constraint (5).png]]

To see what information each node contains, we can expand the nodes by pressing the red underlined buttons.

![[Connection&Constraint (6).png]]

Then connect the output of a driving node to the input of a driven node, just like how we do in the shader graph of Unity. We can see the yellow highline next to the connected attributes in the channel box as well.

![[Connection&Constraint (7).png]]

Connection would map the connected attribute of the driving object to the one of the driven object in an absolute 1 to 1 fashion. They are not relative. For example, a freeze-transform nurbs object would have zero translation values although it is not in world space coordinate. And a joint with some translation values got connected by the nurbs object at translation. The result would be the joint gets move to the origin of the world space coordinate (0, 0, 0), which is wrong.  

Think of it this way, when a nurbs object get freeze transformed, it changes itself from the world space coordinate into its object space coordinate, and the origin of its object space coordinate is the pivot point. But a joint doesn't do that, a joint always use world space coordinate (and its pivot point is fixed in the middle of the joint). This is just a guess, because a joint does get its rotation value reset without changing the rotation back when freeze transform, which proves there is more details underneath. 

We know the connection doesn't hold a bond that's relative. That's why we need constraint. 
We can find constrain menu when the menu set gets changed to rigging. And as we can see, there are so many types of constraints.

![[Connection&Constraint (8).png]]

We'll first study the parent constraint. We select the driving object, then shift-select the driven object, then click apply. If successful, we can see the constrained attributes are highlighted blue. 

The maintain offset option is the key to maintain a relative bond. With this turned off, we end up with a result just like what we get from connection. Remember we can build a parent-child relationship between objects, and that has the same result. The reason we use a constraint instead is because we want the joints hierarchy to be outside of controls hierarchy. Otherwise, moving a control would move a joint plus any child joint and child joint's controls. But each control should maintain its own independency unless we want otherwise. And constraint even works for more than two objects. A example would be the joint which the geometry is skinned to, is constrained with two other joints, one is IK joint and the other is FK joint. And there is a control to adjust the current effect weight to determine whether the base joint is under the effect of IK joint or FK joint.

![[Connection&Constraint (9).png]]

Point constraint is for translation only. And rotation values of the driving object has no effect on the driven object. This is not equal to the parent constrain with only translation turned on. The parent constrain with only translation turned on, still have effect when the driving objects have non-zero rotation values. The effect is rotation values of driving object would change the translation of the driven object, but the distance between them would remain the same. We should beware of this.

![[Connection&Constraint (10).png]]

Orient constraint is for rotation only. And the translation values of the driving object has no effect on the driven object. This is equal to parent constraint with only rotation turned on. 

Scale constraint is for scale only. Remember the scale value on a joint only affects the geometry that is skinned to this joint. 

To cancel the constraint, we go to the outliner, and delete the constraint object that locates under the corresponding joint. 

Aim constraint is acting like the driven object is looking at the driving object.

![[Connection&Constraint (11).png]]

Aim vector defines which one of the driven object's axis to be used as a ray cast at the driving object. And the up vector defines which one of the driven object's axis to point up. And world up type and the world up vector are used together to define which direction is up. Very much like the orient joint tool. The setting in the picture above would give us the result where x axis is used to point toward the control, and y axis is matching up the direction of the world y axis.  World up type has a couple of options. The scene-up option means using the upward axis in scene coordinate (by default maya uses y axis). The vector option means you can define the world up vector by yourself in world up vector area, and using y axis is just the same like scene-up option. 

Let's look at the problem with scene-up option or defined vector option. They are okay if the controls are not going to move too much.

![[Connection&Constraint (12).png]]

But once the controls move too much, causing the driven objects to rotate too much, the axis is flipped. The reason is we tell the constraint to match the x-axis to the y axis in the scene, in this example. And once the driven object rotates too much, causing the x axis to point down, the constraint would flip the axis to maintain the x-axis point upward. Flipping would cause the object to rotate 180 degree about y axis in this example. For an eye, it might not be noticed, but in general, this is not acceptable.

![[Connection&Constraint (13).png]]

The third option is object-up option. And we needs to specify a world-up object as guidance.

![[Connection&Constraint (14).png]]

In the example above, it uses the control as the world-up object, which is not ideal. The world-up object should be a different object. 

The fourth option is object-rotation-up option. And we needs to specify a world-up object as well as a world up vector. It is using the world up vector but converted to the world-up object model space. If we are using the control as the world-up object, this is the option to go.