---
draft: false
title: Algorithms study
categories:
  - Algos
---

## Big O

O(1) - linear
O(log n) - logarithmic
O(n) - linear
O(n logn) - linearithmic
O(n^2) - quadratic
O(2^n) - exponential
O(n!) - factorial

https://stackoverflow.com/questions/2307283/what-does-olog-n-mean-exactly

## Data structures

1. stack
2. queue
3. linkedlist
4. skiplist - access, search, insert, delete -> AVG: O(log(n)) Worst: O(n)
5. binary search tree - access, search, insert, delete -> AVG: O(log(n)) Worst: O(n)	


### trees

#### traversal

- depth first ->preorder (root, left, right), inorder (left, root, right) , postorder (left, right, root)
- breadth first ->

#### search
def search_recursively(key, node):
 
    if node is None or node.key == key: 
        return node
    if key < node.key:
        return search_recursively(key, node.left)
    else:
       return search_recursively(key, node.right)
       
       
### graph
- depth first search - Space complexity - O(m*n), Time complexity - O(n*m) -> stack
- breadth first search - Space complexity 2(m+n), Time complexity - O(n*m) -> queue





