---
layout:     post
title:      Reinventing the wheel
date:       2015-10-19
categories: Techniques
---
> Do not reinvent the wheel

That's a well known concept in software engineering, which suggests that we should use existing set of functions and libraries instead of re-implementing them.

In this post I am suggesting the same in problem solving, if you encounter a problem which requires a modification in some algorithm, try doing more effort to see if you could maybe modify the input and the output of the problem or do more analysis and be able to use the *vanilla algorithm* or provided functions in the existing libraries.

Using existing implementations or algorithms as have a number of advantages

1. You're moving far away from bugs.
2. An algorithm solves a *problem*, so using the existing algorithm without modifying it, means that you can use any other algorithm that solves the same *problem*, so you're no longer bound to a specific implementation.
3. It's easier to communicate your ideas with other people, since you are discussing abstract ideas (i.e RMQ), not Implementation details (Segment tree).
4. Shorter code in case we're using existing libraries.

Let's go through some examples, I'll show you both ways to solve the same problem, hopefully you learn something new from each example.

#### Sorting
Say you're using a programming language that only allows sorting in ascending order, and does not provide option for implementing your own comparators or so.

So you're given an array of integers, and you want to sort it in descending order.

##### Reinventing the wheel

Well, you code your own sorting algorithm

~~~
void sortDescending(int[] a){
	quickSort(a);
}

void quickSort(int[] arr, int low, int high) {
 	if (low >= high)
		return;
	// .. quick sort code
	while(i <= j){
		while (arr[i] > pivot) i++;
		while (arr[j] < pivot) j--;
		if (i <= j) {
				// swap i, j
		}
	}
	// more quick sort code
}
~~~

##### Modifying the input
~~~
void sortDescending(int[] a){
	for(int i = 0;i<a.length;i++)a[i]*=-1;
	sort(a); //provided in existing library
	for(int i = 0;i<a.length;i++)a[i]*=-1;
}
~~~

Well much shorter, the only drawback here is that 32-bit ranges from -2147483648 to 2147483647, so negating -2147483648 might not really give us what we expect.

That was a very simple example that demonstrates that we could get **shorter code** by doing this, let's move on to a more complicated one. 

---

#### Any source Any destination shortest path
Given a weighted graph and a set of sources, set of destinations we would like the shortest path from any source to any destination (we only want one path).

This can be solved using either dijkstra or bellman ford's algorithm for shortest path

##### Reinventing the wheel

###### Disjktra
We initialize the distance of each of the sources to zero, and we push them all into the priority queue, we stop whenever we pop any destination from the priority queue.

###### Bellman ford's 
We initialize the distance of each of the sources to zero, the answer is the minimum distance in any of the destinations.


##### Modifying the input
We modify our graph by adding two special node, super source and super destination, connect the super source to all sources with edges of cost zero, connect all destinations to the super destination with edges of cost zero, now the answer is the shortest path from the super source to the super destination.

We can use **any** single source shortest path algorithm to solve this problem without knowing about the implementation details of this algorithm.

---

Here's one last example that is harder than the previous ones, this problem actually encouraged me to write this whole post

#### [Riddle](http://main.edu.pl/en/archive/pa/2010/zag) [Algorithmic Engagements 2010]

We have a graph, with *n* nodes and *m* and *k* groups of nodes, we want to choose a single node from each group and color it, such that each edge has at least one of it's end points colored.

