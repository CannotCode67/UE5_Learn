Grouping is basically creating a hierarchy of collection. First, there is a collection of groups, so multiple groups are multiple items in this collection. Then, each group itself is a collection of items that belong to this group. With grouping, we have a collection full of collections. This is what grouping would do to our data, given our data is a collection of items.

Now, we go into code to see it in action in query syntax. The keyword is very straightforwrd, group something by something. The interesting part is now different values from car.Manufacturer become multiple `IEnumerable<T>` collections. All of them are put into one big collection query. We need to access the Key property to reach the value, and under each of them, multiple cars are stored, but I guess their Manufacturer property are gone. When we see a foreach loop nested inside another foreach loop, we know we have a hierarchy of collection here.
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = from car in cars
	            group car by car.Manufacturer;

	foreach (var group in query)
	{
		Console.WriteLine(group.Key);
		foreach (var car in group.OrderByDescending(c => c.Combined).Take(2))
		{
			Console.WriteLine($"\t{car.Name} : {car.Combined}");
		}
	}
}
```

Although it looks very straightforward, there are a few pitfalls we need to watch out for. After the grouping, we no longer have the access to car since the collection is different now, and its item is not car. You may say, then we should have the access to each group, right? Well, not until we specify it explicitly using the keyword `into`. However, that's not required in method syntax. Two little details to mention in this example. In query syntax, if we don't put ascending or descending after the orderby, C# would treat it implicitly as ascending. The "\t" inside the string interpolation is to tell it to add indentation, short for tab.
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = from car in cars
	            group car by car.Manufacturer into manufacturer
	            orderby manufacturer.Key;
	/*
	var query = cars.GroupBy(c => c.Manufacturer.ToUpper())
					.OrderBy(g => g.Key);*/
					
	foreach (var group in query)
	{
		Console.WriteLine(group.Key);
		foreach (var car in group.OrderByDescending(c => c.Combined).Take(2))
		{
			Console.WriteLine($"\t{car.Name} : {car.Combined}");
		}
	}
}
```

Now that we know grouping and joining, we can perform a groupjoin operation to our data. It is very similar to do the joining first, the grouping second, but it can do all that at once. Let's see it in code. Like grouping, groupjoin would also create a hierarchy of collection, and when it is done, the outer sequence should be the outer collection. Here, we want the cars to be grouped by manufacturer, and have the headquarter information from the other data set. So, the manufacturers become the outer sequence and cars become the inner sequence. We do our usual selectors stuff like joining. Then, the keyword `into` would turn the joining into groupjoining. The difference is with grouping, we only have the access to each group, but here we still have the access to manufacturer, and the carGroup. That's probably because the underlying data structure formed by groupjoin is slighly different. My guess is now, the entry in manufacturer sequence is just like what it used to be except one more property, which is a collection of cars. I am not sure, sicne the simplest projection would be projecting itself if my guess is true, but now it is an object with two properties referencing to manufacturer object and carGroup object. If my guess is true, then we should have the access to carGroup through manufacturer, and we are double referencing here. We should test it once we have the chance. The method syntax for groupjoin is like joining, requiring a projection for its last parameter to turn both the manufacturer and carGroup into something. By now we should know the access code in foreach loop has to adopt with the projection we define in the query.
```C#
static void Main(string[] args)
{
	var cars = ProcessCars("fuel.csv");
	var manufacturers = ProcessManufacturers("manufacturers.csv");

	var query = from manufacturer in manufacturers
				join car in cars on manufacturer.Name equals car.Manufacturer
					into carGroup
				orderby manufacturer.Name
				select new
					   {
						   Manufacturer = manufacturer,
						   Cars = carGroup
					   };
	/*
	var query = manufacturers.GroupJoin(cars, m => m.Name, c => c.Manufacturer,
										(m, g) => new
												  {
													  Manufacturer = m,
													  Cars = g
												  })
							 .OrderBy(m => m.Manufacturer.Name);*/
					
	foreach (var group in query)
	{
		Console.WriteLine($"{group.Manufacturer.Name}:
							{group.Manufacturer.Headquarters}");
		foreach (var car in group.Cars.OrderByDescending(c => c.Combined).Take(2))
		{
			Console.WriteLine($"\t{car.Name} : {car.Combined}");
		}
	}
}
```
One last thing to show is the keyword `into`, even though we have used it in the groupjoin as well as afer the group by. What it does is essentially putting what we have into a reference. So, we can use it after the select in query syntax, so that a quey syntax would not end and be able to proceed to further inspection.
```C#
	var query = from manufacturer in manufacturers
				join car in cars on manufacturer.Name equals car.Manufacturer
					into carGroup
				orderby manufacturer.Name
				select new
					   {
						   Manufacturer = manufacturer,
						   Cars = carGroup
					   } into result
				group result by result.Manufacturer.Headquarters;
```
Now, the query would have those anonymous objects grouped by the Headquarters. To access the Headquarters, we know need to do it through the Key property. To get all the cars out, we need to go through each Headquarter collection, and we have the Cars collection to iterate through. All of sudden, we find ourselves in a situation where `SelectMany()` is useful.
```C#
foreach (var group in query)
{
	Console.WriteLine($"{group.Key}");
	foreach (var car in group.SelectMany(g => g.Cars))
							 .OrderByDescending(c => c.Combined)
							 .Take(3))
	{
		Console.WriteLine($"\t{car.Name} : {car.Combined}");
	}
}
```
