In order to have a sculptable 3D subtool, we need to draw any 3D mesh into the canvas, then press T or click Edit button on the top left corner to

LMB click and hold --- rotate camera

Shift + LMB --- switch front/back/left/right/top/button cameras

Alt + LMB --- pan camera

Mouse wheel --- zoom in or out (sometimes, this is not default, and the way we set it is to go to the document menu in top left corner, in that menu, there are zoom in and zoom out  buttons, by holding ctrl and alt, then click the button, we are in a hotkey setting mode for this button, then simply scroll the mouse wheel to assign the hotkey. Do it for both zoom in and zoom out buttons. This quick assign hotkey mode works for all the buttons)

In the canvas…

Masking

Pressing ctrl would go into mask + mode, then left click any part of the subtool to draw mask or left drag to marquee select area to assign mask, or left lick empty space in canvas to inverse mask.

Pressing ctrl and alt would go into mask - mode, all the controls are the same, but now instead of adding mask, it substracts mask.

Selection

Pressing ctrl and shift would go into selection mode, then left click any part of the subtool to send the whole group which the clicked area belongs, to the other selection set, or left drag any part of the subtool to marquee select the geo and send it to the other selection set.

Pressing ctrl and shift, then left drag in empty space in canvas would switch between two selection sets. Only the activated selection set would show its containing geo.

Pressing ctrl and shift, then left click in empty space in canvas would merge two selection sets and effectively bring everything back into one piece.

DynaMesh can only apply to mesh that undergoes some changes. So you cannot dynaMesh a subtool, then dynaMesh again right away. You need to apply some changes to the subtool between dynaMesh operations.

Decimation would triangulate the subtool, and the geo is not evenly spread over the subtool like dynaMesh. Therefore, if we still need to apply modifications to the mesh, we should use dynaMesh, only use decimation when this subtool is good to export.