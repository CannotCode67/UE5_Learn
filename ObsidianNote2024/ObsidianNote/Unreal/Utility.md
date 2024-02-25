
Logging

`UE_LOG(logTemp, Display, TEXT("String Value"))`
API above is actually a C++ macro, and it can output text message to the output console. The TEXT() itself is another C++ macro, and it can accept variables to use as output like this: `UE_LOG(logTemp, Display, TEXT("float Value: %f"), 3.17f)`. When it includes different types of variable, it needs to use different format letter. Below is an exmaple. `UE_LOG(LogTemp, Warning, TEXT("Text, %d, %f, %s"), intVar, floatVar, *FStringVar);`

You may notice `TEXT()` macro cannot handle FVector, FRotator, etc. Unfortunately, there are built-in functions called `ToCompactString()` in each class to convert them into FString.

There is also another way to log in C++.
```
if (GEngine)
{
	GEngine->AddOnScreenDebugMessage(1, 60.0f, FColor::Cyan, FString("Print!"));
}
```
Code above prints message onto the screen, not the output console. The first parameter is the key value, meaning all the message sharing the same key would share the same message slot on the screen. The later message would replace the older one. While having different key values, messages can appear on the screen at the same time, just in a different position of the screen. The second parameter is seconds to live there if not replaced.


SceneView
Unreal doesn't have a scene view when entering play mode. To get something similar, Unreal offers a shortcut of F8 key to turn game view into scene view.

Drawing DebugGeo

`DrawDebugLine(GetWorld(), FVector1, FVector2, FColor)`
API above draws a debug line from FVector1 to FVector2 when we are inside play mode. There are overload versions taking in other parameters like how long for the line to be shown. Check the API reference.

How to do a RayCast in unreal
```
FCollisionShape sphere = FCollisionShape::MakeSphere(Radius);
FHitResult hitInfo;
bool geoCastHit = GetWorld()->SweepSingleByChannel(hitInfo, FVector1, FVector2, FQuat::Identity, ECC_GameTraceChannel2,sphere);
```
Code above, we invoke the `SweepSingleByChannel()` through UWorld object. This API returns the true if it hits any collider. We need to use a FHitResult object to store the collision details. The geo it uses to sweep is a FCollisionShape object. The sweeping path is from FVector1 to FVector2. The ECC_GameTraceChannel2 is the trace channel, and we can find it through the project file->config file->open the DefaultEngine.ini file. Then, search the name of your defined trace channel. On that line, the Channel value is what we use to put into the API. Now, there must be a better way to get this information. But this is an overview of the geo cast in Unreal. The FHitResult object has the ImpactPoint field to store the actual collision point, like hitInfo.point in Unity.


`DrawDebugSphere(GetWorld(), FVector, radius, segmentNumber, FColor)`
API above draws a debug wire sphere at FVector position when we are inside play mode. The raidus defines the radius of the sphere, and the segmentNumber defines how many segments this sphere has. There are overload versions taking in other parameters like how long for the sphere to be shown. Check the API reference.

```
FHitResult.GetComponent()->WakeAllRigidBodies();
// line above would wake the rigidbody in case it is in sleep.
// rigidbody would go into sleep if there is no interaction going on for some time.
// it is to save performance.
UPhysicsHandleComponent->GrabComponentAtLocationWithRotation(
	FHitResult.GetComponent(),
	NAME_NONE, // to specify bone name
	FHitResult.ImpactPoint,
	GetComponentRotation()
	);
```
API above is member function on UPhysicsHandleComponent class. It allows us to grab a target actor in the way defined by the parameters. Check API references.

Referencing blueprint-created components in C++
C++ variables for components are pointers, and we need to create the components with codes either in constructor or outside the constructor. Those codes would return pointers to store in our C++ variables. Without the creation process, our variables are nothing but some empty pointers. Now, we can create components in blueprint, but there is no way to retrieve those components in C++ since we create C++ first then create a blueprint based on that. To deal with this situation, Unreal offers a solution which is FComponentReference.
In header file.
```
UPROPERTY(EditAnywhere, meta = (UserComponentPicker))
FComponentReference Ref_raycastStart;

UPROPERTY(EditAnywhere)
USceneComponent* raycastStart;
```
In cpp file.
```
void ANPC::BeginPlay()
{
	Super::BeginPlay();
	raycastStart = Cast<USceneComponent>(Ref_raycastStart.GetComponent(this));
}
```
Above it is the setup in C++, of course we need to create the actual component in blueprint and assign the component's name to the FComponentReference variable. Notice, we are extracting the value from FComponentReference, but we are doing it in BeginPlay(), not constructor. Because by the time we have edit the blueprint, the constructor is already called.