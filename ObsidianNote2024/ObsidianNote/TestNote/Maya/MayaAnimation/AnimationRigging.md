Unity's animation rigging package is Unity's solution for runtime rigging, or a more general name: procedural animation. 

First, we need to install the package in Unity's package manager. 

For game objects that need to utilize the package, they first need to add the rig builder component. This rig builder component needs to affect the same hierarchy as the animator, so the best practice would be adding the rig builder component to the same object which the animator is attached to. 

There is also another component offered by this package, the bond renderer. It is a helper component that visualize the hierarchy in scene view, so that we know which bone we are affecting and how the bone is affected. This is also added to the root node, and there can be as many bone renderer as you need. 

Then, we create an empty child object under the root node (root node being the ultimate parent object for this whole game object, and it is usually where the animator component, physics components, scripts, etc. are attached. Give this empty child object another component come with the package, called rig. This rig component is the main entry point for all rig constraints. There should only by one Rig component per control hierarchy, added to the root node. (what exactly is control hierarchy?) Lastly, this rig-component-attached object needs to be referenced by the rig builder component. 

Under this rig-component-attached object, we create one child object for each constraint we want to implement. Each this kind of child object, is added with the component named by the constraint. For example, if we want to implement the two-bond-IK constraint, we create an empty child object under the rig-component-attached object, then add the two-bond-IK constraint component to the newly created child object. 

Each type of constraint has its own use case and setup. 

Rig transform component: When a specific GameObject part of your rig hierarchy is important for manipulation but not referenced by any rig constraints, you'll want to add the RigTransform component. This means if the position of an object is referenced by constraints, but we alter this object's position by changing its parent object's position or rotation, then we need to add the rig transform component to this parent object. Otherwise, the change in runtime won't be captured by the rig constraints. 

The way we can blend this runtime rigging smoothly into our character's regular animation, is to keyframe the weight of the constraint, so that the effect comes in gradually as defined by the animation curve. Position or rotation of the source objects are normally changed based on the player's input or script. Setting it up this way would allow the procedural animation to cope with multiple situations since the position and rotation can now by different values and the animation would still work.