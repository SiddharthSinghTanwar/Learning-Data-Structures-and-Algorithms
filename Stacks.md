## Stacks

### 📘 Core Concepts:

- **LIFO** (Last In First Out)
- Operations: `push`, `pop`, `top`, `isEmpty`, `size`
- Time Complexity:
    - `push`, `pop`, `top`, `isEmpty` — **O(1)**

### 🛠️ Implementations:

- Using arrays or `list` (Python)
    - ✅ Easy and fast (O(1))
    - ❌ No fixed size or thread safety
    
    ```python
    stack = []
    
    # Push
    stack.append(10)
    
    # Pop
    val = stack.pop()
    
    # Peek
    top = stack[-1] if stack else None
    
    # Check empty
    is_empty = len(stack) == 0
    ```
    
- Custom class (like interview scenarios)
    
    ```python
    # with fixed capacity
    # ✅ Useful when memory or size is constrained (embedded systems, etc.)
    class FixedStack:
        def __init__(self, capacity):
            self.stack = [None] * capacity
            self.capacity = capacity
            self.top = -1
        
        def push(self, val):
            if self.top == self.capacity - 1:
                raise OverflowError("Stack is full")
            self.top += 1
            self.stack[self.top] = val
        
        def pop(self):
            if self.top == -1:
                return None
            val = self.stack[self.top]
            self.top -= 1
            return val
        
        def peek(self):
            return self.stack[self.top] if self.top != -1 else None
    
        def is_empty(self):
            return self.top == -1
    
    # with variable capacity
    class Stack:
        def __init__(self):
                self.items = []
        def push(self, item):
                self.items.append(item)
        def size(self):
                return len(self.items)            
        def peek(self):
                return None if self.size() == 0 else self.items[-1] 
        def pop(self):
                return None if self.size() == 0 else self.items.pop(-1)
      
     #------------------------------------------------------------------      
     # Using deque
    from collections import deque
    
    class Stack:
        def __init__(self):
            self.container = deque()
    
        def push(self, val):
            self.container.append(val)
    
        def pop(self):
            return self.container.pop()
    
        def peek(self):
            return self.container[-1]
    
        def is_empty(self):
            return len(self.container) == 0
    
        def size(self):
            return len(self.container)
    ```
    

- Using a linked list
    - ✅ Good for fixed-size control and interview questions
    - ❌ More code, less performant due to object overhead
    
    ```python
    class Node:
        def __init__(self, val):
            self.val = val
            self.next = None
    
    class StackLL:
        def __init__(self):
            self.top = None
        
        def push(self, val):
            new_node = Node(val)
            new_node.next = self.top
            self.top = new_node
        
        def pop(self):
            if not self.top:
                return None
            val = self.top.val
            self.top = self.top.next
            return val
        
        def peek(self):
            return self.top.val if self.top else None
    
        def is_empty(self):
            return self.top is None
    ```
    
- Using two queues
    - ✅ Interview-favorite trick
    - ❌ Less efficient: `push` is O(n)
    
    ```python
    from collections import deque
    
    class StackWithQueues:
        def __init__(self):
            self.q1 = deque()
            self.q2 = deque()
        
        def push(self, x):
            self.q2.append(x)
            while self.q1:
                self.q2.append(self.q1.popleft())
            self.q1, self.q2 = self.q2, self.q1
        
        def pop(self):
            return self.q1.popleft() if self.q1 else None
        
        def top(self):
            return self.q1[0] if self.q1 else None
    
        def is_empty(self):
            return not self.q1
    ```
    
- Using a single queue
    - ✅ Trickier implementation but uses less memory
    - ❌ Still not truly constant-time ops
    
    ```python
    from collections import deque
    
    class StackSingleQueue:
        def __init__(self):
            self.q = deque()
        
        def push(self, x):
            self.q.append(x)
            for _ in range(len(self.q) - 1):
                self.q.append(self.q.popleft())
        
        def pop(self):
            return self.q.popleft() if self.q else None
    
        def top(self):
            return self.q[0] if self.q else None
    
        def is_empty(self):
            return not self.q
    ```
    
