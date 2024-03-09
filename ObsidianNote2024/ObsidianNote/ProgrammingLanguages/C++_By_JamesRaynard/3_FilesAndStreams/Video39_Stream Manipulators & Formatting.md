
We have seen `std::flush` and `std::endl`, but there are other stream manipulators. Most of them are define in `<isotream>` header. Manipulators that take arguments are defined in `<iomanip>` header.

Boolean value underneath is an integer with true being 1, and false being 0. When we output a Boolean value, we get the numeric value instead of "true" or "false". `boolalpha` is a manipulator that makes stream output a Boolean value in the form of "true" or "false".
```
int x = 2;
bool is_negative = x < 0;
cout << is_negative << endl; // output 0
cout << boolalpha << is_negative << endl; // output false
cout << (x==1) << endl; // still output false
cout << noboolalpha << false << endl; // output 0
```
We can see once `boolalpha` is used, the stream behavior would remain under affect (sticky) unless we cancel it with `noboolalpha`. 

Many stream manipulators are designed to format the output. The default output style would just output data without any space.
```
cout << "number" << 123 << endl; // output number123
cout << "character" << "abc" << endl; // output characterabc
```
What if we want a table-like style? `setw()` manipulator will pad the output field to make it the width of its argument. So `setw()` takes an argument which is the width of the output field. But `setw()` only affects the next data item, so we have to place it before every piece of data we want to have the affect on. This is called un-sticky, and  `setw()` is the only un-sticky manipulator we have. Since it takes in an argument, it is defined in `<iomanip>`. If the data is too big for the width, then output would just display the data as it is without formatting, even that data is more than the width.
```
cout << setw(15) << "number" << setw(15) << 123 << endl;
cout << set(15) << "character" << setw(15) << "abc" << endl;

//output be like
//         number            123
//      character            abc
```
We see the padding is placing on the left, and data is pushed to the right. This is call right-justified
output, and it is default behavior. To change that, we have the `left` manipulator. And as we mentioned all manipulators are sticky except for `setw()`, so to undo any sticky manipulator, we have to use the counter manipulator. For `boolalpha`, it is `noboolalpha`. For `left`, it is `right`.
```
cout << left << setw(15) << "number" << setw(15) << 123 << endl;
cout << set(15) << "character" << setw(15) << "abc" << endl;
cout << right << setw(15) << "number" << setw(15) << 456 << endl;

//output be like
//number         123            
//character      abc            
//         number            456
```
The padding used to fill the width can be changed with `setfill()`.
```
cout << setfill('#');
cout << left << setw(15) << "number" << setw(15) << 123 << endl;
cout << set(15) << "character" << setw(15) << "abc" << endl;
cout << setfill(' ');
cout << right << setw(15) << "number" << setw(15) << 456 << endl;

//output be like
//number#########123############
//character######abc############
//         number            456
```
To undo its work, we just place `setfill()` again with whitespace character.