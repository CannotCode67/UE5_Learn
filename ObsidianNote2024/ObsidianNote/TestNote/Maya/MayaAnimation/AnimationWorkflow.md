Setting up the frame rate and time slider. 

Turn on auto-key. 

Go to Animation layer to create a new layer, and this new layer is for block poses. 

The base layer should have nothing. 

Find out the key poses of the target animation, from references or imaginations, it doesn't matter. 

Take all the controls and key them at the starting frame with default values. 

Construct each key pose at different frames. 

Once we have all the key poses and they are in appropriate positions in time slider, meaning the time the character needs to finish the animation is desirable (timing), then we have the blocking layer ready. 

We duplicate this blocking layer and rename it the spline layer. In this layer, we work on the transitions between each poses, using the graph editor. 

Well, it seems we cannot duplicate this blocking layer and change the duplicate's mode to override. As the default layer mode when any animation layer is created is additive, and we finish the animation blocking in additive mode. When we try to change the layer mode to override, it breaks the animation. The way I find for now, to do it is to create an empty layer, set its layer mode to override, then copy the animation in time slider of blocking layer, lastly paste the animation into the time slider of spline layer. This way, it won't break the animation. Maybe there is a better way, need more research. 

Sometimes, the graph editor would show the key frames of selected controls in multiple layers, not just the current active layer. The fix is to make sure, the animation is not in play-back mode, and then with the controls selected, click other layer and click the wanted layer again. This should make the graph editor show the keyframes only in current layer. 

Once we the animation passes this spline test, we have the spline layer finish. 

And we can start work on any additional variation like facial expression or limb gesture on their own layers. 

We can combine layers to get the animation of combining result.