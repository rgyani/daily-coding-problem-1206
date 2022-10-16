# Daily Coding problem: 1206

This problem was asked by Uber.

A rule looks like this:

```
A NE B
```

This means this means point A is located northeast of point B.

```
A SW C
```

means that point A is southwest of C.

Given a list of rules, check if the sum of the rules validate. For example:

```
A N B
B NE C
C N A
```
does not validate, since A cannot be both north and south of C.

```
A NW B
A N B
```
is considered valid.


### Solution
First, let’s break down what it means for a list of rules to be invalid. Consider the following list of rules:
```
A N B
B N A
```
The second rule is obviously invalid, since the first node already stated that B is north of A.

We can also see that the following list is equivalent and also invalid:

```
A N B
A S B
```
So, we can see that two rules invalidate each other if they relate the same pair of points and are in opposite directions. 

However, two rules do not invalidate each other if they are in the same or direction or are orthogonal to each other:
```
A N B
A E B
```
In this case, we see that it is valid for A to be North of and East of B at the same time.

A N B
C N B
In this case, the relative position of A and C is ambiguous, other than that they are both north of B.

Let’s take a look at another example, similar to the first provided example:

```
A N B
B N C
C N A
```
In this case, we see that C cannot be north of A because it is implied that C is south of A by the previous two rules. 

Next, we need to figure out how to deal with the diagonal cardinal directions (e.g. NE, NW, SW, SE). Let's take a look at the case where there are two rules relating the same two points, and the directions are orthogonal (perpendicular) to each other:

```
A N B
A E B
```
We also notice that these two rules can be simplified into one: A NE B. Similarly, we can break down any diagonal direction into the two simple directions (N, E, S, W) that it is composed of.

Now, we can model the relationships between points as two graphs 
* to_north graph
* to_west graph


Consider the input again
```text
A N B
B NE C
C N A
```

we have the two graphs as

###to_north graph
```
A -> B
B -> C
C -> A
```

###to_west graph
```
C -> B
```

**Since there is a cycle in the to_north graph, the rule is invalid**

We can find cycles in either to_north or to_west using **Depth-First Search (DFS)**.   
If either graph contains a cycle, the rules are invalid.

Depth-First Search (DFS) is the most appropriate algorithm for finding cycles in a directed graph. Recall that DFS maintains two auxiliary data structures

* A visited set to indicate that all reachable nodes from the current node have been visited (and no cycles have been found), and
* An in_process set to indicate that the current node has started exploration, but not all reachable nodes from it have been explored.

A graph contains a cycle when a node which is present in the in_process set, and not present in the visited set is visited again! 

