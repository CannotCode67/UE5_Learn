Animation systems are the systems responsible for most of the moving element in game. By moving elements, it means anything moves in a game. Some movements can be achieved through vertex displacement technique in rendering pipeline, but most movements are from animation systems. 

In the early history of game animation, there was cel animation, much like sprite animation nowadays. A series of still images occupying the same grid in sequences to produce the illusion of motion without updating static elements in other grids. It is used mainly for 2D game, but in 3D game, it can still be used for billboards (quads that are always facing the camera).
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage.png]]

Next, we have the rigid hierarchical animation which is the earliest approach to 3D character animation. The mesh was modeled as a collection of rigid pieces, and those pieces were constrained to one another in a hierarchical fashion, just like skeleton. However, the constraints are directly applying between parts of the mesh. It performs FK behavior, but where two rigid pieces connect always has intersecting happened, and no IK behavior.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (1).png]]

The extreme of the other way is per-vertex animation. The vertices of mesh are animated, and the motion data is exported to be used in game. It takes a tremendous amount of time to create, and the motion data is huge. The result is great though, so nowadays it is used for facial animation mostly, as the human face is often too complex to simulate using joints. Artiest uses sculpting tool to create a couple of versions of facial expressions, and we can tell the mesh to blend between them. This has a more specific name, morph target animation. Maya calls it blendShape.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (2).png]]

Then, we have what we mostly use today, the skinned animation. This and the morph target animation are used altogether for most game animations today. In skinned animation, we create  a collection of joints (skeleton) and we tell the vertices of the mesh to follow the skeleton in appropriate way.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (3).png]]

One way we can treat all those different animation methods is to see them as data compression techniques. We know per-vertex animation theoretically offers the best result given enough time and resources. But in reality, there are all kinds of limitations. And we need to control the vertices in a faster and more economical way, but we still want result that is good enough to be used in game. In skinned animation, one joint can drive many vertices, and it is like a user interface for animator, so they don't need to move each vertex explicitly. Instead, animator controls a much smaller amount of joints to do the animation.

Skinned animation is the main one we study. We mentioned the skeleton in which multiply joints are put together in a hierarchical way. Each joint will maintain a relatively still relationship between itself and its parent joint, so any translation and rotation of the parent joint would affect all its child joints, but not the other way around.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (4).png]]

A skeleton is usually represented with an array. In an array, each element can be referred by index, and each joint is also an array containing three pieces of information as follow. So a skeleton is an array containing smaller arrays, in the sense of data structure.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (5).png]]

Notice one of the information hold by the joint is the inverse bind pose transform. The bind pose transform is the joint's transform in its local space when we perform bind skin, and it is represented with a 4-by-3 matrix (can be 4-by-4). The inverse of that is stored. Buy why? 

Next, we have the concept of pose. A pose is what it means in English. In the context of game animation, it is referring to the joint's translation, rotation, and scale, relative to themselves at earlier frame. In game, the typical frame rate is 30 frames per second or 60 frames per second. A pose can be the differences of joint's translation, rotation, and scale between any two frames. 

To put it in data structure, a joint's transform (translation, rotation and scale) is represented in a 4-by-3 matrix, the difference (pose) is also a 4-by-3 matrix describing the needed translation, rotation, and scale to convert the joint's transform at one frame to that at another frame, an affine transformation matrix. There is also another data structure for pose, and it is the SQT data structure (scale, quaternion rotation and vector translation). When we say pose of a skeleton, it is referring to the set of all its joints' poses. 

A joint's pose is much like the joint, using the parent joint as the coordinate space origin, so the pose's value is specified relative to the parent joint. Sometimes, we call this kind of pose the local pose, and it is almost always stored in SQT format. Why? 

Some game engines don't support joints to be scaled. Some support joint scaling uniformly. Some even support joint scaling nonuniformly. Omitting the scale ability can greatly simplifies the mathematics of frustum and collision tests in engines, and it saves a lot of memory as animation data can be huge for memory to load. 

In the process of animation pipeline, the local pose would get converted into global pose, so it can be taken into the calculation for information needed in the render pipeline. The converting process is similar to converting any matrix into different space. 

A bigger unit of animation data above pose is the clip. In movie, everything is planned out, the animation clip can be very long without worrying any problem. But in game, it is interactive, the animation clip needs to be short for the player might act differently from the expectation. And game character often plays different animation clips based on the player's input. 

