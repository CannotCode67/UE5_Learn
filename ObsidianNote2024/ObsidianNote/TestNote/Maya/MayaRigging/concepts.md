What is a rig?

Rig = following:
Skeleton (made by joints) 
^ controlled through point&rotate constraints/utility node/key driven editor
Rig joints (made by fk&ik joints) 
^ controlled through point&rotate constraints/utility node/key driven editor
Anim curves (made by NURBS shape)
^used to push out the translate value of the anim curve so it can use zero out to reset
^ controlled through parenting
Transform groups (made by group node)


Skeleton is the joints and their hierarchy structure, which the mesh is binded to. 
Most joints should have zero rotation values.
Child joint should only have translate X value, which is called the bond length.
In order to adjust child joint for more than one direction, the parent joint should rotate instead of moving the child joint in translate Y or Z direction. After adjusting, freezing rotation on the parent joint would clear out the rotation value.

Freeze Transformation on joint would:
-push the rotation value to joint orient attribute
-translate value don't get zeroed out

Freeze Transformation on transform node (group node):
-rotation values are reset to world space
-translate values are pushed to world space pivot attribute


Maya rotate order: z > y > x
Meaning: rotate Z axis will affect both y and x axises, rotate y axis will affect x axis, rotate x axis will only affect itself.

Gamble lock meaning: axises overlaping.

Rotate y axis can easily cause gamble lock, so this axis should be put into the direction where the mesh is unlikely to move.




