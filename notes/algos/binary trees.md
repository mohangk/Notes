# Different type of searches

## sequential
- averageTime (n+1 )/ 2
- worseTime - n

## binary
- O(log n)

## red-black tree (*)

A red-black tree is a binary search tree that is empty or in which the root element is colored black, every other element is colored red or black and the following properties are satisfied:
Red Rule: If an element is colored red, none of its children can be colored red.
Path Rule: The number of black elements must be the same in all paths from the root element to elements with no children or with one child.


# Binary trees

## height 
- of a binary tree is the number of branches between the root and the farthest leaf, that is, the leaf with the most ancestors. 

## level

## two trees

either is empty or in which each non-leaf has 2 branches going down from it.

## full
full if t is a two-tree with all of its leaves on the same level. 
a binary tree T is full if each node is either a leaf or possesses exactly two child nodes.

## complete 
if t is full through the next-to-lowest level and all of the leaves at the lowest level are as far to the left as possible. 	
a binary tree T with n levels is complete if all levels except possibly the last are completely full, and the last level has all its nodes to the left side.


## Full Binary tree theorem
- I = internal nodes, L = leaves, N = total number of nodes
- N = I + L
- L = I + 1
- N = 2I + 1
- I = (N-1)/2
- L = (N+1)/2

Basically, this theorem says that the number of nodes N, the number of leaves L, and the number of internal nodes I are related in such a way that if you know any one of them, you can determine the other two.

## Traversal

### In order traversal: Left -> Root -> Right
=> worstTime(n)

  31
25   47
     42 50

25 31 42 47 50

### post order traversal : Left -> Right -> Root
- postOrder traversal produces postfix notation.


### preOrder traversal: Root -> Left -> Right
- produces prefix notation
- also known as depth-first-search =>  search goes to the left as deeply as possible before searching to the right

### breadthFirst Traversal - Level by level
- worst time is N 
- first in first out

# Binary Search trees

- require logarithmic time on average, linear time in worse case - for inserting, removing and searching
- TreeSet - logarithmic in the worse case 