Each animation clip has a local timeline denoted by t. At the start of the clip, t = 0, and at the end t = T, where T is the duration of the clip. Each unique value of t is called a time index.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (6).png]]

Because timeline is continuous, thus t is a real (floating-point) number, not an integer. But that doesn't mean the animator needs to set infinite amount of poses. Instead, animator only needs to set the key pose/ key frame, and any pose in between can be retrieved by interpolation between two key poses based on the time index.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (7).png]]

We use sample to denote a specific point of time, and we use frame to denote a period of time. Sometimes, people use frame for both, but we are trying to avoid any misconception.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (8).png]]

For a looping animation clip, the last sample would be the first sample for next iteration. But if the clip is not meant to loop, then its last sample would be an actual sample.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (9).png]]

Sometimes, we might use normalized time for the animation clip, where a normalized time unit u = 0 for the beginning of the clip, and u = 1 for the end of the clip. The use case of this, is to synchronize two or more animation clips with different durations. For example, we want to go from the a 2-sec run cycle clip to the 3-sec walk cycle clip. With the same normalize time unit for both clips, it is easier to line up the feet for a smooth transition. 

There is a concept called global timeline. It is the timeline begins when the game object is instantiated, and the timeline ends when the game object is destroyed from the scene. We can treat each animation clip the game object will play as one local timeline, and map them one by one to the global timeline until the game object is destroyed. 

The concept of time-scaling for animation clip is most naturally expressed as a playback rate, denoted by R. If R = 2, that means the clip is played back twice as fast as it was recorded. Thus, when it gets mapped to the global timeline, its duration is 1/R = 0.5 of its recorded duration. 

The animation systems can use either local clock or global clock. If it uses a local clock approach, each the animation clip would have its own local clock. In global clock approach, the animation clip's local timeline would get converted into global version, and each game object has its own global clock or share the same global clock. Obviously, the global clock approach is more complex, but it does allow the animation clips to communicate with each other. 

For example, a character plays its attack animation clip, and the enemy should play the reaction clip. However, making these two animation clips to look correct requires the clips to align well in time. The character's attack animation clip is called due to the player's input, but the enemy's reaction clip is called by code, and it can be called on the same frame or next frame based on the order of the code. The result might be a delayed reaction. In a global clock approach, however, animation clips have a unifying unit to talk to each other, the attack animation's start time can be passed to the reaction clip. And even the reaction clip is called next frame, it can start playing in the middle to align, skipping the beginning.  

Now, we look at the animation clip from a data structure perspective. When we export the animation clip, we export the skeleton along with animation data baked in. Those animation data is in SQT format like this. However, the animation data in this format is huge, so it would get compressed in various ways to save memory in reality.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (10).png]]

As long as the skeleton underneath is the same, different meshes can play the same animation clip. And this is called animation retargeting. Some engines can even ignore animation channels for joints that cannot be found, allowing the same animation clip to be played by mesh that are different from the skeleton used for recording the animation. However, for a good result, the mesh's skeleton cannot be too different after all. 

Animation curves are smooth, but underneath, game engines interpolate linearly between samples. This is called piecewise linear approximation.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (11).png]]

Animation systems can permit additional metachannels in the animation data, these channels are not relevant to the poses, but to contain event triggers for other systems in game engine to react as they see fit. Another use case is to permit special joints, known in Maya as locators to carry the transform information for other objects to use.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (12).png]]

There are other use cases.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (13).png]]

The skeleton drives the mesh, but more accurately it drives the vertices of the mesh. One vertex can be driven by multiple joints, and use a weighted average to combine the final result. The vertex carries the index of indices of the driving joints, and the weighting factor describing how much influence each driving joint has. Usually, there is an upper limit on the number of driving joints for one vertex, and it is often 4 due to the size is convenient to store, and not obvious improvement for increasing the driving joints.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (14).png]]

The matrix to transform the vertex from bind pose to current pose is called skinning matrix. 

In render engine, we know the vertex must be transformed into world space for calculation, therefore some engines multiply the model-to-world transform matrix by the skinning matrix in animation systems, so the render pipeline can do one less job for skinned geometry. 

