Instead of searching every elements like linear search algorithm, binary search compares the desired value to the mid-point of an array and if the desired value is bigger, then it does the comparison again with the new mid-point, this time the mid-point of the second half of the array. Repeat this process until the desired value is found or we run out of element to check. In order to work, the array needs to be sorted, and not working for stacks or queues or linked list?

![[AlgorithmPt.1/BinarySearch/GetImage.png]]

![[AlgorithmPt.1/BinarySearch/GetImage (1).png]]

Binary search in flow chart
![[AlgorithmPt.1/BinarySearch/GetImage (2).png]]

Binary search in pseudocode
![[AlgorithmPt.1/BinarySearch/GetImage (3).png]]

Worst-case time complexity
![[AlgorithmPt.1/BinarySearch/GetImage (4).png]]
-the number of dividing in half procedure is the floor of (log2 n), where n is the array size, and log base is 2.

Pizza splitting example
![[AlgorithmPt.1/BinarySearch/GetImage (5).png]]
Searching problem abstraction 

Sometimes, we don’t need an actual array to apply searching algorithm. When we know what the solution or desired outcome looks like, we can apply searching algorithm to try all possible solutions. Like f(x) = y, when we know what y looks like, we can search all the x to try which generates the desired f(x).