DCboolManager
-select one object, then select second object, open DCboolManager, click new boolean operation, the first object is the base, second object is the changing factor
-we can use more than one changing factor to perform different boolean operations on the base at one procedure.

SimplePipe
-SimplePipe can turn a curve object into a pipe polygon based on the settings.
-we usually extract this curve from a polygon object's edge (using convert edge to curve, aka the third script on the shelf)

VertSnap
-it snap vertices of one object to nearest vertices of target object
-first, we open the script, load the target object, then we select another object, switch to vertices mode, select vertices that we want to move, click run. It would run based on the distance factor. If a vertex is found to snap to within the range, it would run.

CleanUpDuplicatedNames(the 10th script)
-automatically rename objects to avoid having the same names

ZbrushExportUVsetup
-with object's hard edge defined, select all the faces of the object, run the script
-it would create the proper UV layout for zbrush to work on
-each UV shell is its own polygroup in zbrush, and we need to use polygroup functions

eFlow
-it averages the flow of the selected edges
-it is part of the ZhCG_polyTool which also contains other tools like shape line.
-shape line first adds a curve as a target, then select the edges and click run, it would try to mimic the curve shape with the selected edges

curveInfo
-it shows the length of a selected curve

Planar
-it flatten the selected face based on settings

Camera
-UV planar projection based on camera view

HardEdgeFromUVborder
-automatically harden edges based on the UV borders

Rename
-Rename selected objects based on settings like prefix, suffix, etc.

SmartMeshTool
-perform mesh operations without leaving unnecessary nodes

VertexAlignTool
-align vertices based on settings