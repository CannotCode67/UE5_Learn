Singleton script 

-this is a template or base class for some other system managers to inherit. 

-managers that inherit from this class would have only one instance at the same time, and this programming pattern is called singleton. 

-public class Singleton<T> : MonoBehaviour where T : Singleton<T> 

>T means Generic type, and it's treated as a placeholder until the instantiation of its instance. It's also used to avoid overloading. 

>where T : Singleton<T> means this generic type T has to be the extend class of Singleton<T>. "where T" is describing the T in class Singleton<T>. ": Singleton<T>" means base class is Singleton<T>, and only the extend class of Singleton<T> would have this base class. 

-private static T instance; 

-protected virtual void Awake() 

{ 

if (instance != null) 

{ 

Debug.LogError("bla bla bla"); 

} 

else 

{ 

instance = (T) this; 

} 

} 

>static keyword means this variable is shared across all the instance of this class. Changing it on one instance would change this variable for all other instances. We assign the refernce to the script itself to this variable, effectively making it the only one instance for the class. 

>instance = (T) this, remember need the (T) 

>protected keyword means its extend class can call this base class function. 

>virtual keyword means it can be overwritten by its extend class. 

-public static T Instance 

{ 

get {return instance;} 

} 

>this is called accessor and its convention is capitalize the first letter of the variable it returns. 

>usually it contains a getter and a setter, but in this case we don’t want other system to change this variable, so we only have a getter. 

GameManager script 

-this script is attached to a tranform game object in the first scene called Boot. 

-public class GameManager : Singleton<GameManager> 

>this class is an extended class of the Singleton<T> class. 

>we specify the type GameManager replacing the T placeholder. 

-private string  _currentLevelName = string.Empty; 

>unity can store scene date and access it by its name(string) or its index in the scene build list. 

>scene build list : file->build setting->add open scene->scenes in build 

>scenes in build shows scene's name and its index. 

>the application loads scenes in a top-to-bottom order. 

>only scenes in this build list can be manipulated by the unity scene management system. 

-AsyncOperation ao = SceneManager.LoadSceneAsync(levelName, LoadSceneMode.Additive); 

>more about SceneManager read the manual 

>SceneManager has both LoadScene() and LoadSceneAsync(), the second one can run in parallel while the first one cannot. Async means asynchronous function. 

>LoadSceneAsync returns an object of type AsyncOperation. 

>LoadSceneMode is a enum that has two values: Additive and Single. Single means load the scene and destroy the previous one when load finishes. Additive would just load and add the new scene without destroying the previous ones. 

-ao.completed += OnLoadOperationComplete; 

>ao.completed is an unity event. By +=, it adds the OnLoadOperationComplete function to its listener list, meanning ao's completion will trigger the OnLoadOperationComplete function. 

>ao.completed.AddListener(OnLoadOperationComplete) is the same.  

>ao.completed = OnLoadOperationComplete would replace everything in that listener list with just this OnLoadOperationComplete function. 

-void OnLoadOperationComplete(AsyncOperation ao) 

>function that listen to an event needs to has its event passing value as parameter. 

>no explicit accesser means private automatically. 

-DontDestroyOnLoad(gameObject); 

>unity provides this function to make sure something(this case the game object) can sticks around.  

-protected override OnDestroy() 

    { 

        base.OnDestroy(); 

        for (int i = 0; i < _instanceSystemPrefabs.Count; i++) 

        { 

            Destroy(_instanceSystemPrefabs[i]); 

        } 

        _instanceSystemPrefabs.Clear(); 

    } 

>it needs the same access modifier to extend the method. 

>override keyword means an extended version of the original method. 

>base.OnDestroy() would call the OnDestroy function of the base class. 

>_instanceSystemPrefabs.Clear() would make sure the reference is garbage collected. 

-public enum GameState {PREGAME, RUNNING, PAUSED} 