Often, we need to blend two or more animation clips to produce the final animation clip. The term animation blending refers to any technique that allows such a thing to happen. You may ask why don't we just tell the animator to key the final animation clip directly. Well, animation blending is more complicated than that. For example, combining an arm-waving animation with body facing forward, with an animation of body facing left. We can adjust the blend ratio to get an arm-waving animation with body facing any angle from forward to left. This is much more versatile than telling the animator to create a arm-waving clip of body facing at certain angle. Moreover, we can even adjust the blend ratio during runtime to get the pose we want. Also, there are so much more use cases. 

We first study the lerp blending. LERP stands for linear interpolation, and it does that for pose's translation, rotation, and scale values. When we learn the pose's SQT format, the values are recorded at each integer frame, so 0, 1, 2, 3…etc. However, due to the unstable frame rate of game, we actually see poses at frame 0.9, 1.85…etc. We get the pose at 0.9 by interpolating the values at sample 0, and values at sample 1. Also, we mentioned the pose's SQT format, in reality, is not used exactly like what we learn due to its huge size, instead various compression techniques are applied such as storing only disparate key frames that spaced at uneven intervals across the local timeline. In this case, LERP blending is used to get the pose's values by interpolating two even more spaced out poses' values. 

Another use case for LERP blending is the cross fading. Before we jump right into it, we need to understand what is cross fading. Retaining smoothness in one clip is okay, but retaining the smoothness at where the character transitions from one clip to the next is so much harder. In the transition of two clips, if the skeleton's translation, rotation, and scale values have no jump, then, we have the C0 continuity.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (15).png]]

There are also higher level of continuities, C1 = velocity continuity, C2 = acceleration continuity. Having C0 continuity is good enough for game, and a decent approximation of C1 continuity is great. C2 continuity usually is not our goal. Now, when we use LERP blending for the transition, it is sometimes called cross fading, and if we use it correctly, we can have a great result. 

To cross fade two animations, we overlap the timelines of two clips by some reasonable amount, and blend the two clips together in a way where the weighting of the next clip starts from zero and reaches one at the end of the overlapping timeline. This is called smooth transition.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (16).png]]

There is also frozen transition in which the former clip just stops when the next clip starts to blend in.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (17).png]]

The blending factor can vary just like animating curve in maya. We can give it a ease-in/ease-out affect to make it even smoother if that is what we need.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (18).png]]

There is a way to achieve C0 continuity without blending. In practice, animators often decide upon a set of core poses. By making sure every clip starts and ends in one of those core poses, we can easily get C0 continuity without blending. For higher level continuities, we still need the cross fading. However, this is the prefer way in reality as it prevents any drawbacks from cross-fading two animation clips that are just too different to blend together. 

The last common use case of LERP blending, is character locomotion. When a character walks or runs, it changes its directions in two basic ways. Either it can face in the direction it moves, or it can face one direction while moving in a different direction. The first one is called pivotal movement, and the second one is called targeted movement or strafing.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (19).png]]

In strafing, the animator creates three clips in which the character faces at the same direction, but moves forward, left, and right. The idea is to put these three clips in a semicircle like this.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (20).png]]

And when the character moves in one direction in game, we get the angle and use that to adjust the blend factor. For example, if the moving angle is 45 degree, then the blend factor is 0.5 for both the forward moving clip and the strafing left clip. We cannot add the backward moving directly because it would have unnatural result, instead we need to hand key two more strafing left and right clips to work with the backward moving clip properly. And possibly add a transition clip for the character to adjust its leg when transitioning from forward to backward. 

For pivot movement, we can play the forward movement clip and rotate the entire character about its vertical axis. However, a human tend to lean into one side of its body when turns. So, we need to create three clips, forward moving clip, left and right turning clips, then blend them together to get a turning clip at desired angle. 

So far, we are studying the simplest LERP blending. There are complex LERP blends and Read game engine 11.6.3 section for more information about them. 

Moving next, we have the partial-skeleton blending technique. The LERP blending we study is applying its affect to the whole skeleton. However, it is quite common to blend two or more clips together in a way where both clips have weighting factor of 1, but applying to different area of the skeleton. A good example would be to blend the arm-waving clip and a natural arm-swing walking clip to have the arm-waving walking clip. The arm-waving clip and the walking clip have weighting factor of 1, but one is applying to the upper body, and the other is applying to the lower body. In partial-skeleton blending, each joint has another blend factor, and this blending factor is sometimes called a blend mask as it is used to mask out certain joints so they are not affected by one of the clips. Partial-skeleton blending can be applied along with LERP blending. The biggest issue for partial-skeleton blending would be its tendency to create a unnatural result. Even we can smooth out the transition area by mixing the blend factor for both clips, sometimes the unnaturalness not just comes from the blend mask transition area. For example, an arm-waving walk clip and an arm-waving run clip would have the same upper body movement, but when running, the upper body should be unable to maintain the perfect balance and have impact on the arm waving motion. 

