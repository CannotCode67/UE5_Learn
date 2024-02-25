-scripts 

TankMovement Script 

Methods: 

-GetComponent<Rigidbody>(); 

>used to return and store the object's component(here it's rigidbody component) 

>the variable to store the component has to be declear as the component type(here it's Rigidbody) 

>if there's multiple components of the same type, it would return the first one it finds. 

Accessing component's property(using dot) 

-it is like accessing object's attribute(m_Rigidbody.isKinematic = true;) 

-float movementInputValue = Input.GetAxis("AxisName"); 

>return the float value ranging from -1 to 1 of the identified axis. 

>it's frame rate independent. 

-Mathf.Abs(int value/float value); 

>return the absolute value of the parameter 

-Random.Range(min value, max value) 

>return a float value between min and max parameters. 

-AudioSource.play(); 

>when audioSource.clip is assigned a different one, it needs to run the Play() to get started. 

-Vector3 movement = transform.forward * movementInputValue * speed * Time.deltaTime; 

>Vector3.forward equals Vector3(0, 0,1), but transform.forward returns a normalized(means 1 if not zero) vector3 value that represents the blue axis(z axis) of the transform in world space, which means it still works while rotated. 

>Time.deltaTime : used to smooth the movement in case of frame rate irregularity, when it is in Update(), it is needed. But FixUpdate() has a fixed frame rate, so if it's in FixUpdate(), it is not necessary. 

>speed : a float/int variable to control how fast the movement is. 

>movementInputValue : mentioned above, float value range from -1 to 1. 

-m_Rigidbody.MovePosition(m_Rigidbody.position + movement); 

>MovePosition moves the rigidbody object to the parameter position 

-float turn = m_TurnInputValue * TurnSpeed *Time.deltaTime; 

-Quaternion turnRotation = Quaternion.Euler(0f, turn, 0f); 

-m_Rigidbody.MoveRotation(m_Rigidbody.rotation * turnRotation); 

>m_TurnInputValue is like movementInputValue, a float value ranging from -1 to 1 

>TurnSpeed : a float/int variable to control how fast the turn is 

>Quaternion.Euler(vector3 value) : converts the vector3 value to whatever it equals in Quaternion. Quaternion is a better data form for storing rotation value than vector3(which also called Euler). 

>MoveRotation(m_Rigidbody.rotation * turnRotation) : use * instead of + for rotation, because this is how Quaternion works? 

CameraControl Script 

Methods: 

-M_Camera = GetComponentInChildren<Camera>(); 

>used to return and store a child object's reference(here it is a camera type) 

>basically it's the child version of GetComponent<type>();  

-transform.position = Vector3.SmoothDamp(transform.position, m_DesiredPosition, ref m_MoveVelocity, m_DampTime); 

>transform.position means where it is now 

>m_DesiredPosition means where it wants to go 

>m_MoveVelocity means how fast it goes, ref needs to read the manual 

>m_DampTime means how long before it starts to go 

-if(!m_Targets[i].gameObject.activeSelf) 

>activeSelf returns the local active state of the object. 

>Note that a GameObject may be inactive because a parent is not active, even if this returns true.  

-m_Camera.orthographicSize = Mathf.SmoothDamp(m_Camera.orthographicSize, requiredSize, ref m_ZoomSpeed, m_DampTime); 

>basically, it's the mathf class version of smoothdamp, it works the same as the one in vector3 class. 

-Vector3 desiredLocalPos = transform.InverseTransformPoint(m_DesiredPosition); 

-Vector3 targetLocalPos = transform.InverseTransformPoint(m_Targets[i].position); 

>InverseTransformPoint converts the parameter position value from world space to local space, by local space, it means local to the cameraRig in this case, cause that is the object this script attached to, it's the cameraRig object's transfom that was accessed when using transform.InverseTransformPoint. Remember the cameraRig object has rotation value other than (0, 0, 0), its local space is different from the world space. Here the second line converts the tank's position in world space to a position value(a vector3 value) in space local to the cameraRig. 

-size = Mathf.Max(size, Mathf.Abs (desiredPosToTarget.y)); 

-size = Mathf.Max(size, Mathf.Abs (desiredPosToTarget.x) / m_Camera.aspect); 

-size += m_ScreenEdgeBuffer; 

-size = Mathf.Max(size, m_MinSize); 

>Mathf.Max returns the bigger parameter value of the two. 

>it's using desiredPosTarget.y because in local space, y axis is the axis pointing straight up like x-y-coordinate system. 

>remember distance/aspect = size in orthographic camera, so both x and y are needed to adjust the size of the camera. If the tank is way in the up/down direction, y is bigger, if the tank is way in the right/left direction, x/aspect is bigger. The bigger one will be added some buffer size to be the actual size of the camera so it won't miss any tank. 

>the size of camera cannot be smaller than the min size, otherwise it's not comfortable to look at 

-public void SetStartPositionAndSize() 

{ 

FindAveragePosition(); 

Transform.position = m_DesiredPosition; 

M_Camera.orthographicSize = FindRequiredSize(); 

} 

>setting the access modifier to be public means other objects can call this function as well, and it is intent to be used by the game manager object 

UIdirectionControl script 

Methods: 

-private Quaternion m_RelativeRotation = transform.parent.localRotation; 

>transform.parent.localRotation returns canvas(parent object) local rotation value. 

tankHealth script 

Methods: 

-private ParticleSystem m_ExplosionParticles = Instantiate(m_ExplosionPrefab).GetComponent<ParticleSystem>(); 

>get the particleSystem component from the instantiated m_Explosion object. 

>Instantiate only works with prefab? 

-m_ExplosionParticles.gameObject.SetActive(false); 

>disable the m_ExplosionParticles object 

Object pooling? Look it up online 

-m_Slider.value = m_CurrentHealth; 

>access slider object's value property 

-m_FillImge.color = Color.Lerp (m_ZeroHealthColor, m_FullHealthColor, m_CurrentHealth / m_StartingHealth); 

>lerp means linearly interpolates between colors a and b by t. 

>t ranges from 0 to 1, so using m_CurrentHealth / m_StartingHealth turns the number into a ratio ranges from 0 to 1 

>m_FillImage.color determines the slider UI foreground's color 

-m_ExplosionParticles.transform.position = transform.position; 

-m_ExplosionParticles.gameObject.SetActive (true); 

-m_ExplosionParticles.Play(); 

-m_ExplosionAudio.Play(); 

-gameObject.SetActive (false); 

>moves the particleSystem object to where the tank is 

>makes the particleSystem active and play the effect 

>play the audioClip to have the sound effect 

>set the tank inactive cause it is dead 

shellExplosion script 

Methods: 

-public LayerMask m_TankMask; 

>public variable to reference the player layer, using the LayerMask type. 

-Destroy(gameObject, m_MaxLifeTime); 

>this function destroys the gameObject when reaching the max life time. 

>this static function is called in Start() function, so when it is instantiated it would start counting the life time. 

-private void OnTriggerEnter(Collider other) 

{ 

Collider[ ] colliders = Physics.OverlapSphere ( transform.position,       

M_ExplosionRadius, 

M_tankMask);  

For(int I = 0; I < colliders.Length; i++) 

{ 

Rigidbody targetRigidbody = coliiders[i].GetComponent<Rigidbody>(); 

        if(!targetRigidbody){continue;} 

targetRigidbody.AddExplosionForce (m_ExplosionForce,       transform.position, m_ExplosionRadius); 

TankHealth targetHealth = targetRigidbody.GetComponent<TankHealth>(); 

If(!targetHealth){continue;} 

Floate damage = CalculateDamage(targetRigidbody.position); 

targetHealth.TakeDamage(damage); 

} 

M_ExplosionParticles.transform.parent = null; 

M_ExplosionParticles.Play(); 

M_ExplosionAudio.Play(); 

Destroy(m_ExplosionParticles.gameObject, m_ExplosionParticles.duration); 

Destroy(gameObject); 

} 

>this OnTriggerEnter(Collider other) is called when the collider of shell object(this script attached to) bumps into other colliders. 

>Collider[ ] colliders is an array to store all the detected colliders 

>Physics.OverlapSphere creates an imaginary sphere with an origin of transform.position, a radius of m_ExplosionRadius. Anything in this sphere's range will then be checked if it's in the m_tankMask. If it is, it will be stored in the colliders array. More about this method read the manual. 

>targetRigidbody.AddExplosionForce adds force to the targetRigidbody, and this force is origined at transform.position, with a radius of m_ExplosionRadius, a magnitude of m_ExplosionForce.  More about this method read the manual. 

>TankHealth is the variable type referencing the tankHealth script on tank object.   

>targetRigidbody.GetComponent<TankHealth>(); how is possible to get the tankHealth script from a rigidbody component? In tutorial, it explains GetComponent can be used to return other component on the same object with speicfic component, here it accesses the tankHealth component through the rigidbody component since they are on the same object. Needs more info. 

>targetHealth.TakeDamage(damage); calling the public function on the tankHealth script. 

>m_explosionParticles.transform.parent = null, basically detaches the particleSystem object from its parent object, so when the shell gets destroyed, the particle effect can still play on. 

>destroy not the particleSystem component but the particleSystem object 

>m_ExplosionParticles.duration is the particle effect duration time. 

>destroy the shell object. 

-private float CalculateDamage(Vector3 targetPosition) 

 { 

       Vector3 explosionToTarget = targetPosition - transform.position; 

       float explosionDistance = explosionToTarget.magnitude; 

       float relativeDistance = (m_ExplosionRadius - explosionDistance) /  m_ExplosionRadius; 

       float damage = relativeDistance * m_MaxDamage; 

       float damage = Mathf.Max (0f, damage); 

       return damage;                       

 } 

>its trying to turn the distance into a ratio to calculate how much damage to deliver 

>the last line is a failsafe if damage is a negative number. damage can be negative in cases where the tank's position center is outside the radius but its collider is within to be picked up by the physics.OverlapSphere method. In those cases, m_ExplosionRadius - explosionDistance will get a negative number. 

![[Unity/Tanks/Tanks-images/GetImage.png]]

TankShooting script 

Methods: 

-Rigidbody shellInstance = Instantiate (m_Shell, m_FireTransform.position, m_FireTransform.rotation) as Rigidbody; 

>Treat the object as a rigidbody component using as Rigidbody. Only works when the object has a rigidbody component. 

-shellInstance.velocity = m_CurrentLaunchForce * m_FireTransform.forward; 

>the velocity takes a vector3 value. M_CurrentLaunchForce is magnitude. M_FireTransform.forward is the direction. 

-public AudioSource m_ShootingAudio; 

-public AudioClip m_ChargingClip; 

-public AudioClip m_FireClip; 

-m_shootingAudio.clip = m_ChargingClip; 

-m_shootingAudio.Play(); 

-m_shootingAudio.clip = m_FireClip; 

-m_shootingAudio.Play(); 

>Assigning another audioclip will automatically stop the previous sound.


-TankMesh

When drag the mesh to scene:

It creates a parent object with only transform component

And children objects consist of different mesh parts,

If it is combined mesh, it it would only have one child object for the whole mesh.

Put the parent object to player layer but not the children objects

Any component should be added to the parent object.

Rigidbody component:

-Constraints > use to prevent certain movement from happening

-isKinematic > on means it takes no outside forces into its own physical behavior, but it still can apply forces to other objects with collider and rigidbody components.

Collider component:

-IsTrigger > yes means triggering events when collided/ no means simulating physical collision when collided.

AudioSource component:

-loop > plays the clip again and again

-if multiple audio clips needed, one audio cilp for one audioSource component

-play on awake > when the object awakes the clip starts to play automatically

Turning the object into a prefab:

How? ->Putting the object into the prefab folder, done

Object that is a prefab in the scene has a few more version control options

New changes applied to the prefab instance is shown in grey color, while the original property is shown in blue.

Particle system object

-added to the tank parent object as a child object alongside with mesh objects

-particle system object is expensive to instantiated, so instead of instantiating and destroying them, using enable and disable to turn it on and off is more efficient.


-camera

Clear flags means what fills the space outside the envirnment mesh

 projection type:

-Perspective > scale changes along the distance from the camera

-Orthographic > scale stays fixed over distance

Create an empty game object and name it CameraRig, then put the camera object under this cameraRig object as a child object

-the idea is any camera movement should affect cameraRig, and indrectly the camera follows the same movement as a child object.

-why not directly attach the movement script to camera object?

Child object's tranform value is related to its parent object, for example, if its position value is (0, 0, 0), it means the child object is exactly where the parent object is, but the parent object's position value might not be (0, 0, 0) for that is the position value in world space.
![[Unity/Tanks/Tanks-images/GetImage (1).png]]![[Unity/Tanks/Tanks-images/GetImage (2).png]]

-UserInterface

The container of all the UI is canvas

Create any UI element will automatically create a canvas

 There are kinds of canvas:

-screen space canvas : filling up the whole screen(health bar that stays fixed in certain part of the screen like upleft corner)

-world space canvas : filling up a certain part of the 3d world(health bar that follows player mesh)

-change it in canvas component's render mode option

Objects in canvas are rendered from the bottom of the hierarchy to the top of it.

Canvas scaler component : read the manual

EventSystem object: handles the UI interaction like clicking or dragging a UI element might trigger some reaction

All UI elements by default have RectTranform component which provides a few extra options to manipulate the UI element.

For worldSpace canvas, in order to follow the target mesh, just drag the canvas object under the parent object as a child object alongside with the mesh renderer child objects.

Adjust the rectTransform attribute to make it fit the need and desired size.

Pressing T or click the rect tool on the upper left corner will activate the UI manipulating tool.

SliderUI:

In hierarchy, it looks like this:

![[getImage.JPG]]

Here Handle are rendered on top, then HandleSlideArea, then Fill, then FillArea, then background.

If not making an interactable UI, just get rid of the HandleSliderArea and its child object(Handle).

If the canvas is already tuned to worldSpace and its size and position are adjusted, then we can use the anchor presets(the left rect shape in rectTransform component) to make the UI elements fit the canvas. Shift: also set the pivot, Alt: also set the position.

In the Slider component, uncheck the Interactable will turn off the mechanics for interaction behind the scene, just deleting the interactable area would not do it.

Transition property handles what would happen as dragging the slider, since it's not interactable, it would be none.

Background object changes the source image to custom sprite, change the alpha in color property to make it transparent.

Fill object changes the source image to custom sprite, change the alpha in color property to make it transparent, change the image type to filled, and set the fill method to radia360, fill origin to left, unchceck clockwise.

Add the UIdirectionControl script to the slider object.




-ShellMesh

Drag the model to the hierarchy will create the shell object

Unlike the tank mesh, it is one single object without any child

Object. Any component should be added directly to this object.

Collider component

-check isTrigger

Rigidbody component

-check useGravity

Add the ShellExplosion object to shell object as a child object

The shellExplosion is just a particleSystem

 add the audioSource component to shellExplosion

Add the light component to the shell object

Add the shellExplosion script to the shell object

Drag it into the prefab folder to turn the shell into a prefab.



-FiringShell and UI

Create a child object of the tank parent object, rename it "FireTransform".

Adjust the position and rotation of the object.

Using the same canvas as the tank health UI, create a slider object under the canvas.

Rename it "AimSlider". Delete the handle area and its child object because this UI is

Not interactable either. Delete the background as well for it's supposed to be nothing when the button is not pressed.

Use the rectTransform component and its anchor preset to adjust the positions of both the slider object and fillArea object. Use the UI manipulating tool to adjust the slider UI more precisely. The UI manipulating tool works on 2D(canvas plane), so if it's needed to be moved at the third axis, we need to use the transform tool. ImageType choose sliced, check fill center and raycast target for ??? Reason.