Sounds like an easy 2-SAT problem, go read [this post](http://mradwan.github.io/algorithms/2014/10/14/solvable-sat/) if you don't know how to solve 2-SAT.

We need to find a coloring that satisfies the following **conditions**

1. If there's an edge connecting A to B, then one or more of them has to be colored *$$(A \lor B)$$ must be true, thus A being false implies that B is true, and B being false implies that A is true*.
2. If Nodes A, and B were in the same group then either of them has to be not colored *$$(¬A \lor ¬B)$$ must be true, thus A being true implies B is false, and B being true implies A is false*.

Easy huh, but the problem with this idea is that it has too many clauses because for each group of size x, we have $$ x * (x - 1) $$ clauses, thus we might actually end up with O(n^2) clauses trying to solve it this naive way.

##### If you have a hammer ... 
Recall that 2-SAT can be solved by finding strongly connected components in the implication graph where the edges are formed by the implications we stated above, and checking that no variable and it's complement are in the same connected component, We can use [Kosaraju's Algorithm](https://en.wikipedia.org/wiki/Kosaraju%27s_algorithm) for this matter.

To get around the huge number of clauses (Which comes from the groups) problem we modify our dfs function to take into account the "edges" in between each group without actually having to store the edges or representing them in our memory or even iterating over all of them.

After all a group of size k can be traversed using k edges, cause all we care about here is the dfs order/tree here so we don't have to pass by all the edges.

The idea is to have a set for each group saying which nodes in this group are still unvisited, and whenever we visit each node we first iterate over the edges  coming from **condition 1** (call those normal edges), then while the group of this node still contains unvisited nodes we do dfs at the first unvisited node.

see the simplified implementation below to get the idea.

~~~
// a 2D array containing the groups
int groups[k][?];
int groupNum[n]; // number of group of each node
bool visited[n]; // visited or not by dfs
//next node's to visit in each group
int next[k];

int getNextUnvisitedNode(int groupNum){
	while(next[groupNum] < group.length && visited[next[groupNum]])
		next[groupNum]++;
	if(next[groupNum] == group.length) return -1;
	else return next[groupNum];
}

void dfs(int node){
	if(visited[node])return;
	visited[node] = true;
	for(i in node.normalEdges)
		dfs(i);
	while(getNextUnivistedNode(groupNum[node])!=-1)
		dfs(getNextUnvisitedNode(groupNum[node]));
}
~~~~

This solves the problem, we can implement Kosaraju's algorithm using this dfs function and get the correct answer, however if we decided to use tarajan's algorithm to find the strongly connected components this may not work, as tarajan's algorithm requires passing over all the edges of the graph.

We can't also use other algorithms for solving 2-SAT as the random walk algorithm, that's because when we faced a problem at the high level we decided to solve it on the level below, which is the *implementation* level.

We modified the traditional dfs function to a custom one, that works for special kinds of graphs.

##### A little bit more analysis, and ... variables

So let's take a look again at the expression we want to solve, the expression looks like this

$$(A \lor B) \land (C \lor D) \land (Exactly\ one\ from[D,E,F,G,H,I,J]) ... $$

the problem is arising from the *Exactly one from* clause, in the previous solution we exchanged this clause by a number of clauses that is quadratic to the number of variables, can we do better? is there a way to change this exactly one clause to a set of clauses linear in the number of variables ?

Let's first write each group as a sequence of nodes, where $$S_i$$ is true if the ith node in the sequence is colored, false otherwise.

|Node|$$S_0$$|$$S_1$$|$$S_2$$|$$S_3$$|$$S_4$$|$$S_5$$|$$S_6$$|
|-|-|-|-|-|-|-|-|
|is Colored?|F|F|F|T|F|F|F|

Let's introduce two variables for each node, $$P_i$$ which is true if and only if the colored in the group occurred before this node in the sequence, N_i which is true if this node is colored, or the colored node occurs after this node in the sequence.

|Node|$$S_0$$|$$S_1$$|$$S_2$$|$$S_3$$|$$S_4$$|$$S_5$$|$$S_6$$|
|-|-|-|-|-|-|-|-|
|is Colored?|F|F|F|T|F|F|F|
|$$P_i$$|F|F|F|F|T|T|T|
|$$N_i$$|T|T|T|T|F|F|F|

To propagate the $$S_i $$, $$P_i$$ to next nodes these clauses must be true

$$(¬S_i \lor P_{i+1}) \land (¬P_{i} \lor P_{i+1}) $$

To propagate the $$S_i$$, $$N_i$$ to the previous nodes these clauses must be true

$$(¬S_i \lor N_{i}) \land (¬N_{i+1} \lor N_{i}) $$

To prevent selecting more than one node and coloring it, we state that for each node, either the colored node is (before it in the sequence) or (at it or after it)

$$ (¬P_{i} \lor ¬N_{i}) $$

These clauses provided are true if and only if At most one variable is true, not really what we wanted in the first place but it turns out that we don't need more than this, since by this way we're guaranteed to color at most one node from each group, an assignment, if one group ends up with no nodes colored we can simply color any of them.

I don't think that there's a way to reduce an *Exactly one from* clause to normal 2-SAT clauses, also notice that the quadratic method we used in the first solution still transforms it to an *At most one from* clause.

Now we've exchanged the *At most one from* clause to 5 normal 2-SAT clauses, which is linear, we combine those clauses with the ones from **condition 1**. clauses and feed them into **ANY** algorithm that solves the 2-SAT problem, and we'll get the answer to our problem!

I've learned about this from [Looking for a Challenge book](http://www.lookingforachallengethebook.com/).

This example shows the beauty of solving problems in the higher level where we speak in terms of problems and reductions from one problem to another, not in terms of algorithms or code.