Therefore, many game developers have turned to a more natural-looking technique known as additive blending. Additive blending introduces a new kind of animation called difference clip. Sometimes, it is also called additive animation clip. In essence, a difference clip contains the difference between two regular animation. Conceptually, clip A - clip B = difference clip. For example, a regular walking clip and a tired walking clip can produce a difference clip which contains values in SQT format to make the character look tired. And if we add this difference clip to a regular running clip, we would get a tired running clip. We can also apply LERP blending to the difference clip before adding to regular clip to adjust how tired the character looks like.

As we know, to produce the difference clip, the order matters. We call clip A as source clip, and clip B as reference clip, so difference clip = source clip - reference clip. This is okay in concept, but in data, we cannot simply subtract poses as matrix are not scalar quantities. The matrix equivalent of subtraction is multiplication by the inverse matrix. Therefore, what really happens underneath is, the poses in difference clip = poses in source clip * the inverse of poses in reference clip. Adding the difference clip to a target clip would be matrix concatenation, so poses in the new clip = poses in difference clip * poses in target clip. 

Given its usefulness, it has limitation which clip A and clip B have to have the same duration. Also a difference clip can apply to any target clip in theory, but there is no guarantee for it to generate a good result every time. Here are some rules of thumb for guidance:
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (21).png]]

Additive blending is used for stance variation. For each desired stance, the animator creates a one-frame difference animation, and additively blend them with the base animation.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (22).png]]

Additive blending can be used to add randomness or variation on top of an otherwise entirely repetitive locomotion cycle.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (23).png]]

Remember we discuss LRRP blending for its usefulness at locomotion. Well, additive blending can create similar affect, but just for the head or the weapon instead of the whole body. With a weapon aiming forward clip, the animator creates another four difference clips of weapon aiming extreme left, right, up, and down. Additive blending them together, and use LERP blending to adjust the difference clip so the weapon can aim at any angle within the extreme limitations.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (24).png]]

One trick to mention here is we can fix the local clock of a clip before blending. For example, we need three aim poses, and animator can create three one-frame clips for additive blending, or it can create a three-frame clip with one pose at each sample. And lock this 3-frame clip at certain sample in local timeline before additive blending. We can even get an in-between pose by setting the sample between integer samples, without using LERP blending. Of course, we can use this trick only when the engine has this capability. 

We know post-processing technique that works on the camera to add further adjustment to what we see in graphic. There is also animation post-processing that modifies the pose prior to rendering it. And just like graphic post-processing, there are different kinds of animation post-processing techniques. 

Procedural animation is any animation generated at runtime rather than being driven solely by data exported from maya. For example, a car's front wheel would turn based on the player's input. We can create a turning clip or we can create a quaternion based on the player's input, and apply that quaternion to the joint's Q channel by multiplication. This kind of adjustment is made to the local pose before global pose calculation. 

Inverse Kinematics is used to adjust the joint so that if a character's foot is on a surface that is not flat, the whole leg would bend a little to adjust the foot instead of stepping right through the ground. Another example would be a character picking up something. In the clip, it is fine, but in game, the object might not align well, so inverse kinematics can use that object to calculate a global pose for end effector (the joint), and tell the skeleton to adjust itself so that the joint would match that end effector.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (25).png]]

For more information about this IK technique, read Game Engine 11.7.2 section.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (26).png]]
The last pose-processing technique we discuss is the rag dolls physics. Again for more information read Game Engine 11.7.3, 12.4.8.7, 12.5.3.8 sections.

We mention compression techniques a little bit before. We would study it in more detail here. Animation data can be huge. 1000 seconds of animation data, uncompressed, can take up 114.4 megabyte in memory. Sounds okay, but 1000 seconds is on the low side for a modern game, and the memory of playStation3 is just 256 megabyte. Of course, nowadays PC and PlayStation have at least 8 gigabyte memory, but we never have all the memory at our disposal, and memory saved from compressing animation data allows a richer variation of animation clips. 

