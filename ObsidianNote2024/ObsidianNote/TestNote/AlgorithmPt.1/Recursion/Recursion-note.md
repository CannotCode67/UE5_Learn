Recursion occurs when a thing is defined in terms of itself or its type, or called self referencing. 

Object or method exhibits recursive behavior when it has a simple base case and a recursive step. 

In computer science, recursion is the idea of reducing a problem to a sub-problem and solving sub-problems to eventually solve the original problem. 

There are two main ways to go about this: 1.decrease and conquer 2.divide and conquer. 

Decrease and conquer 

-The main idea of decrease and conquer is that if I have a general problem, we can start with an algorithm to solve a particular simpler instance of this problem for a particular input. Then for arbitrary inputs to the problem, we reduce our problem to the already solved simpler instance. Here, recursion is what we use to do the reduction. We keep applying the algorithm within itself until we get down to the simpler instance or base case as we call it.  

With a factorial example
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage.png]]
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (1).png]]
-one thing to remember is anything you can do with a recursion, you can do it with iteration, and vice versa. So, recursion and iteration are interchangeable.

With Euclidean algorithm example
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (2).png]]
-putting the recursive step in return is the simplest form of recursion, easy to write and understand.

Linear search algorithm in recursive form![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (3).png]]
-notice that recursion changes the code structure massively, but it does not change the idea of the algorithm, therefore, it would not alter the algorithm's efficiency.

Bubble sort algorithm in recursive form
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (4).png]]
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (5).png]]
-the difficulty to put bubble sort algorithm in recursive form is to identify the recursive step. 
-in each pass, the largest value would go to the right most spot of the vector/sub-vector. So, one pass can take care of one value at worst. When there is n elements in a vector, we use n-1 passes. With each pass, the vector becomes smaller with the rightmost element excluded. Therefore, one pass is one recursive step.


Insertion sort algorithm in recursive form![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (6).png]]
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (7).png]]

-again, identify the recursive step is even more difficult than bubble sort. 

-in this recursive insertion sort, the right most element checks its left-side elements to see find the first one that is smaller than it, then do a shift procedure. The best case would be the left-side elements are sorted. The smallest sub vector that needs to be sorted is a sub vector with only two elements. Then each time adding one more element, it would need to be sorted again. Here, it would take n-1 times of sorting procedure to take care of a vector of n elements. Therefore, one sorting procedure is one recursive step, here sorting procedure means code starting from j = r to Shift(vector, i, j). 

-notice this recursive form of insertion sort is somewhat different from the iterating one. 

-an iterating insertion sort starts from the second element of the vector, and moving to the right. But it performs sorting procedure n-1 times just like its recursive form. So there should be no difference in performance.



Creating Permutations from elements of a vector
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (8).png]]
-the idea is a permutation consists of all the elements as starting parts plus the permutations from a smaller sub vector(excluding the starting element).
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (9).png]]
-each recursive step would run a for loop to settle the starting parts and send the rest to next recursion, until there is only one element in the sub vector.


Recursion provides a way of thinking, designing algorithm. It is one of many ways to tackle algorithmic problems. If the desired algorithm is presented, there is no need to change it to recursive form because recursion doesn't alter the efficiency but does make it harder to understand.


Binary search algorithm in recursive form
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (10).png]]


Divide and Conquer 

-The idea of divide and conquer is recursively divide the input into smaller instances. Despite it sounds similar to decrease and conquer, they are actually similar to some extend. However, in divide and conquer, we usually divide the input without doing any comparison, we do comparison when we putting them together. While decrease and conquer, we usually extract the biggest or smallest element then repeat the same step on the smaller instance, and all these extracting(decrease) requires comparison at the start. But remember, there is a vague line between divide and conquer and decrease and conquer, they have different names just to offer two ways of thinking to solve the problem: cutting the input into two smaller instances or extracting a piece of element to make a smaller instance, do it repeatedly then solve the problem.


Algorithms in divide and conquer fashion 

Quick sort 

-setting the middle-position element as the pivot, then take it out of the array, set up two array: left and right, iterate through the array, elements smaller than the pivot are appended to the left array, elements bigger than the pivot are appended to the right array. Recursively do these steps on each of these two sub-arrays until there is only one element in all the sub-arrays.
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (11).png]]

![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (12).png]]

-in quick sort, we do comparison when dividing the inputs, but it is divide and conquer style for it splits the input into two(one of them can be empty). 

Merge sort 

-dividing the array using middle position as pivot, repeat the step until all sub-arrays contain only one element, merge the sub-array into bigger sub-array while comparing and rearranging the order, repeat this step until the original array is formed and sorted.
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (13).png]]
![[AlgorithmPt.1/Recursion/Recursion-images/GetImage (14).png]]
-here we do not compare elements when diving, we compare elements when putting sub-vectors back together. Again, a vague line between decrease and conquer and divide and conquer.
