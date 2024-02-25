Mouse Manager Script 

Achiving the goal where character would move in a click-to-go fashion. 

The first thing is to get the position of the mouse'clicking. 

-Physics.Raycast(Camera.main.ScreenPointToRay(Input.mousePosition), out hit, 50, clickableLayer.value) 

>RaycastHit hit; 

>Physics.Raycast function description in unity documentation:

![[Unity/CharacterController/GetImage.png]]

>Camera.main would access the main camera object 

>ScreenPointToRay is a built-in function of any camera object 

>ScreenPointToRay returns a ray starting on the near plane of the camera and going through positions(x and y) pixel coordinates on the screen. 

>Remember screen space is defined in pixels so there is no z coordinate.  

>The origin(0, 0) of screen space in unity is bottom-left corner of the screen, unlike the one in Javascript P5 (top-left corner). 

>After the raycast function, the variable hit now have the detailed info of the hit point (our mouse's clicking position info). 

The next step is to use unityEvent to pass the target location to the navmesh agent component, so the character will move to the destination. 

outside the mousemanager class 

-using UnityEngine.Events; 

-[System.Serializable] 

-public class EventVector3 : UnityEvent<Vector3> { } 

inside the mouse manager class 

-public EventVector3 OnClickEnvironment; 

-void Update() 

{ 

if (Input.GetMouseButtonDown(0)) 

{ 

OnClickEnvironment.Invoke(hit.point); 

} 

} 

>remember unity event needs to have a delegate type (return type & parameter type) 

>here we use the class eventVector3 to specify the delegate type, it inherit from the UnityEvent<Vector3> base class. 

>it's a new way to specify the delegate type for event. 

>hit.point is a vector3 value, so it is an acceptable parameter for UnityEvent<Vector3> class and its extended classes. 

>In this script, there's no AddListener(), because listener is added manually in the inspector where the OnClickEnvironment public field is shown.

![[Unity/CharacterController/GetImage (1).png]]


Handling the passageway 

When character is moving through a doorway, we want the character not just move to the doorway but also through the doorway to the other side. In order to do that, we need to pass the doorway position + the direction vector to the event listener. 

We look at the collider, we see the z axis is the direction we are looking for.
![[Unity/CharacterController/GetImage (4).png]]

![[Unity/CharacterController/GetImage (2).png]]
>doorway.forward is the z direction unit vector, and increase it's magnitude by 10.

NPC controller script 

Achiving the goal where NPC would patrol between waypoints and if player is within aggro range pursue the player. 

-InvokeRepeating("Tick", 0, 0.5f);
![[Unity/CharacterController/GetImage (3).png]]
>This "Tick" method communicate with the navmesh component to constantly(every 0.5 second) set the destination whether it's a waypoint or the player. 

>Remember not to call InvokeRepeating method in Update() or FixedUpdate(). 

-if (waypoints.Length > 0) 

{ 

InvokeRepeating("Patrol", 0, patrolTime); 

} 

>This "Patrol" method alter the target waypoint by changing the accessing index of the waypoints array. 

-void Patrol() 

{ 

index = index == waypoints.Lengh - 1 ? 0 : index + 1; 

} 

>index is equal to 0 or its original value + 1 based on if index is equal to the end of the array, if true, 0, otherwise, index + 1. 

>in line ternary statement technique 

-void Tick() 

{ 

agent.destination = waypoints[index].position; 

agent.speed = agentSpeed / 2; 

if (player != null && Vector3.Distance(transform.position, player.position) < aggroRange) 

{ 

agent.destination = player.position; 

agent.speed = agentSpeed; 

} 

>set the agent.destination to one of the waypoint.position and move in half the max speed 

>once the player is within the aggroRange, agent.destination is set to the player.position, and it moves at max speed.

navmesh obstacle component 

navmesh agent component - obstacle avoidance 

NPC controller 

-void OnDrawGizmos 

{ 

Gizmos.color = Color.red; 

Gizmos.DrawWireSphere(transform.position, aggroRange) 

} 

>this would draw a sphere with wireshade and colored red, its center is the transform.position, and its radius? is the aggroRange.