One technique is channel omission. If characters do not require nonuniform scaling or any scaling at all, we can take one third of the data out of the way by omitting the S in SQT format. And if stretching is not needed for limbs, we can take translations out of the way for those joints. And poses that stay the same over the whole clip can be stored as a single sample at the beginning of the clip. 

Quantization, for more information, read Game Engine 11.8.2 section. 

Some clips look roughly the same even they are in lower sample rate. For those clips, we can record them in 15 frames per second, or maybe even lower. If a channel's data varies in an approximately linear fashion during some interval of time within the clip, we can omit all the samples within this interval except the endpoints. Then, we can retrieve those poses through linear interpolation. 

Curve-based compression, for more information, read Game Engine 11.8.4 section. 

Instead of loading all the clips into the memory, we can load animation clip only when needed. Most games load a core set of animation clips into memory when the game boots, and keep them there for the duration of the game. For example, player's character's core move set, and animations that apply to objects over and over again. Other clips are loaded as needed. Clips for certain class of enemy would not be needed if the level has no such class of enemy.

Animation system architecture. Now, we understand the nitty gritty of animation, how that knowledge is integrated within the game engine defines the architecture of the animation system. Most animation systems are comprised of up to three distinct layers: animation pipeline (low-level), action state machine (middle-level), and animation controllers (high-level). 

The animation pipeline transform inputs (animation clips and blend specifications) into the desired outputs (local and global poses, plus a matrix palette for rendering). The stages in this pipeline are as followed: 

1.  Clip's data is decompressed, and a static pose is extracted for the requiring time index. The output for this phase is a local skeletal pose for each input clip. 
    
2.  Input local poses are blended together using various animation blending techniques to output one single local pose for all joints in skeleton, that is if blending is needed. 
    
3.  The skeletal hierarchy is walked to convert local pose in joint space into global pose in world space through matrix concatenation. 
    
4.  Post-processing techniques are applied to modify the pose. Some post-processing techniques require local pose, and this kind of pose processing actually happens before the 3rd stage. Some require global pose, and they happen in this 4th stage. 
    
5.  If post-processing techniques require global pose and output local pose, then we need to recalculate the global pose here in 5th stage. 
    
6.  Once the final global pose is generated, each joint's global pose matrix is multiplied by the corresponding inverse bind pose matrix. This would generate the palette of skinning matrices to go into the rendering pipeline.
 ![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (27).png]]

Some data are shared, and some are on per-instance basis. Skeleton, skinned meshes, and animation clips are shared data. For any one type of character, there will be one skeleton, one or more meshes and one or more animation clips. When there are multiple instances of one character type, each instance would hold their own per-instance data including clip state (local clock and playback rate), blend specification, partial-skeleton joint weights, local pose, global pose, and matrix palette. 

We discuss how the animation clips can be blended together with a weighting factor, but we never discuss how these factors are managed. One simple way is flat weighted average. In this approach, we maintain a flat list of all clips that have nonzero weight factor. To calculate the final pose, we extract a pose at certain time index for each clips in this list, and combine the poses with weighted average to generate the final pose. 

Another approach of managing blend specifications is blend trees. The animation blend tree is an example of what is known in complier theory as an expression tree or a syntax tree. Need more research. 

The low-level animation pipeline is very powerful, but rather too complex for direct use by game code. Therefore, we need an interface in between, and that is the action state machine (ASM). As its name suggests, there are action states. Each state corresponds to an arbitrarily complex blend of simultaneous animation clips. For example, an "idle" state might be comprised of a single full-body animation, or it can be additively blended with a couple of difference clips for randomness. 

Often in practice, animators, game designers and programmers cooperate to create the animation and control systems for the main characters in game. They come up with what the character can do and define states and blend trees based on that. Given that information animators then create clips to serve as the inputs. 

Some engines even allow custom blend tree node types so we can create a blend tree node to encapsulate a bunch of basic logic inside just like defining a function in programming. 

While there are multiple states, there must be transitions between states. With core poses, we can just jump from one state to another. Or we can use cross fading. However, cross fading doesn't work for all cases, such as transition from lying on the ground to standing upright. For cases like this, we need to introduce special states called transitional states which are comprised of hand-key transition clips. Technically speaking, they can be treated just like a normal state with all kinds of blending. There are transition parameters to define for any transition to happen.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (28).png]]

In theory, for n states, there are n^2 transitions in total. However, in a real game, we often define a couple of transition states to act as the central states. A good example would be the monster hunter's character actions, in which the character would only have a few options if it is not in one of the central states. 

