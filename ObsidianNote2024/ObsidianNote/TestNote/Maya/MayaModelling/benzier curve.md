Benzier curve has the benzier handle which is a more powerful tool than CV (control vertex) when shaping the curve. That's one of the reasons why using a benzier curve over a regular CV curve in some cases. However, benzier curve cannot be treated as a regular curve in Maya, so we need to convert it to regular curve when we are satisfied with the shape. However, the converted curve needs to be rebuilt to regain the smooth shape.

Create a benzier curve with two points.

Get to the benzier handle:
select CV, then shift RMB to select benzier;

Add additional benzier handles:
select control point and along any position of the curve, we can left click to add a control point, then shift + RMB to select insert knot to add a control vertex with benzier handle to that location.

Convert:
convert benzier curve to NURBS object;

Rebuild:
rebuild curve function;
