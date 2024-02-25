How can we tell if one algorithm is better than another? And to what extent? 

-The sole purpose of measurement standards is to answer questions above.  

Basic ideas of how to do it 

1.Counting instructions and fomulate the expression for curve 

2.Define the curve where variable x is the input size (N) and variable y is the execution time 

3. Different types of curves indicate different performance characteristic of algorithms 

Counting instructions and formulate the curve expression, example 1![[Algorithm/MeasurementStandard/MeasurementStandard-images/GetImage.png]]

Asymptotic Performance 

-When N gets very large, constant factors can be ignored. 

-So, Complexity (N): N *(some number) 

-the value of (some number) depends on various factors like hardware, programming language, and running envirnment, etc. 

-the most valuable information is the expression indicates a linear curve. 

Counting instructions and formulate the curve expression, example 2
![[Algorithm/MeasurementStandard/MeasurementStandard-images/GetImage (1).png]]

Asymptotic Performance 

-When N gets very large, constant factors can be ignored, and lower order of N can be ignored. 

-So, Complexity (N): N^2 * (some number) 

-this expression indicates a quadratic curve.

When both the upper bound and the lower bound of a function are defined as below, we use big Theta to denote that.
![[Algorithm/MeasurementStandard/MeasurementStandard-images/GetImage (3).png]]

When only the upper bound a.k.a the worst case is defined as below, we use big O to denote that. It is the most useful one in practice.
![[Algorithm/MeasurementStandard/MeasurementStandard-images/GetImage (2).png]]

When only the lower bound a.k.a the best case is defined as below, we use big Omega to denote that.
![[Algorithm/MeasurementStandard/MeasurementStandard-images/GetImage (4).png]]

Amortized Complexity 

-complexity of multiple calls 

-need more information