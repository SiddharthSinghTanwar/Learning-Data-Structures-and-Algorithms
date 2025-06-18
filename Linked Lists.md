# Linked Lists

## 🔹 Time Complexities (Traditional Implementations)

| **Operation** | **Array** | **Singly Linked List** | **Doubly Linked List** |
| --- | --- | --- | --- |
| **Access** | O(1) | O(n) | O(n) |
| **Search** | O(n) | O(n) | O(n) |
| **Insert at head** | O(n) (need to shift) | O(1) | O(1) |
| **Insert at tail** | O(1) (if space) (amortized, if dynamic) or O(n) | O(n) (without tail pointer), O(1) (with tail pointer) | O(1) (with tail pointer) |
| **Insert at middle** | O(n) | O(n) | O(1) (if pointer to prev) /O(n)  (if search needed) |
| **Delete at head** | O(n) (need to shift) | O(1) | O(1) |
| **Delete at tail** | O(1) | O(n) | O(1) (with tail pointer) |
| **Delete at middle** | O(n) | O(n) | O(1) (if pointer to prev) / O(n) (if search needed) |

## 🔹 Space Complexities

- **Singly Linked List**:
    - O(n) for data + O(n) for `next` pointers.
- **Doubly Linked List**:
    - O(n) for data + O(n) for `next` + O(n) for `prev` pointers.

The **space complexity isn't as critical**, but **doubly linked lists use more memory** due to the extra pointer.

## Implementation

- Singly Linked List
    
    ```python
    class Node:
        """
        An object for storing a single node of a linked list.
        Models two attributes - data and the link to the next node in the list.
        """
    
        data = None 
        next_node = None
    
        def __init__(self, data):
            self.data = data
    
        def __repr__(self):
            return "<Node data: %s>" %self.data
    
    class LinkedList:
        """
        Singly Linked List
        """
    
        def __init__(self):
            self.head = None
    
        def __repr__(self):
            """
            Return a string representation of the list
            Takes O(n) time
            """
    
            nodes = []
            current = self.head
    
            while current:
                if current is self.head:
                    nodes.append("[Head: %s]" %current.data)
                elif current.next_node is None:
                    nodes.append("[Tail: %s]" %current.data)
                else:
                    nodes.append("[%s]" %current.data)
                current = current.next_node
            return '-> '.join(nodes)
    
        def display(self, index):
            """
            Displays the node at the matching index
            displays none if index doesn't match
            Takes O(n) time
            """
            
            if index == 0:
                return self.head
            
            current = self.head
            while current and index > 0:
                current = current.next_node
                index -= 1
            
            return current
    
        def is_empty(self):
            return self.head == None
        
        def size(self):
            """
            Returns the number of nodes in the list
            Takes O(n) time.
            """
    
            current = self.head
            count = 0
    
            while current:
                count += 1
                current = current.next_node
    
            return count
    
        def add(self, data):
            """
            Adds new Node containing data at head of the list
            Takes O(1) time.
            """
    
            new_node = Node(data)
            new_node.next_node = self.head
            self.head = new_node
    
        def search(self, key):
            """
            Search for the fist node containing data that matches the key
            Return the node or None if not found
            Takes O(n) time
            """
    
            current = self.head
            while current:
                if current.data == key:
                    return current
                else:
                    current = current.next_node
            return None
    
        def insert(self, data, index):
            """
            Inserts a new Node containing data at index position
            Insertion takes O(1) time but finding the node at the insetion point takes O(n) time
            Takes overall O(n) time
            """
    
            if index == 0:
                self.add(data)
    
            if index > 0:
                new_Node = Node(data)
    
                position = index
                current = self.head
    
                while position > 1:
                    current = current.next_node
                    position -= 1
    
                prev = current
                next_Node = current.next_node
    
                prev.next_node = new_Node
                new_Node.next_node = next_Node
    
        def remove(self, key):
            """
            Removes Node containing data that matches the key
            Returns the node or none if key doesn't exist
            Takes O(n) time
            """
    
            current = self.head
            previous = current
            found = False
    
            while current and not found:
                if current.data == key and current is self.head:
                    found = True
                    self.head = current.next_node
                elif current.data == key:
                    found = True
                    previous.next_node = current.next_node
                else:
                    previous = current
                    current = current.next_node
            
            return current
    
        def removeAt(self, index):
            """
            Removes Node from Linked list having 0-index position matching the index
            Returns the node or None if index doesn't exist
            Takes O(n) time
            """
    
            current = self.head
    
            if index == 0:
                self.head = current.next_node
                return current
            
            while index > 0:
                prev = current
                current = current.next_node
                index -= 1
            
            prev.next_node = current.next_node
            return current
    
        def node_at_index(self, index):
            """
            Returns the node at the matching index
            """
            if index == 0:
                return self.head
            else:
                current = self.head
                position = 0
    
                while position < index:
                    current = current.next_node
                    position += 1
    
                return current
    ```
    
