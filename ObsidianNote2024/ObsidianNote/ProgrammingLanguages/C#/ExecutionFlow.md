Sometimes, we want our code to be executed differently based on the inputs. The way we can control the execution flow is through the C# programming syntax, such as if statement, switch statement, foreach loop, etc. All these statements commonly exist across languages, but C# might just have its own little revision on them.

First, we learn the branching/switching of the execution flow. We might already know we can do this with if statement and switch statement, but let's backup a bit and examine the branching in a conceptual way. When the execution flow needs to go one way and not the other, it would need some kind of judgement to make its decision upon. And by not going the other way, we are skipping some written code. Greater level of programming, like software architecture, would skip code through design technique. However, when it comes down to the concrete example, and we have to solve the problem in this very spot, we have to use if statement or switch statement, there is no other chocie. So, don't get obsessed with not using if statement, just use it when you think you need it. There might be a better route, but the optimization happens in latter stage. Be aware of that, then you should be fine.

From a design standpoint, if statement is for making decision on two choices, and switch statement is for making decision on many choices. Sure, there is else if statement to create more branches, but a switch statement is simply more readable and easier to maintain in scenario where many different choices exist.

If statement uses boolean value to decide which way to go, and we should avoid nesting if statement whenever we can.

Following two pieces of code work just the same, but the first one is eaiser to read and maintain.

>if (grade <= 100 && grade >= 0)
>{
>	grades.Add(grade);
>}

>if (grade <= 100)
>{
>	if (grade >= 0)
>	{
>		grades.Add(grade);
>	}
>}

Before we study switch statement, let's use if statement to solve a problem that would have been better taken care of by using switch statement.

>public void AddLetterGrade(char letter)
>{
>	if (letter == 'A')
>	{AddGrade(90);}
>	else if (letter == 'B')
>	{AddGrade(80);}
>	else if (letter == 'C')
>	{AddGrade(70);}
>	else if (letter == 'D')
>	{AddGrade(60);}
>	else {AddGrade(0);}
>}

Now, let's use switch statement to express the same logic. You might argue that the switch statement somewhat requires more writting, and it doesn't seem to be a better alternative over the if statement. In some cases where the variable isn't compared against a single value, but a range of value, switch statement seems even worse than the if statement.

>public void AddLetterGrade(char letter)
>{
>	switch(letter)
>	{
>		case 'A':
>			AddGrade(90);
>			break;
>		case 'B':
>			AddGrade(80);
>			break;
>		case 'C':
>			AddGrade(70);
>			break;
>		case 'D':
>			AddGrade(60);
>			break;
>		default:
>			AddGrade(0);
>			break;
>	}
>}

Even Microsoft, the C# language developer would probably agree with you, so Microsoft gives the switch statement the ability of pattern matching. Notice the order matters, because d >= 80 is bigger set containning d >= 90. However, with this power, switch statement is just catching up with the functionality of if statement, but some people would use it because they think switch statement is easier to read.

>switch(result.Average)
>{
>	case var d when d >= 90.0:
>		result.Letter = 'A';
>		break;
>	case var d when d >= 80.0:
>		result.Letter = 'B';
>		break;
>	case var d when d >= 70.0:
>		result.Letter = 'C';
>		break;
>	case var d when d >= 60.0:
>		result.Letter = 'D';
>		break;
>	default:
>		result.Letter = 'F';
>		break;
>}



Second type of changeing execution flow is looping. There are four kinds of looping syntax in C#, the foreach loop, the for loop, the while loop, and the do-while loop. Looping is useful when dealing with collections of data, in which we often need to iterate through the data structure to get something done or retrieve something.

The foreach loop, the for loop, and the while loop are usually interchangeable in terms of functionality. However, the do-while loop would always execute the code once, then check for the condition to decide if the execution should go back and execute the code again. So only use the do-while loop, when you know you need to execute the code at least once, which put the do-while loop at a slightly special position.

All those loops use slightly different syntax. First, we check the foreach loop.

