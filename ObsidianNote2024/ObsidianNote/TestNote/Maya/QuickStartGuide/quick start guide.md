SHORT WRITING:
MB = mouse button; 
LMB = left mouse button; 
RMB = right mouse button; 
MMB = middle mouse button;

WAY TO READ:
Functionality ---- keyboard input

Viewport navigation
pan ---- alt MMB
zoom ---- alt RMB or scroll wheel
rotation ---- alt LMB
frame selected ---- F button

Marking menus (5 of them)
ctrl + RMB; 
shift + RMB; 
ctrl + shift + RMB; 
W button + LMB;
space button;

remove component/object ---- delete
remove component along with vertices ---- ctrl backspace
change value ---- MMB
subtly change value ---- ctrl MMB
redo the last action ---- G button
apply the last used axis ---- MMB
curve/edge snap ---- hold C button when move
angle snap ---- hold J button when rotate
vertex snap ---- hold V button when move
grid snap ---- hold X button when mvoe
alter pivot ---- D button (snap tools above also apply to pivot altering)
duplicate object ---- ctrl D button or move tool shift
duplicate object with transformation/rotation ---- shift D button
grouping objects ---- control G button (given objects selected already)
parenting objects ---- P button or MMB drag in outliner window
adjust object without affecting its children ---- Insert button
unparenting objects ---- shift P button or MMB drag out in outliner window
hide / unhide ---- H button
open  whole hierarchy ---- shit LMB in outliner window
reveal selected in hierarchy ---- right click in outliner, click reveal selected
delete history on object ---- alt shift D button (given objects selected already)
change pivot mode (to world/object) ---- ctrl shift RMB
select ---- LMB click or LMB drag in viewport
Select if not already selected and unselect if already selected ---- shift LMB click/drag in viewport
add select ---- control shift LMB click
remove select ---- control LMB click
quick select ---- hold tab and LMB
grow select ---- shift > button
shrink select ---- shift < button
range select ---- given one component selected, shift LMB click another component
Simplified extrude mode ---- move tool shift
slide mode ---- move tool ctrl shift
isolate mode toggle ---- ctrl 1 button
raw polygon mode(default mode) ---- 1 button
smooth polygon preview mode ---- 3 button
bevel ---- ctrl B button
shift W button ---- set a translation key on animation timeline ( given object selected already)
S button ---- set a key on evey channel about the selected object
Toggle soft select ---- B button
attribute editor ---- ctrl A button


a newly created polygon object has 3 nodes:
transform node: location info in viewport space
shape node: info for rendering in viewport
poly node: the actual info about this polygon(vertices, edges, etc)
any modification would add and connect new nodes to these three.
atrribute editor shows all the nodes connected to the polygon and their details.


Alter camera render distance : 
First, get to the desired camera attribute editor. We can go to certain view like perspective, press view (at top left corner of the viewport) and camera attribute editor. Or select the desired camera in outliner, and access its attribute editor through this shortcut (ctrl A button), or look for its attribtue editor in the right side of Maya window.
Second, the far clip plane paramerter is the one controlling how far the render distance should be. And the near clip plane parameter is the one controlling how close the render distance should be.