- **Thread-Safe Stack using `queue.LifoQueue`**
    
    ### 🧵 `queue.LifoQueue` is a **thread-safe** version of a stack in Python.
    
    - It’s part of Python’s **`queue` module**, which is used in **multi-threaded programs**.
    - `LifoQueue` is a **drop-in thread-safe stack.**
        
        **Drop-in** means you can **replace your existing class with this new one without changing other code**.
        
        - `LifoQueue` from `queue` module is **drop-in**: acts like a regular stack (`put`, `get`) **but with thread safety built-in**.
        - So if your stack is accessed from multiple threads, just use `LifoQueue` instead of `list`.
        
        You don’t need to understand how threading works to use it — that’s the point of **drop-in replacement**.
        
    - It ensures only one thread can access the stack at a time, using **locks** internally.
    - ✅ Useful in multi-threaded environments
    - ❌ Slower due to locking overhead
    
    ```python
    # Using queue Module
    from queue import LifoQueue
    
    # Initializing a stack (optional)
    stack = LifoQueue(maxsize = 3)
    
    # qsize() show the number of elements
    # in the stack
    print(stack.qsize())
    
    # put() function to push
    # element in the stack
    stack.put('g')
    stack.put('f')
    stack.put('g')
    
    print("Full: ", stack.full())
    print("Size: ", stack.qsize())
    
    # get() function to pop
    # element from stack in
    # LIFO order
    print('\nElements popped from the stack')
    print(stack.get())
    print(stack.get())
    print(stack.get())
    
    print("\nEmpty: ", stack.empty())
    ```
    
- **Custom Stack with GetMin (Min Stack Pattern)**
    
    ### 📌 **Min Stack is a special stack** that supports:
    
    - `push(x)`
    - `pop()`
    - `top()`
    - `getMin()` → returns the **minimum element in the stack at all times**, in **O(1)** time.
    
    ```python
    class MinStack:
    	def init(self):
    	self.stack = []
    	self.min_stack = []
    	
    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
    
    def pop(self):
        val = self.stack.pop()
        if val == self.min_stack[-1]:
            self.min_stack.pop()
        return val
    
    def top(self):
        return self.stack[-1] if self.stack else None
    
    def get_min(self):
        return self.min_stack[-1] if self.min_stack else None
    
    ```
    
    ```python
    # the single-stack Min Stack using a mathematical trick 
    # to store the minimum without needing a second stack.
    # 💡 Concept:
    # We encode the previous minimum inside the stack when a new min is pushed.
    class MinStack:
        def __init__(self):
            self.stack = []
            self.min = None
    
        def push(self, val):
            if not self.stack:
                self.stack.append(val)
                self.min = val
            elif val >= self.min:
                self.stack.append(val)
            else:
                # Store encoded value
                self.stack.append(2 * val - self.min)
                self.min = val
    
        def pop(self):
            if not self.stack:
                return None
            top = self.stack.pop()
            if top < self.min:
                self.min = 2 * self.min - top
    
        def top(self):
            top = self.stack[-1]
            return self.min if top < self.min else top
    
        def get_min(self):
            return self.min
            
    ```
    

| Implementation | Space | Push | Pop | Notes |
| --- | --- | --- | --- | --- |
| Python list | O(n) | O(1) | O(1) | Fastest and easiest |
| Linked List | O(n) | O(1) | O(1) | Good for understanding internals |
| Two Queues | O(n) | O(n) | O(1) | Common interview trick |
| Single Queue | O(n) | O(n) | O(1) | Slightly optimized version |
| `LifoQueue` | O(n) | O(1) | O(1) | Thread-safe |
| Fixed Capacity | O(n) | O(1) | O(1) | Use when bounded size is needed |
| Min Stack | O(n) | O(1) | O(1) | Advanced use-case |
