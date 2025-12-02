## Data Structures
Data structure is efficiently storing data.
> e.g. Google Map (Dijkstra's Algorithm) shortest path calculation

**Memory and Running Time Tradeoff** - There is always a trade off between memory usage and running time of the underlying algorithm.  

### Abstract data types (ADTs)

They define the model (logical description) for the given data structure. It is like an Interface or Abstract class in programming. e.g.   
> Stack - push() and pop() with LIFO,  
Queue, Priority Queue, Associative Arrays

Data structures gives the concrete implementation for the abstract data type. Our goal is to achive time complexity closer to O(1).  
## Array Data Structure (Linear)
All the items of an array are stored next to each other in the memory. Items can be accessed by the index starting from 0.  
**Advantages - Accessing the items, random access O(1)**  
> memory address = array's address + index * data size

**Multidimensional Arrays**  
Every single item can be identified wit 2 indexes - *rowindex* and *columnindex*.  

### Types of arrays 
1. **Static Arrays** - Size of the array does not change.
1. **Dynamic Arrays** - Size of the array may change dynamically.

### Array operations

1. Adding items: O(1), resize operation takes O(N)
1. Adding item to an arbitary position: O(N) linear running time as the items have to be shiffted (in worst case, all of them).
1. Removing last item: O(1).
1. Removng an arbitary item: O(N) to search for the item, O(1) to remove that item, O(N) to shift the items.

## Linked List
The main aim is to be able to store items efficiently (insertion and removal operations).  
In arrays wheen adding items to an arbitary values, we have to shift the values on index ahead by one taking O(n) time.  
In linked list, we have the access to the pointer to the first node and other elements can be accessed starting with this node.  
Last node of the linked list is pointing to a NULL.  

### Linked list structure
Every node stores the data itself and a reference to the next node in the linked list data structure.  
Linked list require more memory that arrays.  
It has an advantage - there cannot be holes in the data structure so there is no need for shifting items.  
The items are not stored next to each other in the memory - so there is no random indexing.  
We can implement more compplex data structures and abstract data types such as stacks and queues.  
Finding arbitary item in the linked list still has O(n) linear running time.

## Linked list operations
1. Adding an item at the beginning: O(1)
1. Removing an item from the beginning: O(1)
1. Adding an item at the end of the linked list: O(n)
1. Removing item from the end of the linked list: O(n)


### Advantages of Linked list
Dynamic data structures.  
No need to resize.   
Can store different sized items.

### Disadvantages of Linked list
Require more memory.  
No random access.  
Cannot go backwards.
