```C#
float h = CrossPlatformInputManager.GetAxis("Horizontal");
float v = CrossPlatformInputManager.GetAxis("Vertical");
// calculate camera relative direction to move:
m_CamForward = Vector3.Scale(m_Cam.forward, new Vector3(1, 0, 1)).normalized;

m_Move = v*m_CamForward + h*m_Cam.right;
//...more code
m_Character.Move(m_Move, crouch, m_Jump);
```
m_Cam is the transform component of the main camera. And Transform.forward returns a normalized vector representing the blue axis of the transform in world space. The difference between Vector3.forward and Transform.forward would be Vector3.forward is using the world axis expressed in world space, it almost always returns a vector of (0, 0, 1). The Transform.forward instead returns the local z axis expressed in world space. Another method Transform.right is very similar but it returns local x axis expressed in world space.

The Vector3.Scale method multiplies two vector component wise. So by multiplying the m_Cam.forward with a vector of (1, 0, 1), we are getting rid of the y-axis value and keeping the x-axis and z-axis values as the same. Anything times zero would get a zero, and anything times one would get its original value.

With a normalized method in the end, we are getting an unit vector that only has x-axis and z-axis values, representing the camera facing direction in x-z plane.

You may ask why the m_Cam.right doesn't get the same treatment as the m_Cam.forward does. This line of code expects the main camera doesn't have any rotation about the z-axis, and normally the main camera shouldn't. However, odds might still happen like a camera shake effect would produce unexpected result.

m_Move now has the combined result from two unit vectors and their corresponding factors, representing the direction to move the character.

We put those codes in fixed update, and in the end, we send those information through a method in another script to actually move the character. So the character movement is taken care in fixed update.

The method looks like this.
```C#
public void Move(Vector3 move, bool crouch, bool jump)
{
 // convert the world relative moveInput vector into a local-relative
 // turn amount and forward amount required to head in the desired
 // direction.
 if (move.magnitude > 1f) move.Normalize();
 move = transform.InverseTransformDirection(move);
 CheckGroundStatus();
 move = Vector3.ProjectOnPlane(move, m_GroundNormal);
 m_TurnAmount = Mathf.Atan2(move.x, move.z);
 m_ForwardAmount = move.z;
 ApplyExtraTurnRotation();
 
 // control and velocity handling is different when grounded and airborne:
 if (m_IsGrounded)
 {HandleGroundedMovement(crouch, jump);}
 else
 {HandleAirborneMovement();}

 ScaleCapsuleForCrouching(crouch);
 PreventStandingInLowHeadroom();

 // send input and other state parameters to the animator
 UpdateAnimator(move);
}
```
We add two unit vectors to produce our direction vector, so it can have a magnitude bigger than one. That's why we need to normalize it again.

The transform keyword is a keyword like this or value, and it is referring to the Transform component attached to this game object. So, when we call its InverseTransformDirection() method, we are converting the direction vector in world space into one in local transform space. There is a method called TransformDirection() that would take a vector in local space and return one in world space, effectively doing the opposite thing. Both methods work for a vector representing direction. If the vector represents a position, then we should use TransformPoint() and InverseTransformPoint() methods instead.

Now a vector can represent a point or a direction. The difference is the way we interpret them. When we treat it as a point, we take into the account of origin, so when we call `transform.TransformPoint(Vector3.up)`, we turn a vector from local space into world space. You may say Vector3.up returns a vector3 (0, 1, 0) in world space. Well yes, it returns a vector3 (0, 1, 0), and that does represent the y axis in world space. But it is just a vector, when we put it into the method, the method will treat it as a local vector. So, if the Transform component of this game object has a vector3 (5, 2, 1), and its axes are not rotated, the vector3 (0, 1, 0) will be transformed into a vector3 (5, 3, 1). Since a local vector3 (0, 1, 0) can be represented by a world vector3 (5, 3, 1).
Next, we talk about treating a vector as a direction. When we do, we don't care about the origin position, but we still care about the axes. Normally, a direction vector should be normalized for proper calculation. As long as the Transform component is not rotated, meaning the axes of the Transform component are pointing to the same directions as the world axes do, calling the method `transform.TransformDirection(Vector3.up)` will output the same vector3 (0, 1, 0). Even though the position of the Transform component might be something like (5, 2, 1), we just don't care because if the vector represents a direction, then as long as the local axes are the same as the world axes, a direction in local space is the same direction in world space. However, if the y axis of Transform component is pointing to the same direction as the world x axis does. Then, calling the method `transform.TransformDirection(Vector3.up)` will output a vector3 (1, 0, 0), sicne the local direction represented by (0, 1, 0) is the same direction represented by (1, 0, 0) in world space.

