Usually, low level systems are there throughout the whole gameplay. They should be created once the game program runs, and get deleted when the program closes. Therefore, they should be reachable all the time no matter what state of the game is. Singleton approach is a good fit for them because Singleton is global in a way that there is only one instance of this class can exist at a time. So we don’t need to find and maintain a reference to it in every single class. How specifically to do it in C#? We make the class has a static variable of its own type, and we assign this( meaning this very instance) to that variable. And we also need a static accessor to protect it from being revised from the outside, so only getter, no setter. Finally, some mechanism to make sure it is the only one instance of the class. 

How to start subsystems in a desired order? 

First, why do we need to? It is because each subsystem can be interdependent like registering listeners for events. If instance holding that event is null, then registering would fail. And registering for events is mostly a one-time thing done right after the instance is created, so we have to make sure the order to instantiate our subsystems would not cause any registering failure. 

Now, a simple approach would be explicitly instantiating the subsystems in a line by line fashion.
![[LowLevelSystems.png]]
Or put the prefabs in array and iterate through, store the instances in a list that hold by GameManager. It is more advanced, but very similar. 

- void InstantiateSystemPrefabs() 

{  GameObject prefabInstance; 

   for (int i = 0; i < SystemPrefabs.Length; ++i) 

   {  prefabInstance = Instantiate(SystemPrefabs[i]; 

      _instancedSystemPrefabs.Add(prefabInstance);}} 

Same doing when shutting down. 

protected override void OnDestroy() 

{  base.OnDestroy(); 

   for (int i = 0; i < _instancedSystemPrefabs.Count; ++i) 

   {  Destroy(_instancedSystemPrefabs[i]);} 

   _instancedSystemPrefabs.Clear();}