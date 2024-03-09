
Nested maps are just maps that contain other maps.
```
int main() {
	map<int, string> level_one_map = {{1, "player"}, {10, "door"}};
	map<int, string> level_two_map = {{5, "player"}, {10, "monster"}};

	map<int, map<int, string>> game_map = {{1, level_one_map}, {2, level_two_map}};
}
```
In the order C++, we have to write `map<int, map<int, string> > game_map`, with a space between the two `>` sign, otherwise the compiler thinks this is a left shift operator `<<`. Although the compilers nowadays have no such a problem, C++ developers already get used to defining the types else where.
```
using level_map = map<int, string>;
//typedef map<int, string> level_map;

int main() {
	level_map level_one_map = {{1, "player"}, {10, "door"}};
	level_map level_two_map = {{5, "player"}, {10, "monster"}};

	map<int, level_map> game_map = {{1, level_one_map}, {2, level_two_map}};
}
```