Foreach loop defines a variable and uses that variable to store each iterated value, then executes the code inside the curly brace. Those codes might or might not have anything to do with the variable, but either is fine. Also, using implicit typing for the variable might just make the code reusable. Foreach loop is the simplest and most readable looping syntax, so use it whenever possible. When is it not possible? 

>foreach (var grade in grades)
>{
>	result.Low = Math.Min(grade, result.Low);
>	result.High = Math.Max(grade, result.High);
>	result.Average += grade;
>}
>result.Average /= grades.Count;
>return result;

The foreach loop is a simplified version of for loop, but the for loop does provide a finer control over the iteration process. The for loop also defines a variable, and uses that variable as indexer to iterate through the collection. The following for loop works just the same as the foreach loop above. But as we can see, in a for loop syntax, we get to decide the starting value of the indexer, the looping criteria, as well as the change on the indexer after each iteration. And foreach loop under the hood just assumes the indexer to be zero, and it postincrements by one. If we want to skip the first element, we can change the starting value of the indexer to 1 with a for loop. But with a foreach loop, we don't have the ability to do so.

>for (var index = 0; index < grades.Count; index += 1)
>{
>	result.Low = Math.Min(grades[index], result.Low);
>	result.High = Math.Max(grades[index], result.High);
>	result.Average += grade;
>}
  result.Average /= grades.Count;
>return result;

While loop is very much like a for loop, but it takes the indexer part out of its own syntax. While loop defines the looping criteria, and that's it. We need to declear the indexer and modify the indexer by ourselves.

>var index = 0;
>while (index < grades.Count)
>{
>	result.Low = Math.Min(grades[index], result.Low);
>	result.High = Math.Max(grades[index], result.High);
>	result.Average += grade;>
>	index += 1;
>}
  result.Average /= grades.Count;
>return result;

Do-while loop is again very much like a while loop, except we first execute the code in curly brace, then we check for condition. We still need to handle indexer on our own, but notice, we need to put a semi-colon after the condition, unlike the other looping syntax.

>var index = 0;
>do
>{
>	result.Low = Math.Min(grades[index], result.Low);
>	result.High = Math.Max(grades[index], result.High);
>	result.Average += grade;>
>	index += 1;
>} while (index < grades.Count);
  result.Average /= grades.Count;
>return result;

Now that we learn how to iterate through collections of data, but sometims we also need a way to twist the iteration a bit, so we can end the iteration early or skip one or multiple iterations. The jumping statement is what we are looking for. They are used mainly along with iteration, so we normally don't see them in code outside iterations.
The two common jumping statements we'll learn are break statement and while statement. The break statement allows us to jump out of the entire iteration, so the iteration would end even if the looping criteria matches.

When execution reaches break statement, it jumps right out of the iteration, so the bottom three lines of codes would not be executed, even they are in the same current iteration.

>foreach (var grade in grades)
>{
>	if(grades == 42.1)
>	{break;}
>	result.Low = Math.Min(grade, result.Low);
>	result.High = Math.Max(grade, result.High);
>	result.Average += grade;
>}
>result.Average /= grades.Count;
>return result;

The continue statement would skip the current iteration, and jump to the next iteration.

>foreach (var grade in grades)
>{
>	if(grades == 42.1)
>	{continue;}
>	result.Low = Math.Min(grade, result.Low);
>	result.High = Math.Max(grade, result.High);
>	result.Average += grade;
>}
>result.Average /= grades.Count;
>return result;

There is also one jumping statement to mention, the goto statement. It allows us to jump to a point where we explicitly state. Here, when execution reaches goto staement, it skips not only the entire iteration, but also this line of code: result.Average /= grades.Count;
Goto statement is extremely rare, and it is not a recommended practice, the only reason we'll mention it here is if we encounter code like this, we would still be able to understand it.

>foreach (var grade in grades)
>{
>	if(grades == 42.1)
>	{goto done;}
>	result.Low = Math.Min(grade, result.Low);
>	result.High = Math.Max(grade, result.High);
>	result.Average += grade;
>}
>result.Average /= grades.Count;
>done:
>return result;