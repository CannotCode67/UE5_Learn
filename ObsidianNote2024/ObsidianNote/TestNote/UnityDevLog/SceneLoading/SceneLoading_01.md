variable _currentLevelName 

>should it always match the active scene? 

SceneManager.ActiveScene 

>The currently active Scene is the Scene which will be used as the target for new GameObjects instantiated by scripts.  

LoadLevel 

>AsyncOperation ao = SceneManager.LoadSceneAsync(levelName, LoadSceneMode.Additive/Single); 

check if ao == null, if yes, debug a error and return. 

ao.completed += OnLoadOperationComplete;  //using Async would need to use callback, right? 

_loadOperations.Add(ao); //needed? 

_currentLevelName = levelName;  //Set the _currentLevelName to the LoadingScene name. 

OnLoadOperationComplete(AsyncOperation ao) 

>check if _loadOperations.Contains(ao) // more like a double checking. 

if yes, remove ao from _loadOperations 

check if _loadOperation.Count == 0; 

if yes, UpdateGameState(GameState.RUNNING); // always to RUNNING? 

Instance.InitSession(); // it is related to RUNNING GameState. 

SceneManager.SetActiveScene(SceneManager.GetSceneByName(_currentLevelName)). // Setting the active scene to the loaded scene. 

UnloadLevel 

>AsyncOperation ao = SceneManager.UnloadSceneAsync(levelName); 

ao.completed += OnUnloadOperationComplete;  //using Async would need to use callback,right? 

The idea of using Async is to prepare the scene seamlessly. 

It should always load the next scene, when the next scene is loaded, unload the current scene. 

LoadSceneMode.Additive means we need to handle the unload manualy, and it allows multiple scenes existing at the same time. 

LoadSceneMode.Single means we could have only one scene existing at the same time. 

Not using ButtonEvent to call the RestartFromEndGame() would make the execution flow normal and updateState() work properly. 

Problem with calling unloadlevel when FadingIn animation completed is the _currentLevelName is changed to something else 

when LoadLevel is called right after FadingIn. This _currentLevelName approach might need a further rework. Now, unload the 

GameOver scene explicitly is a temporary solution to maintain a functional game scene.