- Doubly Linked List
    
    ```python
    class Node:
        def __init__(self, val) -> None:
            self.val = val
            self.prev = None
            self.next = None
    
    class MyLinkedList:
        def __init__(self) -> None:
            self.left = Node(0)
            self.right = Node(0)
            self.left.next = self.right
            self.right.prev = self.left
    
        def get(self, index):
            cur = self.left.next
            while cur and index > 0:
                cur = cur.next
                index -= 1
            if cur and cur != self.right and index == 0:
                return cur.val
            return -1
        
        def addAtHead(self, val):
            node, next, prev = Node(val), self.left.next, self.left
            prev.next = node
            next.prev = node
            node.next = next
            node.prev = prev
    
        def addAtTail(self, val):
            node, next, prev = Node(val), self.right, self.right.left
            prev.next = node
            next.prev = node
            node.next = next
            node.prev = prev
    
        def addAtIndex(self, index, val):
            cur = self.left.next
            while cur and index > 0:
                cur = cur.next
                index -= 1
            if cur and index == 0:
                node, next, prev = Node(val), cur, cur.prev
                prev.next = node
                next.prev = node
                node.next = next
                node.prev = prev
    
        def deleteAtIndex(self, index):
            cur = self.left.next
            while cur and index > 0:
                cur = cur.next
                index -= 1
    
            if cur and cur!=self.right and index == 0:
                next, prev = cur.next, cur.prev
                next.prev = prev
                prev.next = next
    ```
    

---

## 🔹 Notes on Modern Language `list` (e.g., Python)

In Python and similar high-level languages:

- Python’s `list` is actually a **dynamic array**, not a linked list.
- Operations like `append()` are **amortized O(1)** because Python over-allocates memory.
- Insertions and deletions (except at the end) are **O(n)** due to element shifting.

## 🔹 When to Use What

- **Array (Python list)**:
    - Fast random access (indexing).
    - Good for stacks, queues (with care), and frequent `append()`.
- **Singly Linked List**:
    - Efficient insert/delete at head.
    - Not commonly used directly in high-level languages due to overhead and lack of random access.
- **Doubly Linked List**:
    - Best when you need to traverse in both directions or delete elements in O(1) time with a reference.
    - More memory usage.

✅ Why Linked Lists Aren’t Common in Modern Languages (like Python)

Linked Lists are not built-in in most high-level languages like Python because:

- Dynamic arrays (like Python's list) are more efficient in most practical use 
cases, especially with CPU caches.
- Random access (indexing) is O(1) in lists but O(n) in linked lists — and that 
matters a lot in real-world programs.
- Linked lists use more memory (extra pointers), and pointer-chasing is slower due to poor cache locality.

Most high-level languages don't bother with native linked list implementations for everyday use.

When better performance is needed for certain patterns, specialized data structures are used, often backed by efficient C implementations.

🔧 **Enter collections.deque**

Python’s collections.deque (double-ended queue) is implemented in C, and it is a linked list–like structure, but actually closer to a doubly-linked list or a block-linked list (technically a "linked list of arrays" for cache efficiency).
deque is highly optimized for:

- Fast appends/pops from both ends → O(1)
- Much better for use cases like queues or stacks than list

collections.deque is written in C as part of Python's collections module, which is part of the standard library.
It’s the go-to when you need a real double-ended queue (like in BFS, sliding windows, etc.).

✅ **Time and Space Complexities of `collections.deque`**

| Operation | Time Complexity | Notes |
| --- | --- | --- |
| `append(x)` | **O(1)** | Add to right end |
| `appendleft(x)` | **O(1)** | Add to left end |
| `pop()` | **O(1)** | Remove from right end |
| `popleft()` | **O(1)** | Remove from left end |
| `extend(iterable)` | **O(k)** | k = len of iterable |
| `extendleft(iterable)` | **O(k)** | Adds items to the left one by one |
| `remove(value)` | **O(n)** | Linear search to find the value |
| `rotate(k)` | **O(k)** | Moves elements from one end to another |
| `index(x)` | **O(n)** | Linear scan |
| `insert(i, x)` | **O(n)** | No random access; requires traversal |
| Random access (e.g., `dq[i]`) | **O(n)** | Internally traverses nodes |
| `len(dq)` | **O(1)** | Size is tracked internally |

### 🚫 When *not* to use deque:

- You need **fast random access** (`dq[i]`) → use `list` instead.
- You insert/remove in the **middle** a lot → use `list` or `bisect` on a sorted list.

---

### ✅ When to use `deque`:

- You need **fast queue or stack behavior** from both ends.
- You are implementing sliding window algorithms.
- You need a rotating buffer.

### Reversing a Linked List

- Recursive
  
    🧠 Core Idea:

    Reversing a list recursively uses a two-step logic:

    1.  Go to the end (traverse):

     -   This is the recursive descent.

    2.  Reverse the links as we come back (unwind):

     -  This is where the actual reversal happens.
  
    
    ```
    def reverseList(head):    
    		if not head:        
    				return None        
    		
    		newHead = head    
    		if head.next:        
    				newHead = reverseList(head.next)        
    				head.next.next = head            
    				
    		head.next = None    
    		return newHead
    ```
    
- Iterative
    
    ```
    def reverseList(head):
        if not head:
                return         
        prev = None
        current = head
        while current:
                next_node = current.next  # Store the next node
                current.next = prev       # Reverse the link
                prev = current            # Move prev to current
                current = next_node       # Move to the next node
        return prev  # New head of the reversed list
    ```
