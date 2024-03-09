
Polymorphism is when we use a base class object to represent a derived class object. With traditional pointers, this is how we do it.
```
int main() {
	vector<Shape*> shapes;
	shapes.push_back(new Circle);
	shapes.push_back(new Triangle);
	shapes.push_back(new Square);

	for (auto& it : shapes)
		it->draw();

	for (auto& it : shapes)
		delete it;
}
```

It is a similar process with `unique_ptr`, but we have constructor calls instead of `new`, and we don't need to `delete` memory as the destructors take care of it.
```
int main() {
	vector<unique_ptr<Shape>> shapes;

	shapes.push_back(make_unique<Circle>());
	shapes.push_back(make_unique<Triangle>());
	shapes.push_back(make_unique<Square>());

	for (auto& it : shapes)
		it->draw();
}
```

One of the design pattern is the factory pattern, in which a function would create different new objects based on the passed arguments. The advantages of factory pattern is we provide another layer between the actual constructor calls and the caller's intention. And we can apply whatever logics here to invoke the constructor we want based on the arguments. An example of that is `make_pair<>()`.

To implement factory pattern with traditional pointer.
```
Shape* create_shape(int sides) {
	if (side == 1)
		return new Circle;
	else if (side == 3)
		return new Triangle;
	else if (side == 4)
		return new Square;
	else
		return nullptr;
}

int main() {
	Shapes* pShape = create_shapes(3);
	if (pShape)
		pShape->draw();
	delete pShape;
}
```

To implement factory pattern with `unique_ptr`.
```
unique_ptr<Shape> create_shape(int sides) {
	if (side == 1)
		return make_unique<Circle>();
	else if (side == 3)
		return make_unique<Triangle>();
	else if (side == 4)
		return make_unique<Square>();
	else
		return nullptr;
}

int main() {
	auto pShape{create_shape(3)};
	if (pShape)
		pShape->draw();
}
```

