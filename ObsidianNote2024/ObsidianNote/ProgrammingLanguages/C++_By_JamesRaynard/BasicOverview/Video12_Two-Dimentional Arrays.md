
Multi-Dimensional arrays are arrays that contain elements which are array as well.

`std::string names[2][4]; // this would create a two-dimentional array a.k.a a table`
The line above would create an array of two slots. Each slot can store an array of four `std::string`.
The table would look something like below:
```
 Fred   Wilma   Pebbles   Dino
Barney  Betty    Bamm     Hoppy
```
And if we want to initialize the `std::string`.
```
std::string names[2][4] = {
	{"Fred, "Wilma", ...},
	{"Barney", "Betty", ...}
};
```
Another way to initialize it
```
std::string names[2][4] = {
	"Fred", "Wilma", ..., "Barney", "Betty", ..., "Hoppy"
};
```
Both ways are the same. In memory, it is stored as a single block, so the second method looks more like what it is in the memory.

To flatten a two-dimensional array into an one-dimensional array, it is good for performance, but with a cost of notation ease.