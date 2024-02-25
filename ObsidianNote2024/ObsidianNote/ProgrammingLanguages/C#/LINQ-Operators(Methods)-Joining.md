In part1, we study filtering, ordering, and projecting functionalities of LINQ, now we are about to learn joining, grouping, and aggregating functionalities.

Joining happens when we have two sets of data, and we put them into one. In previous example, our CSV file is a table regarding different cars and their fuel efficiency. Now, in order for us to study joining, we have another table of manufacturers and their countries. Like what we did with cars, we create a Manufacturer class to used in projection, and we bring the data into the program the same way we did for car. Notice the lambda expression can have curly brace to contain the implementation, but once you use curly brace, you would need a return keyword.
```C#
private static List<Manufacturer> ProcessManufacturers(string path)
{
	var query = File.ReadAllLines(path)
	                .Where(l => l.Length > 1)
	                .Select(l = >
	                {
		                var columns = l.Split(',');
		                return new Manufacturer
		                {
			                Name = columns[0],
			                Headquarters = columns[1],
			                Year = int.Parse(columns[2])
		                };
	                });
	return query.ToList();
}
```
Joining in both syntaxes has two different looks, and even behave a little bit differently, so we first start with query syntax. In joining, there are two sets of data, and the main one is called outer sequence, and the other one is called inner sequence. Which one is which? Outer sequence typically has more data than the inner one, and it is used in the `from` keyword. In other words, we put the inner sequence after the `join` keyword, because we are joining it to a main data set. There are also multiple entries in inner sequence, so we use the same structure, something in a collection of something. Then, we need to tell C# how to match entries on both sequences, using keyword `on` to connect the criteria. This criteria has to be an equality relationship, and we have to use keyword `equals` here, not `==` operator. After this line, we have joined two sequences together, and have accesses to both car and manufacturer. Another thing to watch out for is if C# cannot find an inner sequence entry that matches some of the entries in main sequence, then those main sequence entries would be thrown away from the query. Like, if the manufacturers doesn't have a entry of TOYOTA, then all the TOYOTA made cars in main sequence will be excluded after the joining.
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = from car in cars
	            join manufacturer in manufacturers
	                 on car.Manufacturer equals manufacturer.Name
	            orderby car.Combined descending, car.Name ascending
	            select new
	            {
		            manufacturer.Headquarters,
		            car.Name,
		            car.Combined
	            };
	foreach (var car in query.Take(10))
	{
		Console.WriteLine($"{car.Headquarters} {car.Name} : {car.Combined}");
	}
}
```
Now, we move on to the method syntax. At the first look, the `Join()` method can be overwhelming, because it takes four parameters: one `IEnumerable<T>`, and three delegates. I mean, none of them are simple types. Let's examine them one by one. The first one, we need to provide the inner sequence to join against, but apparently it needs to be something implementing `IEnumerable<T>` interface. Second, we need to provide the outerKeySelector, basically a `Func` delegate automatically takes in a Car object, and return something of type TKey. Third, we provide the innerKeySelector, another `Func` delegate automatically takes in an object of type in the inner sequence, returns something of type TKey. And because we provide outerKeySelector first, this TKey is fixed when we work on the innerKeySelector. It is string in our example. Both selectors work just like criteria in query syntax, setting up an equality relationship. Then, the part where we mention method syntax behaves differently is the forth parameter. A resultSelector is a `Func` delegate automatically takes a Car object, an object of type in inner sequence, and returns something of TResult. Basically, we need to perform a projection, but this projection needs to accept both Car object and Manufacturer object, and return something we define. After `Join()` method, we lose accesses to both c and m, and gain access to the thing we project into. Although we continue using c for reference, but now c is denoting the new anonymous object instead of Car object. We can prove that by looking inside the object of the query with a foreach loop. For each object inside query, we only have access to those three properties and some inherited from System.Object.
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = cars.Join(manufacturers,
						  c => c.Manufacturer,
						  m => m.Name,
						  (c, m) => new
						            {
							            m.Headquarters,
							            c.Name,
							            c.Combined
						            })
					.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name);
					
	foreach (var car in query.Take(10))
	{
		Console.WriteLine($"{car.Headquarters} {car.Name} : {car.Combined}");
	}
}
```
If we wish to keep all the properties from both Car object and Manufacturer object, we can do this. Project both objects into one with two properties referencing to them. The downside is now we need to access the underlying object first before we can access the real property. So, if we still need accesses to both objects after `Join()` but don't want the output to behave like that, we can project one more time before we done with the query, like in the comment. 
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = cars.Join(manufacturers,
						  c => c.Manufacturer,
						  m => m.Name,
						  (c, m) => new
						            {
							            Car = c,
							            Manufacturer = m
						            })
					.OrderByDescending(c => c.Car.Combined)
					.ThenBy(c => c.Car.Name);
				  /*.Select( c => new
				                  {
					                  c.Manufacturer.Headquarters,
					                  c.Car.Name,
					                  c.Car.Combined
				                  });*/	
					
	foreach (var car in query.Take(10))
	{
		Console.WriteLine($"{car.Manufacturer.Headquarters} {car.Car.Name} : {car.Car.Combined}");
	}
}
```

By now, we know how to joint two sequences together in both syntaxes, but our criteria is one equality relationship, and we can actually provide two, or even more. Before we see the code, let's find out what it is to be like when joining two sequences together with two criterias. Let's say we want to join manufacturers into cars when the manufacturer name and the year are matched. In our manufacturers sequence, the years are all 2016. So, for an entry like TOYOTA, JAPAN, 2016, a corolla made in 2016 would match, but a corolla made in 2017 would be excluded. So you can understand different criterias as if they are connected with `&&` operator. Only when all the criterias are met, then the main sequence entry is kept and joined. How can we achieve that in code when all we have is one `equals` keyword? 

If you look at it from the perspective of method syntax, it might make more sense. Remember both selectors are `Func` delegates that share the same return type of TKey. And we can say the return type is an anonymous type that have one or more properties. We then can use such a type to work as criteria. But be careful that C# would only compare the properties that share the same name, that's why we need to explicitly assign the manufacturer.Name into a property named Manufacturer for that's the property name in Car object.
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = from car in cars
	            join manufacturer in manufacturers
	                 on new {car.Manufacturer, car.Year} 
	                    equals
	                    new {Manufacturer = manufacturer.Name, manufacturer.Year}
	            orderby car.Combined descending, car.Name ascending
	            select new
	            {
		            manufacturer.Headquarters,
		            car.Name,
		            car.Combined
	            };
	/* method syntax
	var query = cars.Join(manufacturers,
						  c => new {car.Manufacturer, car.Year},
						  m => new {Manufacturer = manufacturer.Name, 
                                    manufacturer.Year},
						  (c, m) => new
						            {
							            m.Headquarters,
							            c.Name,
							            c.Combined
						            })
					.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name);
	*/
	foreach (var car in query.Take(10))
	{
		Console.WriteLine($"{car.Headquarters} {car.Name} : {car.Combined}");
	}
}
```