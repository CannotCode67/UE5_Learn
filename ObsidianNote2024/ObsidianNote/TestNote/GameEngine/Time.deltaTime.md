The concept of frame rate is easy to understand. For computer games, it is either 30 FPS or 60 FPS. So, there are 30 frames(static pictures) drawn to the screen in one second. Time.deltaTime means the time between each frame, so for 30FPS, deltaTime is 1sec/30frames = 33.3 milliseconds. 

Why should we care? 

Let's call the movement of a character in game deltaX, more accurately movement between the last frame and the next frame. Delta means the difference or change in mathematics. deltaX = velocity * deltaTime. This is why we need to use deltaTime, so the game is frame rate independent. 

Without it, a game would run faster on a more powerful hardware, and vice versa. 

To measure deltaTime, we read the value of the CPU's high-resolution timer twice, once at the beginning of the frame and once at the end of it, subtract it to get deltaTime. And this deltaTime is made available to all game systems. However, we are actually using the deltaTime of the last frame to do calculation for the next frame. It is not perfect, since something might happen to cause the next frame to take much more time. When this happens, we call it a frame-rate spike. 

Frame rate governing means trying to keep the frame rate smooth and consistent as consistent frame rate looks better. A good example is movie is at 24 FPS, but it is consistent and looks very good.