The CheckGroundStatus() method is using a common raycast technique to detect if the character is on the ground or not. If it is on the ground, then a boolean flag called m_IsGrounded is set to be true. Also the animator's applyRootMotion property is set to be true. Lastly, a vector3 property called m_GroundNormal is given the normal information where the place is hit by the ray. `m_GroundNormal = hitInfo.normal`. If it is not on the ground, both boolean properties would be false, and the m_GroundNormal is given the Vector3.up.

The Vector3.ProjectOnPlane() method is projecting the parameter vector onto a plane defined by its normal which is the second parameter. Then, it returns a new vector representing the position where the projection land on that plane. For this to work, we need to provide an origin, as the plane normal can define many planes if the origin is not given. But I am not sure how I can provide one by giving those two parameters. The method has some kind of way to figure out the origin underneath, that's for sure.
![[capture_01.png]]
Now, I am not sure why we need to turn `move` into local direction since the axes of the Transform component attached to the character are not rotated, we are getting the exact vector3 out of the `transform.InverseTransformDirection`, maybe in some extreme cases they do rotate? The `m_GroundNormal` is in world space, and represents direction. But still what does this method use to decide its origin? Maybe this has something to do with the `InverseTransformDirection()` method. Two direction vectors and no rotated axes, whether you change their spaces or not, they are the same. It is not using the game object's Transform component as the origin, because we are giving it two direction vectors and using the `ProjectOnPlane()` method under `Vector3` namespace. There is no local space origin information involved. It is just the origin part that confuses me. Anyway, judging from the result, it works, altering the direction if the surface beneath is not flat.

Next, we have the `Mathf.Atan2(move.x, move.z)` to get the turn amount in radians. This method works for 2D plane. Basically, it is getting the angle by reversing the tangent output, and the output is the first parameter divided by the second, so `move.x / move.z` here. Remember when we learn tangent 45 degree equals one? Reversing tangent output of one will give us 45 degree, or 270 degree, or 45 + 360 degree, etc. When we use move.x and move.z for this method, we are looking at the x-z plane from up top. And the angle we get is probably used to rotate the the y axis of the Transform component in some way.

`m_ForwardAmount = move.z` is straightforward since z axis is the forward direction.

```
void ApplyExtraTurnRotation()
{
  float turnSpeed = Mathf.Lerp(m_StationaryTurnSpeed, m_MovingTurnSpeed, m_ForwardAmount);
  transform.Rotate(0, m_TurnAmount * turnSpeed * Time.deltaTime, 0);
}
```
The `ApplyExtraTurnRotation()` would first decide a method local float called turnSpeed by lerping the two preset turn speed properties by the `m_ForwardAmount`. Those two properties are turn speeds for stationary and for moving. Since the `m_ForwardAmount` comes from a unit vector, its possible max value is one, and minimum value is zero. So it is suitable to be used as a factor in lerping. Once we get the turnSpeed based on how fast we move forward, we apply it directly in rotation like this: `transform.Rotate(0, m_TurnAmount * turnSpeed * Time.deltaTime, 0)`. According to the comment, this is an additional rotation works on top of the root motion rotation, to make the character turn faster. That's why we just jam all those variables into the `Rotate()` method, and tune it by adjusting the stationary turn speed and the moving turn speed.

Next, we use the private bool `m_IsGrounded` to check whether we should deal with the ground movement or the air movement. This boolean is modified when we called `CheckGroundStatus()`, so it should be updated. If it is true, so the character is on the ground, then we pass along the crouch parameter and jump parameter into the method `HandleGroundedMovement()`. If not, meaning the character is in the air, then we call the method `HandleAirborneMovement()`.

