Execution Order of Unity Event Functions

Awake() -> OnEnable() -> Start() -> Update() -> OnDisable() -> OnDestroy()

Awake()

-called once when object is instantiated

OnEnable()

-called once when object is enabled

-object can be disabled and enabled like a switch after instantiation

Start()

-called once after object is enabled and before the frame updates

Update()

-called once per frame

-two supplements:

>FixedUpdate(): frame-independent and called before physics calculations perform

>LateUpdate(): called once per frame and after Update() is executed

OnDisable()

-called once when object is disabled

OnDestroy()

-called once when object is destroyed

Debug.Log()

-c# version of console.log()

Coroutines

-are functions with a return type of IEnumerator

-yield keyword must be included to do nothing or suspend or resume the execution

-yield keyword takes in certain class instance to work

-calling corountines are different from calling regular functions, we need to use StartCoroutine(), and StartCoroutine() can either takes in a reference parameter to the coroutine or the string value of the coroutine name.

-are asynchronous functions running in parallel to Update function or other coroutines.

-Two ways to stop coroutine before it's finished: StopAllCoroutines() and StopCoroutine(). StopCoroutine() takes in a reference parameter to the coroutine or the string value of the coroutine name just like the StartCoroutine(), and more importantly, StopCoroutine() only works when it matches the StartCoroutine().

-How a coroutine is stopped depends on how it was started.

example:

private Coroutine countToNumberRoutine;

private void Update()

{

if(Input.GetKeyDown(KeyCode.Space)

{

//StartCoroutine("CountToNumber", 25000);

countToNumberRoutine = StartCoroutine(CountToNumber(25000));

StartCoroutine(SetShapesBlue());

//Time.timeScale = 0;

}

if(Input.GetKeyDown(KeyCode.S))

{

//StopAllCoroutines();

//StopCoroutine("CountToNumber");

StopCoroutine(countToNumberRoutine);

}

}

private IEnumerator SetShapesBlue()

{

foreach (Shape shape in gameShapes)

{

shape.SetColor(Color.blue);

yield return null;

}

yield return new WaitForSeconds(2);

Debug.Log("Coroutine completed.")

}

private Ienumerator CountToNumber(int NumberToCountTo)

{

for(int i= 0; i<=NumberToCountTo; i++)

{

Debug.Log(i);

yield return null;

}

}

>yield return null means wait for no time to resume the execution

>yield return new WaitForSeconds(2) means wait for 2 seconds of scaled time to resume the execution

>new WaitForSeconds(2) creates an instance of WaitForSeconds class with a parameter of 2 in float type.

>Time.timeScale can change the scale of simulated time to real time. Need to read the manual

>Some other classes for yield to take in: WaitUntil(), WaitWhile(), WaitForEndOfFrame(), WaitForFixedUpdate(), WaitForSecondsRealtime().

>StopCoroutine("CountToNumber") works when StartCoroutine("CountToNumber", 25000) is used.

List and Array

-List<type> loadOperations;

-loadOperations = new List<type>;

-public string[] shapes = {"circle", "square", "triangle", octagon"};

-public string[] moreShapes = new string[4];

>Both of them in C# need to be specific in type.

>List in C# is flexible as it can change its count(means length) , but array cannot. The length of array in C# is fixed once it's initialized. You can change the element in the array to null, but its length stays the same.

>List has all kinds of built-in functions like add, remove, contain, etc.

>array has no built-in function provided?

Delegate, Events, and Actions

>delegate is used to represent other methods, or specificly parameterize them.

>delegate is declared outside the class.

-using somenamespace

-public delegate void TextOutputHandler (string text)

-public class GameSceneController : MonoBehaviour {…}

>delegate's signature includes return type and take-in parameter

>here the return type is void, take-in parameter is string.

>this delegate now can represent any method with the same signature regardless where the method locates in the code.

>delegate type is TextOutputHandler

-public void OutputText (string output)

{Debug.LogFormat("{0} output by GameSceneController", output);}

>this method has void return type and string take-in parameter, so it can be represented by delegate TextOutputHandler.

-private void MoveEnemy (TextOutputHandler outputHandler)

{outputHandler("enemy at bottom");}

>normal parameter has one info, its type. However, method has two info, return type and parameter type. Using delegate, we can put method into other method as a parameter.

>delegate type TextOutputHandler means void return type and string parameter

-void Update()

{MoveEnemy(gameSceneController.OutputText);}

>when calling the MoveEnemy method, we put the OutputText method into the parameter slot like we do with simple values or object.

>the OutputText method can locate in this class or a different class, it doesn't matter.

>Event is declared and invoked in a class, and other classes subscribe and respond to it.

>it's an alternative to the tight coupling approach where a class would have a reference with another class and check conditions in one class to invoke certain methods in another class.

>all events have an underlying delegate type.

-public delegate void EnemyEscapedHandler (EnemyController enemy)

-public class EnemyController : Shape, Ikillable

{

public event EnemyEscapedHandler EnemyEscaped;

private void MoveEnemy()

{

if (EnemyEscaped != null)

{EnemyEscaped(this);}

}

}

>Here, the event EnemyEscaped has a delegate type EnemyEscapedHandler

>if the event is null, it should not be invoked, otherwise it would throw an error.

>When invoked, the event would pass the EnemyController instance as parameter. Subscriber method would be called when the event is invoked, it would receive this EnemyController instance as parameter, but may or may not use the parameter.

-public class GameSceneController : MonoBehaviour

{

private IEnumerator SpawnEnemies()

{

WaitForSeconds wait = new WaitForSeconds(2);

while(true)

{

EnemyController enemy = Instantiate(enemyPrefab, position, rotation);

enemy.EnemyEscaped += EnemyAtBottom;

yield return wait;

}

}

private void EnemyAtBottom (EnemyController enemy)

{

Destroy(enemy.gameObject);

Debug.Log("Enemy Escaped");

}

}

>treat event as a list or array property in a class, outside the class, we need the instance reference and use += to subscribe, each subscriber is one element in that event list/array. When invoked, all subscriber would be called.

>the EnemyAtBottom method is called when EnemyEscaped event is invoked.

>the EnemyAtBottom method has to take in the EnemyController instance parameter, but need not to do anythng about it.

>Action is just like event, but without the need to explicitly declearing a delegate

-public class EnemyController : Shape, Ikillable

{

public event Action<int> EnemyKilled;

private void OnCollisionEnter(Collision collision)

{

if (EnemyKilled != null)

{

EnemyKilled(10);

Destroy(collision.gameObject);

Destroy(gameObject);

}

}

-public class GameSceneController : MonoBehaviour

{

private IEnumerator SpawnEnemies()

{

WaitForSeconds wait = new WaitForSeconds(2);

while(true)

{

EnemyController enemy = Instantiate(enemyPrefab, position, rotation);

enemy.EnemyEscaped += EnemyAtBottom;

enemy.EnemyKilled += EnemyKilled;

yield return wait;

}

}

private void EnemyKilled (int pointValues)

{

totalPoints += pointValues;

}

>except declearing, other aspects are just the same as event.

>Action<T> can take in a generic type for parameter

>maybe it's return type is void by default?