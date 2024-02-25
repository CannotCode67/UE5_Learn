
Creating new class in Unreal vs. in Unity
To create a C++ class, we go to top menu select Tools->New C++ Class. Then, we have a window open, allowing us to select a parent class to inherit, or we can inherit from no parent class at all. By selecting one of the provided parent class, our class would by default have some useful functionalities to be used by the Unreal Engine. Similar thing in Unity would be the MonoBehaviour parent class that we would get by default whenever creating a new script. But Unreal provides multiple parent classes for selection, so that we have more control on what we get given we know those parent classes well.

UCLASS() macro
When we create a new C++ class, and we want it to be interacted with the engine in some ways, we need to put a `UCLASS()` macro on top of the class declaration in header file. And inside that macro, we can specify how we want it to interact with the engine. For example, `UCLASS(meta=(BlueprintSpawnableComponent))` would allow the class to be used as component in blueprint.

Declare a class member field in Unreal, the UProperty() macro
Like Unity, when we declare a variable and want it to interact in some way with the engine, we add attribute on top. A example would be `[SerializedField]`, so that the editor can show it. UProperty() is essentially a macro in C++, and it is the way Unreal chooses to specify how this variable to interact with the engine. We can define metadata and variable specifiers here. A example would be `UProperty(EditAnywhere)` to allow the below variable to be shown and edited in the editor. The UProperty() macro reveals the variable to the Reflection system of Unreal, and one of the benefits comes with that is the garbage collection. We no longer need to do manual memory management for this variable. Since unlike C#, the language implements garbage collection already, C++ does not.

A few UProperty() specifiers to know
![[UPropertySpecifiers.png]]
When we create a C++ class, and then further create a blueprint class derived from it, the fields defined in the C++ class would need above specifiers to control the access from the blueprint. For blueprint class, we edit them in blueprint window, and there are two sections where we can handle fields: the detail panel and event graph. And detail panel can also be found in the instances that are already placed inside a level. We can reference the table above to use the appropriate specifiers to get the accessibility we want for those fields. For detail panel, those fields just show. For event graph, we need to type the field name to add the getter node or setter node based on the accessibility of that field. If we want a field to be read-only accessed through both detail panel and event graph, we need to write this `UProperty(VisibleAnywhere, BlueprintReadOnly)` on top of our field. Also since the blueprint class is a child class, only non-private fields in parent class are possible to be reached. But we can use `UProperty(meta = (AllowPrivateAccess = "true"))` to provide access from the blueprint window. You might think this is somewhat contradict to the private rule, but at least you need to know it to read other people's codes. For fields like pointers, all we need is the Read accessibility to modify the object in detail panel. The write accessibility is referring to the ability to modify the memory address, which is not what we want usually.

More UProperty specifiers
Specifiers above provide control on accessibility, and there are ones for other things. For example, `UProperty(VisibleAnywhere, BlueprintReadOnly, Category = "SomeCategory")`. The `Category` specifier would tell blueprint window to group all the fields under the same category together. It is helpful for organizing the detail panel. Also, in event graph, we can now type the category name instead of field name to show related nodes.

Actor in Unreal, GameObject in Unity
If we create a new class derived from Actor class, then we can put the C++ class object directly into the level. However, one practical way to do it is to create a blue print class object from the C++ class object, then bring the blue print class object into the level. Blue print is like prefab. And when a blue print is brought into the level, it becomes a blue print instance, at the same time it is an actor as well. So, C++ class can be integrated into Actor in this way as if the actor is inheriting from C++ class. Another way is to add C++ class onto Actor as if C++ class is defining a component. We select any actor in the level, and press the Add button in detail panel, then select add C++ class component. This way, it is more similar to the script in Unity, where we add script onto game object, and bring game object into the scene. 

TraceChannels in Unreal, PhysicsLayer in Unity
We can go into the Setting->ProjectSetting->Collision to find the section for TraceChannels. TraceChannels is just layermask.