>enum keyword means enumerations, which is a value type that represents a set of related named integral constants. 

>in other words, what is underneath is {0, 1, 2} in this case. GameState.PREGAME will return a value of 0. It's just with enum, we use names to access the value instead of index like what we do in an array or a list. 

-void UpdateState(GameState state) 

    { 

        GameState previousGameState = _currentGameState; 

        _currentGameState = state; 

        switch(_currentGameState) 

        { 

            case GameState.PREGAME: 

                Time.timeScale = 1.0f; 

                break; 

            case GameState.RUNNING: 

                Time.timeScale = 1.0f; 

                break; 

            case GameState.PAUSED: 

                Time.timeScale = 0.0f; 

                break; 

            default: 

                break; 

        } 

        OnGameStateChanged.Invoke(_currentGameState, previousGameState); 

    } 

>case switch statement is an alternative of if else statement. 

>default can provides a fall safe if _currentGameState doesn't equal to any case, it would still break out of the statement. 

>Invoke() is a built-in method of UnityEvent class to fire the event. Alternatively, we can call this method like we do with a regular function, but using Invoke() is prefered. 

>Here we pass two actual values of type GameState as the arguments. 

>Time.timeScale = 0.0f turns the ratio between simulated time and real time to 0.0f, which would stop the game. read the manual.  

-public Events.EventGameState OnGameStateChanged; 

>we need to create a instance of the event class to use it. 

>this is how we access event class of a container class(Events). 

-public void TogglePause() 

    { 

        UpdateState(_currentGameState == GameState.RUNNING ? GameState.PAUSED : GameState.RUNNING); 

    } 

>ternary operator syntax: condition ? Return value if true : return value if false 

-public void QuitGame() 

    { 

        Application.Quit(); 

    } 

>Application.Quit() is a built-in function to close the execution of the application. More about Application, read the manual. 

UI Manager Script 

- 

MainMenu Script 

-[SerializeField] private Animation _mainMenuAnimator; 

>[] is called decarator. This one, [SerializeField], enables us to see and manipulate the variable in the unity inspector while being private. There are all kinds of built-in decarators. read the manual. 

-public void OnFadeOutComplete(){} 

>This function is automatically called when the fade-out animation clip is finished. Same for fade in. 

-GameManager.Instance.OnGameStateChanged.AddListener(HandleGameStateChanged); 

>AddListener method would enable the OnGameStateChanged event to trigger the HandleGameStateChanged function when the event fires. 

-void HandleGameStateChanged(GameManager.GameState currentState, GameManager.GameState previousState) 

    { 

        if (previousState == GameManager.GameState.PREGAME && currentState == GameManager.GameState.RUNNING) 

        { 

            FadeOut(); 

        } 

    } 

>The HandleGameStateChanged function has to take in the two arguments of the event, no matter it would eventually use them or not. In this case, it does use them. 

PauseMenu Script 

-ResumeButton.onClick.AddListener(HandleResumeClicked); 

>Button class has this built-in event called onClick which fires when the button is clicked.  

Events Script 

-public class Events 

{ 

    [System.Serializable] public class EventGameState : UnityEvent<GameManager.GameState, GameManager.GameState> {} 

    [System.Serializable] public class EventFadeComplete : UnityEvent<bool> {} 

} 

>This Events class is a container class. 

>The [System.Serializable] decarator is required for the instance of this class to be shown in the unity inspector even though it's decleared public. 

> we create unity event by creating new class extended from UnityEvent class. 

>for EventGameState, we want to contain two arguments of type GameState, so when we fires the event, it can pass along values such as the current game state and the previous game state.


UI Manager Prefab 

-it is instantiated by the gameManager in Boot scene. 

-itself is a gameObject called UIManager 

-three child objects: DummyCamera, MainMenu, PauseMenu 

-MainMenu has an Animation Component to play a clip, when the clip is finished, it fires function/an event for actions to listen to.



