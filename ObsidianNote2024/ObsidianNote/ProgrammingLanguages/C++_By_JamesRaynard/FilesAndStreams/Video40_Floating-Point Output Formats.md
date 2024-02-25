
When we output a floating point number, the default behavior is to show 6 significant digits max, meaning 6 figures after decimal point. Some numbers are output as fixed style, and others scientific notation.
```
double pi{3.141'592'653'5};
cout << pi << endl; // output 3.141593 not scentific notation

double c{299'792'458};
cout << c << endl; // output 2.99792e+008 scentific notation
```

To force the output in scientific notation, we have `scientific` manipulator, and `uppercase` manipulator would turn the exponent `e`, into `E`.
```
double pi{3.141'592'653'5};
cout << scientific << pi << endl; // output 3.141593e+000
cout << uppercase << pi << endl; // output 3.141593E+000

double c{299'792'458};
cout << c << endl; // output 2.997925e+008 scentific notation
```

The counter manipulator for `scientific` is `fixed`.
```
double c{299'792'458};
cout << fixed << c << endl; // output 299792458.000000
double e{1.602e-19};
cout << e << endl; // output 0.000000
```
The second one is the charge of an electron. It is so small, that it is rounded to zero with `fixed` style, which explains why the scientific notation is scientific.

Because the default behavior is neither always scientific notation nor fixed all the times, we have to have a manipulator to restore the default setting which is `defaultfloat` manipulator.

The 6 significant digits can be set with the `setprecision()` manipulator. It takes in an integer to denote how many significant digits to output.