We look into the `HandleGroundedMovement()` , and see there is a big if statement to check if we are in condition to perform a jump. `if (jump && !crouch && m_Animator.GetCurrentAnimatorStateInfo(0).IsName("Grounded"))` is basically checking if jump is true, crouch is false, and the current animator state is named Grounded. `m_Animator` is the reference to the character's animator, hooked up in `Start()`. The animator's `GetCurrentAnimatorStateInfo()` method returns the current animator state, the parameter is the animatior layer index which starts at zero. This animator state object also has the `IsName()` method to check if the animator state name matches the string parameter. If all those conditions are matched, then we perform a jump, otherwise, it does nothing. Maybe the `HandleGroundedMovement()` should be renamed `HandleJumpMovement()` since there is no code related to crouching.

Next, how to perform a jump in code? Here, the example uses Rigidbody component's velocity property to do so. `m_Rigidbody.velocity = new Vector3(m_Rigidbody.velocity.x, m_JumpPower, m_Rigidbody.velocity.z)` The velocity property of Rigidbody component is a vector3 value, and it stores the rate of change of Rigidbody position in units per second, normally being meters, but can be others. Also, it is a world space direction, as you will find the value is referencing the world axes. Now, if you set a rigidbody's velocity to something other than (0, 0, 0), then the velocity should remain the same and the rigidbody would just do a constant movement, so moving at a direction at a constant speed. But there are a couple of things that would alter the velocity: the drag property of rigidbody component, the contact with other things like the floor plus the down force called gravity, and we might manually alter the velocity through code, etc. Therefore, our character which is under the influence of gravity and stands on a floor, would not do a constant movement after the jump code. The velocity.y will decrease to zero thanks to gravity, and the velocity.x and velocity.z will decrease to zero. These two are affected by gravity but the way they stop so quickly is because we manually set it in later code.
Other codes in this `HandleGroundedMovement()` method are settting the `m_IsGrounded` to false, disabling the root motion by `m_Animator.applyRootMotion = false`, and setting the `m_GroundCheckDistance = 0.1f` . The only place where it is not 0.1f is in the `HandleAirborneMovement()` method, I think here it is just setting it back to original value in case it is not.

Now, we can move to the other branch of the if statement, take a look inside the `HandleAirborneMovement()` metthod. It is even simpler. Basically, it is defining an extra down force to apply on top of the gravity to the rigidbody, so the character would go down faster without affecting the overall gravity. The way it does is to take the gravity and multipy it by a factor we define, then apply it to the rigidbody with `AddForce()` method. We can access the gravity through `Physics.gravity`, and I think it is a readonly property. The last part is modifying the `m_GroundCheckDistance`. It is asking if the `m_Rigidbody.velocity.y` is smaller than zero. If yes, then the character is in air going downward, so we set the `m_GroundCheckDistance` back to its original value of 0.1f. If not, then the character is in air going upward, so we set `m_GroundCheckDistance` to a much smaller value of 0.01f. This is the only place where `m_GroundCheckDistance` might be set to something other than the original value of 0.1f. Changing it to 0.01f would make sure the raycast never hit other colliders because the origin is 0.1f inside the character's collider, effectively making it impossible to set `m_IsGrounded` to be true. Why do we need this?
Also the code here is an inline ternary statement, `m_GroundCheckDistance = m_Rigidbody.velocity.y < 0 ? m_OrigGroundCheckDistance : 0.01f`. A shorter form of if statement.

After we deal with the jumping, we move on to the crouching. The example use the method called `ScaleCapsuleForCrouching()` to take care of the collider when crouching. First, we pass along the crouch parameter into this method. Inside, we check if `m_IsGrounded` and crouch are both true, if so we furthur check if our `m_Crouching` property is true. Yes being the character is already crouching, in that case we return and do nothing. No meaning the character is entering crouching state, so we need to set the collider smaller to appropriately fit the character. `m_Capsule.height *= 0.5f` and `m_Capsule.center *= 0.5f` are the codes to do that, then we set  `m_Crouching` to be true. Doing it this way would prevent the capsule from shrinking again and again. The shrinking would only happen once.

