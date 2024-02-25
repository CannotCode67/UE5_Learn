We use LINQ to construct queries, but first we need to have some data first. One of the most common data formats for storing data is CSV format, standing for comma separated values. Once we have our CSV file in the project, we can load it in using the File class under the System.IO namespace. File class provides many static methods for interacting with files on hard drive, including bring them in. The `ReadlAllLines()` method takes in a path to the file and it assumps the file is a text file, and bring the value into the program in a way where each line of the text file is treated as a string value and stored in a string array. So if the text file has 10 lines, the method would return with a string array with 10 items inside, each item is a string value equaling to the text in each line of the text file. 

Our example is such a file with information mainly about car and fuel efficiency. The file's first row is the header, so we use `Skip()` method to take it out of the array. Then, using `Where()` to filter out any string that contains 1 or less character. `Select()` method is often referred as projection, transform, or map, because what it does is take each string out of the array and turn it into something, not necessarily else. It takes in a `Func<Tinput, Toutput>`, here Tinput automatically gets the value of string, so we need to give it a method takes in a string value, and return a value of type Toutput. Because this process is a bit tedious, writing it inline impacts the readability, we put the method under the Car class to make the class be reponsible for parsing the string. `ToList()` would turn it into a List. Inside the comment, there is an equivalent code in query syntax.
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	foreach (var car in cars)
	{
		Console.WriteLine(car.Name);
	}
}
private static List<Car> ProcessFile(string path)
{
	return File.ReadAllLines(path)
			   .Skip(1)
			   .Where(line => line.Length > 1)
			   .Select(Car.ParseFromCsv)
			   .ToList();
	/*var query = from line in File.ReadAllLines(path).Skip(1)
	              where line.Length > 1
	              select Car.ParseFromCsv(line);
	  return query.ToList()*/
}
```
We here, basically get the information about each car and its fuel efficiency information in string format, and need to parse the string into different sections to feed multiple properties in a Car object. The `Select()` would call this method iteratively and yield return each new Car object to form the `IEnumerable<Car>`. First, we use `Split()` method in string class to split the one big string into small sections of string, and put them in an array. We then use different Parse methods to turn each string value into appropriate type. The example does that straightforward, but in reality, we should have codes to do some checking, fixing, error handling stuff as you may expect the parsing process is just full of surprises.
```C#
public class Car
{
	public string Manufacturer {get; set;}
	public string Name {get; set;}
	public double Displacement {get; set;}
	public int Cylinders {get; set;}
	public int City {get; set;}
	public int Highway {get; set;}
	public int Combined {get; set;}
	public static Car ParseFromCsv(string line)
	{
		var columns = line.Split(',');
		return new Car
		{
			Year = int.Parse(columns[0]),
			Manufacturer = columns[1],
			Name = columns[2],
			Displacement = double.Parse(columns[3]),
			Cylinders = int.Parse(columns[4]),
			City = int.Parse(columns[5]),
			Highway = int.Parse(columns[6]),
			Combined = int.Parse(columns[7])
		}
	}
}
```

Previously, we learnt two LINQ methods `OrderBy()` and `OrderByDescending()` would sort the query according to some given property/field. But just like sorting in general, we might run into situations where multiple items have the same value for that property, and if we don't give further instruction, C# would just randomly order them. So, the way to add a secondary sort is to use `ThenBy()` method or `ThenByDescending()` method. They work just the same, and we can use them multiple times to add tertiary sort and more. Let's see the code in both syntaxes.
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	var query = cars.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name);
	
	/*var query = from car in cars
	              orderby car.Combined ascending, car.Name ascending
	              select car;*/
					
	foreach (var car in query.Take(10))
	{
		Console.WriteLine($"{car.Name} : {car.Combined}");
	}
}
```

