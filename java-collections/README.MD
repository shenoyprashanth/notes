# Concise overview of Java Collections

- **Iterable** returns an **Iterator**

- **Iterator** provides a consistent way of iterating through a collection using **hasNext()**, and **next()**.

- **Collection** - Represents a collection of elements.	

- **Set** - Is a collection that **does not allow duplicate elements**.

- **HashSet** - Hashtable implementation of a set.

- **SortedSet** - is a collection that **does not allow duplicate elements** & the **elements are ordered**.

- **TreeSet** - Red-Black tree implementation of a SortedSet.

- **List** - A collection that supports **retrieval by index.**
- **ArrayList** - List backed by an Array.
- **LinkedList** - List backed by a doubly linked list.

- **Queue** - A collection that **supports FIFO**.
- Array based implementation of Queue.
- Linked list implementation of Queue.

- **Priority Queue** - Pull elements off the queue based on priority of the item.
- **Blocking queue**- The client gets blocked until an item has been added to the queue.
- **Delayed queue** - The item is not popped off the queue until a certain delay has expired.

- **Deque** - A collection that supports LIFO & FIFO. 
- Array based implementation.
- Linked list implementation.

- **Map** - Represents a key value datastructure. Operations supported are put(), get(), remove(), containsKey(), containsValue().

- **NavigableMap** - In case the specified key does not exist, there are operations to get the value associated with the nearest key (floor, ceiling).

- **Guava MultiMap** - Map where key can have multiple values.

- **Guava BloomFilter** - Probabilistic data structure, that functions like a set.  Can be used for testing membership. If BloomFilter responds with **false**, then the element definitely **does not exist**. However if it returns **true**, then the **element may or may not be there**.