When the character need to do more than one thing at once, it is difficult to achieve that with the character at a single state at a time. Either we can create a really complex ASM or we use a technique called state layer to solve this problem. Each independent layer can be in one state at a time, and we apply blending for layers to combine the layers to get what we want.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (29).png]]

Constraints. 

Constraints are needed when we want to line up two objects, such as putting the gun in the hand, lining up two people for hand shaking, or putting the hands on the right place when climbing a ladder, etc. We need all kinds of constraints to make the game look correct. 

The first kind of constraint is attachment. The concept of attachment is the parent-child relationship just like that in Maya. When the parent joint moves or rotates, the child joint adjusts itself to satisfy the relatively still relationship, but when child joint moves or rotates, the parent joint doesn't react to that. In game, often the parent joint is in one object, and the child joint is in another. If we expose one of the regular joint for this purpose, it is great, but sometimes, we just need another independent joint to provide the proper position and orientation, and we don’t want it to get treated like regular joints because regular joints have the whole expensive animation processing cost. The way we can do this is to mark those special joints as attach points. Some engines has the capability of create attach points in their animation systems, others would need the attach points modeled in Maya, then do the marking in the engine. 

In some clips, multiple characters are involved, and they are perfectly aligned in Maya. However,  when exporting to game engine, we are exporting one clip for each involved character, and tell them to play the clip at the same time in game. The timing is a minor issue, but if there is any interaction between characters, we would need to make sure the characters are aligned properly, because characters in game can be anywhere. 

One solution to this, is to introduce a common reference point for all the clips exported. In Maya, we use the locator to represent the this point, and tag it to be treated specially when exporting. In game, when clips are played, animation system can align up the characters by putting all the reference points in clips in the same location with proper orientation. However, when reference points are created in Maya, they are in model space. We would need them to be in world space for alignment, and we need to decide one of the reference points to be the anchor point. That's why it is a good practice to contain one fixed character like a door or something and use its reference point as the anchor point for it is fixed in the scene. The clip it plays can be opening or closing. 

Sometimes, a character holds a rifle with both hands, and it is hard to keep both hands at the correct places especially when LERP blending is involved as we cannot guarantee both hands are in correct position for all different blending factors. Inverse Kinematics is a good solution for this, if the engine provides such a capability. 

Foot sliding is one of the most common things we see affecting the fidelity of the game. And, there are a couple of ways to overcome this, one of them is motion extraction and foot IK. When we animate a looping forward walking clip, also called as locomotion cycle, the character steps forward, moving further and further away from the world origin in Maya. That's perfectly normal as we cannot adjust the world origin in Maya.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (30).png]]

However, game engine uses this origin to be indicate where the character is. (Not the root joint?) And when game engine moves this origin to move the character, there are two forward movements, origin moving forward, animation moving forward. This can easily cause foot sliding. The fix is the motion extraction, so the character would moves the same, but he's doing it constantly on the origin, as if he is moonwalking.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (31).png]]

By doing this, we are effectively getting rid of the animation moving forward. Then, as long as the movement speed of the origin makes sense with the animation motion, we should be able to avoid the foot sliding. However, things can get tricky if the character doesn't move forward in a constant pace, for example, he is limping (one leg is fast, one leg is slow). We can save the animation data in a special "extracted motion" channel, and game engine can use this to move the local-space origin by the exact amount that the root joint had moved in Maya each frame. This is why we call it motion extraction.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (32).png]]

Motion extraction takes care of the foot sliding when the character is moving in a straight line. But if he turns or moves not in a straight fashion, then we would still see foot sliding. Again, using Inverse Kinematics can help solve this kind of problem. So far, we mention a lot about IK technique for all kinds of problems, but it costs more resources and presents more technical challenges. As long as we can get the feel of the character right, we should not go for animation perfection. 

There are some more constraints briefly described below.
![[GameEngine/AnimationSystems/AnimationSystems-images/GetImage (33).png]]

Finally, we get to the high-level animation controller. 

Normally, each character has one animation controller, and for main character, there is even more than one. One animation controller should take care of one behavior. For example, the main character can drive a car, swim, and locomote. Each behavior is managed by one animation controller, since the character would move differently under different logic. The good way of using animation controller is to put all the logic relating to a particular behavioral category to the same place, making it very clean and easy to manage.