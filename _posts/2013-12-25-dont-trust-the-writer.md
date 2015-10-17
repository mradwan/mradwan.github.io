---
layout:     post
title:      Strategy Don't trust the writer
date:       2013-12-25
categories: Strategy
---

This is not really much of a strategy, this is something you should do while reading a problem, whenever you see a constraint in the problem statement that a solution must satisfy revisit this constraint, asking yourself do I really need to care about this ?
Many times writers add more stuff to the problem statement to confuse people or make the problem harder to solve.

I'll try not to give many hints for the following problems, and focus on showing what's deceiving in the problem statement.

#### [Lock Pattern](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=549&page=show_problem&problem=4334)[ICPC Regionals Arab 2012]
The problem here is asking to count some sort of hamiltonian path in a graph with N (N≤12) nodes.

The path length must be less than or equal to an upper bound L.

But looking at the constraints we see

> L (1 ≤L≤1000) is the length of the pattern

The distance between any two nodes in this graph is at most 3 + 4 = 7, so a hamiltonian path would have at most (N-1) * 7 which is at most 11 * 7 = 77, we can even obtain better bounds.

A constraint that is higher than required might make you think that your solution would exceed the time limit because you have to visit too much states or do more iterations. 


#### [Contestants Division](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1695)[ICPC Regionals Asia Shanghai 2006]

> Due to the high cost of the new judging system, the organizing committee can only afford to set the system up such that there will be only one way to transfer information from one university to another without passing the same university twice.

It's a tree!

> Each test case starts with two integers N and M , (1≤N≤100000, 1≤M≤1000000) , the number of universities and the number of direct communication line set up by the committee

Too many edges, no its not ?

Yes it is.

#### [Free Market](http://codeforces.com/contest/364/problem/B)[Codeforces round #213]
> Note that each item is one of a kind and that means that you cannot exchange set {a, b} for set {v, a}. However, you can always exchange set x for any set y, unless there is item p, such that p occurs in x and p occurs in y.

If you want to exchange {a, b} for {a, v} just do exchange {b} for {v}, this means you can exchange any set for any other set as long as the difference in values is less than or equal d, this makes the problem a lot simpler since we only need to care about the value sums.

#### [RooksPlacement](http://community.topcoder.com/stat?c=problem_statement&pm=7658)[Topcoder SRM 354]

> You are to calculate the number of ways to place **K** rooks on a **NxM** chessboard in such a way that no rook is attacked by more than one other rook.

And then says

> A rook is attacked by another rook if they share a row or a column and there are no other rooks between them.

If there was a rook between them it would be attacked by two rooks then, so no three rooks can share a row or a column.


#### Additional problems
* [TheHomework](http://community.topcoder.com/stat?c=problem_statement&pm=8415)
* [Bad Roads](http://acm.timus.ru/problem.aspx?space=1&num=1527)
* [Casino](http://codeforces.com/gym/100324)
