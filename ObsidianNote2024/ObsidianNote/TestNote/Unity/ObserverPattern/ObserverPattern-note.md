Handling events (events as in general sense, something happen) 

we could: 

-direct object calls 

>directly calling methods 

>needed methods are in one class 

>direct object calls are functionally insufficient to practical tasks. 

-indirect object calls 

>indrectly calling methods 

>methods are in different classes 

>need the reference to the instance to make the method call 

>end up with a tight coupling approach where instances have to maintain references to each other causing inefficient code to run and maintain. 

-using observer pattern
![[Unity/ObserverPattern/GetImage.png]]

>remember C# is a strongly typed programmng language, so a list can only store objects that belong to the same type 

>that's why inplementing oberver pattern in c# needs one more thing, the interface.
![[Unity/ObserverPattern/GetImage (1).png]]

>create a separate c# file named the name of the interface 

-public interface IEndGameObserver 

{ 

void Notify(); 

} 

>now the list in subject class are declared with type IEndGameObserver 

>then the concrete observer class needs to implement the interface using comma and interface name. 

-public class HUDController : MonoBehaviour, IEndGameObserver, SomeotherInterface 

>class in c# can only be derived from a single base class, but can implement multiple interfaces. 

>by implementing interface, a class needs to implement all the methods contained by the interface. 

>in this case, it's the Notify() method. 

>now the concrete obeserver class need to have a reference to the subject instance to call the AddObserver()/RemoveObserver() methods. 

>it seems not be able to get rid of tight coupling. 

-using delegates and events 

>here events are mainly used but events need delegates to work for delegate types 

>events are decleared and invoked in a class, and methods in other classes would subscribe to those events. 

>however, we still need the reference to the event invoking instance to subscribe/unsubscribe the event. 

>it all sounds very similar to the observer pattern, in fact, the event tool in c# is just an implementation of the general observer pattern. 

>how can we get rid of the tight coupling? we know observer pattern cannot accomplish that. 

-using publisher subscriber pattern (a closely related pattern to the observer pattern)
![[Unity/ObserverPattern/GetImage (2).png]]

>create a separate c# file named EventBroker
![[GetImage(5).png]]

>the static key word makes the event and the class member belong to the scope of class instead of instance. 

>now, publisher can directly call the static method without the reference to its instance.
![[Unity/ObserverPattern/GetImage (3).png]]

>we use the class name to specify the scope of the method we call. 

>subscriber can directly subscribe the event without the reference to its instance
![[Unity/ObserverPattern/GetImage (4).png]]
