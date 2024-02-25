Game Loops 

-code wise game loop: what happens every frame in rendering and systems 

-design wise game loop: what the player experiences over and over again 

Game States 

-Finite State Machine programming pattern to implement game states 

-Initial game states for this case: Pregame, Playing, Paused 

-Game Manager contains and handles the game states 

-other systems should act differently according to different game states



Some systems should be gloablly accessible for the life of the game. 

That means those systems should stay between scenes, levels, or even all the time. 

Game manager should persist for the entire life of the game. 

Game manager should instantiate or destroy other systems depending on the game state. 

Game manager should control the game loop (game states& state changing conditions).