Expose C++ functions to BluePrint
We can define custom functions in C++ and put them to use in blue print to extend the functionalities of blue print. The way to do it is to put a `UFunction(BlueprintCallable)` macro on top of whatever function we want to expose. The example does this in the header file, not sure if we can do it in the cpp file instead.

```
UFUNCTION(BlueprintImplementableEvent)
void StartGame();
```
The `UFUNCTION()` macro with the `BlueprintImplementableEvent` specifier would allow the below function `StartGame()` to be implemented in blueprint editor. So we can just declare it in the header file, and invokes it in the cpp file without implementation. This function in C++ simply provides the execution pin to the blueprint when it is invoked. And the implementation logics we make in blueprint would be executed. Here we do this in the C++ ToonTankGameMode class. And the blueprint class derived from ToonTankGameMode class will implement it in the blueprint event graph.

UActorComponent
UActorComponent defines the functionality enabling the attachment between component and an actor. It has no transform. So, it is useful for something like health system where transform is no use. This kind of component when added in blueprint, it shows up in the lower section unlike components that have transform.

SceneComponent in Unreal, Transform component in Unity
USceneComponent is the C++ class name for scene component, and the scene component is derived from actor component. Actor component's C++ class is UActorComponent which defines the functionality to attach the component to an actor. And scene component has a transform and supports attachment, just like transform in Unity.

PrimitiveComponents
Primitive components are scene components that contain or generate some sort of geometry to be rendered or used as collision data. ShapeComponents, StaticMeshComponents, SkeletalMeshComponents are all PrimitiveComponents. ShapeComponents such as UCapsuleComponent, or UBoxComponent, etc. generate geo used for collision detection but are not rendered. StaticMeshComponents and SkeletalMeshComponents contain pre-built geo that is rendered and can be used for collision detection as well.

UPhysicsHandleComponent
This is an actor component we can add to give the actor the functionality to apply physics interaction with a target actor such as grabbing the target actor.

SpringArmComponent and CameraComponent
Above two components are used together to apply a camera to our character. The SpringArmComponent should be attached to the root component, and camera component should be attached to SpringArmComponent. If we need to adjust the camera to modify the view, we adjust the spring arm component as this is the component Unreal design to handle camera related movement, as its name suggests a spring-like arm.

PawnClass and accepting player input
```
void ATank::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	PlayerInputComponent->BindAxis(TEXT("MoveForward"), this, &ATank::Move)
} //&ATank::Move is known as function pointer, memory address to a function.
void ATank:Move(float value)
{
	// do whatever we want with the input value
}
```
The pawn class is a derived class of actor class, and it has a functionality of accepting the player input, thus it is the class choice for player character or character controlled by AI. When we create a C++ class derived from the pawn class, automatically there is a method called `SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)`. We need to do two things inside this method: the first thing is to invoke the base version of this method and pass in the `PlayerInputComponent;` the second thing is to bind the input. To invoke the base version, we use `Super::` as the class. To bind the input, we call the appropriate methods through `PlayerInputComponent`. The binding only works when we already setup the mappings. We can go to project setting->input, then we see action mappings and axis mappings. That's the place to setup what input can generate what value.


ActorComponent and SceneComponent
One way to categorize all the components we can add onto an actor is to divide them into actor component and scene component. Scene component's class is USceneComponent, and it main purpose is to provide a transform functionality. Any class derived from it would also has their own transforms. When they are added onto the actor, they are added as sub-objects. Actor component has no transform functionality, and when they are added onto the actor, they are directly added to the actor. As a result, when we are inside the actor cpp, we can directly invoke the actor component's APIs. But we have to have a reference or a pointer to the USceneComponent or its derived component before we can invoke the APIs on them.
A way to tell if a component is an actor component or an scene component is to see it in the blueprint window, where a components section shows the hierarchy of this actor's components. Scene components are added in the upper part of the section, and actor components are added in the lower part of the section.