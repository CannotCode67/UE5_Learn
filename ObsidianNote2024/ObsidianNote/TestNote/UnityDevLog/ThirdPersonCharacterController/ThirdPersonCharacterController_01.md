Character Movement and Camera Movement 

CharacterMovement 

Collect input through Input.GetXXX(String), String name and related key is pre-map through setting. 

Call the Input methods in Update() because we don't want to miss any key input of the player. 

But apply the movement either in FixedUpdate() or in Update() using Time.deltaTime. 

There are two ways to apply movement: CharacterMovement component or Rigidbody component. They use different built-in methods to apply movements. Check manual. The character movement component is also using physics like rigidbody component. 

Using rigidbody component means apply the movement through physics engine. Although the physics engine achieves this through setting the transform information, it also takes other factors into account like if there is something between  the current position and the target position, the game object might not get there depending on physics calculation. On contrast, setting the transform directly would instantly put the game object to that position, ignoring whether there is a obstacle inbetweeen or the position is inside another object, causing game objects clipping into each other.  

And there are two main movement infos: translation and rotation. 

Flow of translation implementation: 

Collect input through Input methods, put them into Vector3, normalize it to use as direction, a float variable to control the speed, construct a vector3 value with direction * speed * Time.deltaTime, apply this Vector3 value to the component using build-in method preferbly. 

Flow of rotation implementation: 

Rememebr rotation in both component is using Quaternion. The important part is understand how to convert quaternion and eulerAngle back and forth. We either using Input.GetXXX to collect mouseX info to directly set the rotation, or using Input.GetXXX to collect horizontal key input to use it with rotation speed variable to add deltaRotation on top of currentRotation.  

Quaternion.Euler(x, y, z) takes in a Vector3 value and change it to quaternion format. Using this to directly set the rotation like this, rigidbody.rotation = Quaternion.Euler(x, y, z); 

Quaternion.eulerAngles takes in a quaternion value to change it to a Vector3 format. Using this to get the current rotation like this, Vector3 currentAngle = rbody.rotation.eulerAngles.