
`GetName()`
API above returns the name of the object in FString value.

`AActor.GetActorNameOrLabel()`
API above returns the name of the Actor in FSting value.

`GetWorld()->SpawnActor<AProjectile>(ProjectileClass, ProjectileSpawnPoint->GetComponentLocation(), ProjectileSpawnPoint->GetComponentRotation())`
API above is the way to spawn an actor into the level at position and rotation specified. We type the C++ class into the angle bracket, and put UClass in as the first parameter. All blueprint classes are UClass. This is something similar to Instantiate() in Unity.

`Destroy()`
API above is an actor API, which would destroy this actor from the level.

`SetActorHiddenInGame(bool)`
API above would hide the actor based on the boolean parameter.

`SetActorTickEnabled(bool)`
API above would enable or disable this actor's `Tick()` function based on the boolean parameter.

`Actor->ActorHasTag("SomeTag")`
API above returns a boolean value to signify if the actor has a tag equaling to the parameter string. We can set an Actor's tag by searching for tag in the detail panel to get to the tag section. Unreal allows us to add multiple tags to one actor, so we can press the add button to add as many as you want.

`Actor->Tags.Add("SomeTag")`, `Actor->Tags.Remove("SomeTag")`
The Actor's tags membr field is essentially a TArray object of FName. So we can use TArray built-in methods to add and remove tags as we want. FName is one of the string types provided by Unreal. For more information, please search string handling in unreal documentation.

`Actor->SetSimulatePhysics(bool)`
API above can set the actor to be under the influence of physics or not, given that the actor has a UPrimitiveComponent attached.


Attach one actor to another actor in runtime
`Actor->AttachToComponent(USceneComponent, FAttachmentTransformRules)`
API above would attach the Actor to the USceneComponent in parameter. Usually, we use the USceneComponent from the target actor.

`Actor->DetachFromActor(FAttachmentTransformRules)`
API above would detach the Actor from any USceneComponent that it is currently attached to.

From within the actor, get the root component
`Actor->GetRootComponent()`
API above returns the RootComponent member field of the actor. The actor class has a member field called RootComponent, and it is a pointer of type USceneComponent. By default, it should point to a USceneComponent. USceneComponent is the transform component in Unreal. Almost all actors have such a component. And we can assign a different component to this RootComponent field, as long as the component is from a class derived from USceneComponent. Simply `RootComponent = CapsuleComponent`, but of course we need to create this `CapsuleComponent` before we can assign it.


From within the actor, get a hold of certain component
```
UPhysicsHandleComponent* physicsHandle = GetOwner()->FindComponentByClass<UPhysicsHandleComponent>();
```
Code above shows we can invoke the method called `FindComponentByClass<class/typename>()` through the actor reference/pointer. This method is a template in C++. A template in C++ is similar to generics in C#, but a template can parameterize not just type, but also variables, which makes it more powerful than generics in C#. Now, in here, it is used as generics as we are asked to fill in the angle bracket with the type. And what we get in return is the memory address of that component. This is how we get reference of a component from within another component. Unity's process is easier, as we don't need to invoke similar functions through the game object, we can invoke them directly in component.

From within the actor, create a certain component
`CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Cube"))`
API above creates a child object under the parent object, and child object is of UStaticMeshComponent type, and has a name of Cube. This API returns the memory address of the child object. This can only be called in the actor's constructor, as Unreal looks for this method only in that phase.

From within the component, get a hold of the actor
`GetOwner()`
When the C++ class inherits from actor component class, not actor class, so the class object represents an actor component, not an actor. In that case, we can get the memory address value to the actor which owns this component by using the above API. We use a pointer to contain the return value.

`GetComponentRotation()`, `GetComponentLocation()`
APIs above return the component's rotation in FRotator type, the component's position in FVector, given this component has a transform component.

`USceneComponent::GetForwardVector()`
APIs above return the normalized vector representing the forward direction of the component. Maybe it can be used directly in actor as well? Not sure.