Next, we see `Where()` in both syntaxes, and by convention, we use single letter to denote the item in method syntax, and we use more descriptive expression in query syntax. Now, we are only keeping the cars which was made by BMW in 2016.
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	var query = cars.Where(c => c.Manufacturer == "BMW" && c.Year == 2016)
					.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name);
	
	/*var query = from car in cars
				  where car.Manufacturer == "BMW" && car.Year == 2016
	              orderby car.Combined ascending, car.Name ascending
	              select car;*/
					
	foreach (var car in query.Take(10))
	{
		Console.WriteLine($"{car.Name} : {car.Combined}");
	}
}
```
There are two LINQ methods `First()` and `Last()`, which are only available in method syntax. Their functionalities are as their names suggest, take the first item of the query, and take the last item of the query. They return the item directly instead of a collection of 1 item like other methods. The reason we mention them because we can add one of them to the end of the LINQ statement like this. Or we can use them as `Where()`, giving it a criteria, but we only get the first/last one meeting the criteria. Beware when they are used like this, they can throw an exception if no item matches the criteria. So, there is a slightly different version for each of them called `FirstOrDefault()` and `LastOrDefault()`, which return a default value if no item matches the criteria, in this case, it would be null. Our example would still throw an exception because we don't check if it is null before using the variable in WriteLine();
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	var query = cars.Where(c => c.Manufacturer == "BMW" && c.Year == 2016)
					.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name)
					.First();
	/*var query = cars.OrderByDescending(c => c.Combined)
					  .ThenBy(c => c.Name)
					  .First(c => c.Manufacturer == "BMW" && c.Year == 2016);*/
	Console.WriteLine(query);
}
```

Some LINQ operators are quantifiers, meaning they tell us if anything matches a predicate or a specific item is inside the collection, and they all return a boolean value. As you may suspect, a LINQ method returns a concrete type usually belongs to immediate execution category, and you are right. First, we look at method `Any()`. When we don't pass in any thing, it is basically asking whether the collection contains any item, if it returns false, then the collection is empty. But we can also pass in criteria, and `Any()` checks item against the criteria in a lazy manner, like streaming. It checks one at a time, and stops checking as soon as it finds one item matching the criteria. Another method `All()` also has this behaviour, but it is checking if there is any item that doesn't meet the criteria, it stops once it finds one. `All()` is basically asking if all the items are meeting the criteria.
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	var result1 = cars.Any(c => c.Manufacturer == "Ford"); // return true
	var result2 = cars.All(c => c.Manufacturer == "Ford"); // return false
	var result3 = cars.Contains(c => c.Manufacturer == "Ford");
}
```
The LINQ method `Contains()` would use reference equality to check if an item is in the collection or not. The name is very familiar because many built-in collections implement this method already, but LINQ still provides it in case if we work with a collection that does not.

There is a syntax in C# called anonymous type, where we as the name suggests, don't provide the name of the type. One of its use cases is when we do projection, so the `Select()` method again.
In both syntaxes, we project the car item into something we define inline, basically saying this type has three fields respectively storing the car item's Manufacturer value, Name value, and Combined value, and those fields would automatically have the names Manufacturer, Name, Combined respectively. That's why inside the foreach loop we can access those fields directly.
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	var query = cars.Where(c => c.Manufacturer == "BMW" && c.Year == 2016)
					.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name);
	var result = cars.Select(c => new {c.Manufacturer, c.Name, c.Combined});
	/*var query = from car in cars
				  where car.Manufacturer == "BMW" && car.Year == 2016
	              orderby car.Combined ascending, car.Name ascending
	              select new
	              {
		              car.Manufacturer,
		              car.Name,
		              car.Combined
	              };*/
					
	foreach (var car in result.Take(10))
	{
		Console.WriteLine($"{car.Name} : {car.Combined}");
	}
}
```

`SelectMany()` method is called a flattening operator because it can flatten the hierarchy of collections. Imagine we have an array, and each contained item is also an array, so effectively, we have a matrix. What `SelectMany()` would do is bring the element inside each inner array out and put them in one collection, maybe an aray, effectively getting rid of one layer of array hierarchy. 

Let's demonstrate it with an example. In case you don't know, string is a collection of char. And we are iterating through a list of car's names, and inside each iteration, we iterate through the characaters of the name and print them out in console. The comment contains the code using `SelectMany()` to do the same thing.
```C#
static void Main(string[] args)
{
	var cars = ProcessFile("fuel.csv");
	var query = cars.Where(c => c.Manufacturer == "BMW" && c.Year == 2016)
					.OrderByDescending(c => c.Combined)
					.ThenBy(c => c.Name);
	
	var result = cars.Select(c =>c.Name);
	foreach (var name in result)
	{
		foreach (var character in name)
		{
			Console.WriteLine(character);
		}
	}
	/*var result = car.SelectMany(c => c.Name)
	  foreach (var character in result)
	  {
		  Console.WriteLine(character)
	  }
	  */
}
```