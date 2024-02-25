Aggregating means summarizing, so max value, min value, and average value, etc. are some forms of aggregating. Let's see how we can get that information out of the query using LINQ.

Here, we first group the cars by their manufacturer. Then, project it into anonymous type that only contains the information we care. From here we can see the built-in LINQ operators of Max(), Min(), and Average() work as their name suggests. Then, we put it into a reference so that we can put it into order. Very straightforward process.
```C#
var query = from car in cars
            group car by car.Manufacturer into carGroup
            select new
            {
	            Name = carGroup.Key,
	            Max = carGroup.Max(c => c.Combined),
	            Min = carGroup.Min(c => c.Combined),
	            Avg = carGroup.Average(c => c.Combined)
            } into result
            orderby result.Max descending
            select result;
foreach (var result in query)
{
	Console.WriteLine($"{result.Name}");
	Console.WriteLine($"\t Max: {result.Max}");
	Console.WriteLine($"\t Min: {result.Min}");
	Console.WriteLine($"\t Avg: {result.Aug}");
}
```

The problem with the query syntax is inside the projection, in order to get the max value, min value, and average value, we need to iterate through the collection three times, one for each value. That's inefficient. A better solution is the `Aggregate()` method, but it is only available in the method syntax. This method would iterate the collection once to get all the results, but how does it know what do we want? Well, it doesn't, that's why we actually need to provide an object for it to work. If we look at the `Aggregate()` method, we know it takes three parameters, and all three of them seem to be complicated. Essentially, the method wants an accumulator object. The first parameter is the accumulator object at its initial state. The second parameter is a `Func` delegate takes in the accumulator object, and a Car object, then return the accumulator object. This delegate method is called once per car object. The last parameter is also a `Func` delegate takes in the accumulator object and return something, not necessarily else. This delegate would be called once the iteration is finished.

We write the accumulator class called CarStatistics, and a newly created object of that is what we passed into the first parameter. Next, we have the first reference automaticall points to the accumulator object, and the second one points to car, because inside each group there are only car objects. We define a method in accumulator class to collecting information through iteration, and pass that in lambda expression to fir the second parameter. We also write a method to compute the final result once we collect all the data from iteration, and put that into lambda expression to fit the last parameter. The variable result stores the returned object by Compute(), which is the accumulator object. Then, using anonymous type, we project the accumulator object into something our foreach loop can handle. The methods we write in CarStatistics do not necessarily fit the delegate signature, since we can always use a lambda expression that fits the delegate signature to invoke the method like we do in the example.
```C#
var query = cars.GroupBy(c => c.Manufacturer)
                .Select(g =>
                {
	                var result = g.Aggregate(new CarStatistics(),
	                                         (acc, c) = > acc.Accumulate(c),
	                                         acc => acc.Compute());
	                return new
	                {
		                Name = g.Key,
		                Avg = result.Average,
		                Min = result.Min,
		                Max = result.Max
	                };
                })
                .OrderByDescending(r => r.Max);

foreach (var result in query)
{
	Console.WriteLine($"{result.Name}");
	Console.WriteLine($"\t Max: {result.Max}");
	Console.WriteLine($"\t Min: {result.Min}");
	Console.WriteLine($"\t Avg: {result.Aug}");
}
```

```C#
public class CarStatistics
{
	public CarStatistics()
	{
		Max = Int.MinValue;
		Min = Int.MaxValue;
		Count = 0;
		Total = 0;
	}
	public CarStatistics Accumulate(Car car)
	{
		Count += 1;
		Total += car.Combined;
		Max = Math.Max(Max, car.Combined);
		Min = Math.Min(Min, car.Combined);
		return this;
	}
	public CarStatistics Compute()
	{
		Average = Total / Count;
		return this;
	}
	public int Max {get; set;}
	public int Min {get; set;}
	public int Total {get; set;}
	public int Count {get; set;}
	public double Average {get; set;}
}
```
