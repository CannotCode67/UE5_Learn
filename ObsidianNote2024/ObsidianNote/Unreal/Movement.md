`AddActorLocalOffset(FVector)`
API above is a method of actor class, and it moves the actor using FVector as local vector to add to the current position. So FVector is a delta vector in local space.
```
void ATank::Move(float value)
{
	FVector deltaPos = FVector::ZeroVector;
	// FVector deltaPos(0.0f); // this is a faster way to create FVector
	deltaPos.X = value;
	AddActorLocalOffset(deltaPos); //this would move at direction of local x-axis
}
```
As you may recall, if we want our movement to be frame rate independent, we need the deltaTime. In Unreal, I suppose every input generates an event, and the `ATank::Move` subscribes to those events through the binding API. How frequent an event can be fired might not be one to one to the frame rate, but is highly related, I think. So, if we press the w key and hold it, the frequency of calling `ATank::Move` is the frequency of the key event firing, that is for sure. And therefore, even though the input system works through callback, it is still somewhat correlated with the frame rate just like we put our input detection in Update loop in Unity. Unreal's Update loop is called Tick(), and it has a deltaTime parameter, but the `ATank::Move` is invoked through callback, not inside Tick(). How do we get the deltaTime?

```
void ATank::Move(float value)
{
	FVector deltaPos = FVector::ZeroVector;
	// FVector deltaPos(0.0f); // this is a faster way to create FVector
	
	deltaPos.X = value * speed * UGameplayStatics::GetWorldDeltaSeconds(this);
	
	AddActorLocalOffset(deltaPos); //this would move at direction of local x-axis
}
```
Code above shows the API `UGameplayStatics::GetWorldDeltaSeconds(this)` to get the delta time from outside of Tick(). The parameter is any UObject that's in the world we are associated with, so passing in this, the ATank instance would do it.

`AddActorLocalRotation(FRotator, bSweep)`
API above rotates the actor using FRotator as the delta rotator in local space. FRotator is a type representing rotation using rotation degress in three axis like Euler rotation. The other class for representing rotation is FQuat which uses Quaternion. There is a version of `AddActorLocalRotation()` which takes in a FQuat, and you can imagine just about any function dealing with rotation would have one version for FRotator and one version for FQuat. The second parameter by default is false, or we can specify it explicitly. It is a flag asking if this rotation should take into account of physics, so collision. We can set it to true, and it would work only if we have enabled the collision for the root component because it only checks collision for root component, child components are not checked. There is the same bSweep parameter in `AddActorLocalOffset()` as well.
```
void ATank::Turn(float value)
{
	FRotator deltaRotation = FRotator::ZeroRotator;
	deltaRotation.Yaw = value * turnSpeed * UGameplayStatics::GetWorldDeltaSeconds(this);
	AddActorLocalRotation(deltaRotation, true);
}
```
Worth mentioning, FRotator class names its y axis (right axis) as Pitch, its z axis (up axis) Yaw, and x axis (forward axis) Roll. That's why we are accessing `deltaRotation.Yaw` instead of `deltaRotation.y`. And this `ATank::Turn()` function is invoked by input event just like `ATank::Move()`, so we also need to do key binding for it as well.
```
void ATank::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	PlayerInputComponent->BindAxis(TEXT("MoveForward"), this, &ATank::Move)
	PlayerInputComponent->BindAxis(TEXT("Turn"), this, &ATank::Turn)
	
} //&ATank::Move is known as function pointer, memory address to a function.

void ATank:Move(float value)
{
	// do whatever we want with the input value
}

void ATank::Turn(float value)
{
	//...
}
```
There are also `AddActorWorldOffset(FVector)` and `AddActorWorldRotation(FRotator)` as well, but in those functions, FVector and FRotator are treated as they are in world space.

`SetActorLocation(FVector(x, y, z))`
API above can set the position of the actor. Beware that this API works only when the Actor does have a transform component. Yes, an Actor in Unreal doesn't necessarily have a transform, although having one would make it practical to be used. The FVector taken in is treated in world space.

`GetActorLocation()`
API above can get the position of the actor in world space given there is a transform(USceneComponent) component.

`GetActorRotation()`
API above returns the rotation of the actor in world space given there is a transform component, and the return value is of a type called FRotator.

`SetActorRotation(FRotator)`
API above can set the rotation of the actor given there is a transform component. Beware, the rotation in Unreal is just like it in Unity, using quaternion underneath. Check the API reference for its parameters. The FRotator is treated in world space

`USceneComponent->SetWorldRotation(FRotator)`
API above would rotate the USceneComponent to match the FRotator, and since the FRotator is in world space, we use this API. The USceneComponent is normally the root component, so the whole actor would turn. This would snap the actor's rotation to FRotator, and if we want a smooth transition, we need the FRotator to be interpolated.


`MovementComponent->AddMovementInput(GetActorForwardVector(), floatValue);
API above works directly with the movement component to apply movement to our actor, instead of manipulating the USceneComponent. However, we have to make sure the actor has a MovementComponent or component derived from it.

`Controller->AddControllerYawInput(float)`
API above would rotate our controller. Why would we want to do that? Well, controller only has rotation, and its rotation origin is usually at the position of the camera. When we rotate our controller, everything including the camera are rotating using the controller's rotation origin as the origin. This is one way to get the camera rotated just like what we try to achieve with rotating the Ref_Camera_lookAt in Unity. However, it would not work as everything are rotating at the controller's origin, including our character. What we want is the camera rotates using the character pawn as the origin. The spring arm component has a boolean setting called UsePawnControlRotation, if we check that, we would get the behaviour we want.



