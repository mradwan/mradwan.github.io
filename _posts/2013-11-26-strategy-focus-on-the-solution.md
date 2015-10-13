---
layout:     post
title:      Strategy Focus on the solution
date:       2013-11-26
categories: strategy
---

Focus on the solution involves looking at the problem solution and searching to see whether it has some specific properties that can help us in finding it, those little properties can make the solution much easier.

Lets solve some problems using this strategy

**[LittleElephatAndString](http://community.topcoder.com/stat?c=problem_statement&pm=12854) [Topcoder SRM 597]**

First let's define what is the solution ? is it the minimum number of characters ? No! that would make our task harder.

Lets say that the solution is the set of characters that we will move, and we 
would want to get a solution of minimal size.

Now lets focus on this and see the properties we can get, we'll do this by examining some test case
start = ABMKOOFRN, target = OMABKOFRN

~~~
ABMKOOFRN
  M O
AB K OFRN
OMABKOFRN
~~~


With little examination we can observe the following

1. The solution is a subsequence of the start string.

2. A permutation of the solution must form a prefix from the target string.

3. Removing the characters of the solution from the start strings, leaves us out with a subsequence of the original string which is also a suffix of the target string!



Now from the third property how do we get the solution set of minimum size?
Choose the longest suffix of the target string which is also a subsequence of the start string, remove it from the start string then the rest is the solution!

---
**[MysticAndCandies](http://community.topcoder.com/stat?c=problem_statement&pm=12997) [Topcoder SRM 608]**
Let n be the number of boxes, A be the set of all boxes, let S be a solution then it must satisfy the following

* Sum of candy in S ≥ X.
* We can't move candies from S to other boxes and make sum of candies in S < X


The first condition is obvious, while the second must be achieved to ensure that for any valid assignment of candies into boxes, S contains at least X candy pieces.
If we can't move candy from S to other boxes, either of these must be true

* Sum of candies in A-S is equal to the maximum number of candies that can be put in boxes in A-S
* Sum of candies in S is equal to the minimum number of candies that can be put in boxes in S

The answer then is either the minimum number of boxes that holds least X candy pieces, or n - maximum number of boxes that can hold at most C-X candy pieces.
We can calculate both greedily and choose the smaller.

---

**[TaroFriends](http://community.topcoder.com/stat?c=problem_statement&pm=12997) [TopCoder SRM 613]**

A valid solution here can be described by the left most, and the right most cat, each of those has either moved to the left or to the right by X, while all other cats have to be in between those, since N is small here we can try out all solutions and choose the valid solution which gives minimum difference between left most and right most cat.

**TAKE CARE!**

Usually trying out test cases might show you some properties, it might be misleading sometimes and it might not show you all the properties, so try working out your properties in a more generic way.