An else statement to branch the first if statement. Inside, we define a ray and a length. Then we use `Physics.SphereCast()` to detect if there is anything above us. This method uses its origin and radius parameters to simulate a sphere, then uses its direction and length parameters to send this sphere from the origin onto the path. If it hits any collider along the way, it would return true, and optionally return a RaycastHit object carrying detail information. In the example, we just use this as a boolean, if true, then we set the `m_Crouching` to be true and return. If not, we carry out the last part of the `ScaleCapsuleForCrouching()` method, which would be setting the capsule back to its original state by `m_Capsule.height = m_CapsuleHeight` and `m_Capsule.center = m_CapsuleCenter`. Also set the `m_Crouching` to be false. The `SphereCast()` method is for the situation in which the character is crouching to a place where we let go of the crouch button but the character cannot stand up because something like a rock is on top of the character. In that case, the `SphereCast()` would return true and tell the character to continue crouching. If the `SphereCast()` finds nothing, we set the capsule back.

There is also a method called `PreventStandingInLowHeadroom()` right after the method `ScaleCapsuleForCrouching()`. However, what is inside is just the `SphereCast()` and its parameters definitions. I comment out the method and test it. The character seems behaving just fine, no standing up in low headroom even though the method is gone. No surprise there since I think it is integrated into the `ScaleCapsuleForCrouching()` method.

Finally, we hit the bottom of the `Move()` method. `UpdateAnimator()` method is called with the move parameter passed into it. Remember the move property is a direction vector altered by `ProjectOnPlane()` method. Inside the `UpdateAnimator()` method, we first see all the animator parameters gets updated with `SetFloat()` method and `SetBool()` method. The `SetFloat()` method also takes in a damp time parameter and a delta time parameter. The damp time is the time in second for the current value to get to the target value. So, 0.1f would mean the current value would get to the target value in 0.1 second. With this, our character would not instantly snap to the target position, but get to that in a natural fashion. And we use delta time to decide how much of that 0.1 second is reached in this current frame. One more thing, we see the animator parameter called Forward, and another one called InputX. InputX and InputY parameters are used in blend tree for strafing while Forward and Turn parameters are used for the rest. Why? I don't know, it seems to me we can get rid of the InputX and InputY by replacing them with Forward and Turn since they would have the same values anyway.

If `m_IsGrounded` is  false, we set the Jump animator parameter to be equal to the rigidbody's velocity.y. And there is also a JumpLeg animator parameter used by the blend tree for airborne animation to decide which leg is in front and by how much. First we need to know what position we are at during the run cycle animation. `runCycle = Mathf.Repeat(m_Animator.GetCurrentAnimatorStateInfo(0).normalizedTime + m_RunCycleLegOffset, 1)`. This `Repeat()` method works like modulo operator, but it can work on float. Let's say `Repeat(1.5, 1)`, we would get 0.5. `Repeat(5, 2.5)` would return 0. It returns the part that cannot be divided into an integer. Now, we know the first parameter is the current aniamtor state in noramlized time plus some offset we define, and the second parameter is one. So we are only possible to get a value ranging from 0 to 1. Why not just use the animator state's normalized time directly? I don't know. Next, we get another local float `jumpLeg = (runCycle < k_Half ? 1 : -1) * m_ForwardAmount`. The `k_Half` is a constant equal to 0.5f, and by that we are asumming 0.5 is the normalized time spot in run cycle animation when the character switches leg. We are either getting 1 or -1 out of the ternary statement, and using the result to time the `m_ForwardAmount`. The ternary statement result decides which leg is in front, and times it with the `m_ForwardAmount` would answer the by-how-much question.

There is also a property we assign to the `m_Animator.speed = m_AnimSpeedMultiplier` to control the speed of our animations on ground. We use `if (m_IsGrounded && move.magnitude > 0)` to check if the character is on the ground and moving. If so, we apply the speed multiplier. If not, we set it back to the default value of 1.