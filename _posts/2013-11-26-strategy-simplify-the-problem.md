---
layout:     post
title:      Strategy simplify the problem
date:       2013-11-26
categories: strategy
---
This one is a very useful strategy and should always be applied while trying to approach a specific problem, however it might or might not work!

The idea is pretty simple, Keep making the problem simpler until you can solve, then try to extend the solution for the simple problem to work on the original problem.

Making a problem simpler might be reducing number of dimensions i.e 3D to 2D or 2D to 1D, making the input simpler or smaller or else.

A typical example would be calculating the greatest common divisor gcd for n integers, we can start off by trying to calculate the gcd for 2 integers a and which can be easily done using Euclidean algorithm, then we move onto three integers to learn that the gcd(a,b,c) must divide a and b and c, and since it must divide all the three integers, thus it must divide gcd(a,b) so we can see that gcd(a,b,c) = gcd(gcd(a,b),c), now we can easily solve the problem for n integers.

Lets solve some problems using this strategy

####X3[COCI] Find it [here](http://www.hsin.hr/2012/coci/contest1_tasks.pdf)
Given n integers find out the sum of their pairwise XOR

~~~
long result = 0;
for(int i = 0;i<n;i++)
    for(int j = i+1;j<n;j++)
        result+=a[i]^a[j];
~~~

such a solution will only work for small values of n.

so lets simplify the problem by assuming the input numbers are only zeroes and ones, now can we solve it?

The answer is pretty simple, a one will be added to the result only when a one is xor'd with a zero.

So for the simple instance of the problem result will be **count ones** multiplied by **count zeroes** i.e. **count ones \*(n-count ones)**

To extend this solution to solve the original problem we need one more thing, it's a special property in the XOR which is
The ith bit in ***a*** xor ***b*** only depends on the ith bit in ***a*** and the ith bit in ***b***.

Now that we know this property we can solve the problem for each bit independently, let count(i) be the number of integers in input having the ith bit set to one.

The result would be

$$ \sum 2^{i}count(i)(n-count(i)) $$

Where count(i) is the count of numbers having the ith bit set.

To solve problems using this strategy you must find the correct way to simplify the problem, usually practice helps to do this.

