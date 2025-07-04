## Queues

### Implementations:

- Using python list (Naive)
    - ❌ `pop(0)` is **O(n)** — not efficient!
    
    ```python
    queue = []
    
    # Enqueue
    queue.append(10)
    
    # Dequeue
    val = queue.pop(0)
    ```
    
    - Fixed-size array implementation
    
    ```python
    Enqueue(queue, N, F, R, item) {
    	if R == N -1:
    		write overflow and exit
    	if F == -1:
    		Set F = 0 && R = 0
    	else:
    		R = R + 1
    	queue[R] = item
    }
    
    Dequeue(queue, N, F, R, item) {
    	if F == -1:
    		write underflow and exit
    	item = queue[F]
    	if F == R:
    		Set F = R = -1
    	else:
    		F = F + 1
    	return item
    ```
    
- Using `collections.deque` (Best practice)
    - ✅ Fast (`O(1)`) for both enqueue and dequeue
    
    ```python
    from collections import deque
    
    queue = deque()
    
    # Enqueue
    queue.append(10)
    
    # Dequeue
    val = queue.popleft()
    ```
    
- **Circular Queue (Fixed Capacity)**
    - Used in problems like **design circular queue**
    
    ```python
    class CircularQueue:
        def __init__(self, k):
            self.q = [None] * k
            self.head = self.tail = -1
            self.size = k
    
        def enqueue(self, val):
            if (self.tail + 1) % self.size == self.head:
                return False  # Full
            if self.head == -1:
                self.head = 0
            self.tail = (self.tail + 1) % self.size
            self.q[self.tail] = val
            return True
    
        def dequeue(self):
            if self.head == -1:
                return None  # Empty
            val = self.q[self.head]
            if self.head == self.tail:
                self.head = self.tail = -1  # Now empty
            else:
                self.head = (self.head + 1) % self.size
            return val
    ```
    
- Using Two Stacks
    - ✅ Common interview problem — **amortized O(1)** dequeue
    
    ```python
    class QueueUsingStacks:
        def __init__(self):
            self.in_stack = []
            self.out_stack = []
    
        def enqueue(self, val):
            self.in_stack.append(val)
    
        def dequeue(self):
            if not self.out_stack:
                while self.in_stack:
                    self.out_stack.append(self.in_stack.pop())
            return self.out_stack.pop() if self.out_stack else None
    ```
    
- Thread-safe Queue
    - Thread safe multithreaded programs
    
    ```python
    from queue import Queue
    
    q = Queue()
    
    q.put(1)
    q.put(2)
    
    q.get()   # Dequeue
    ```
    
- Double-ended Queue (Deque)
    - Used in **Sliding Window Maximum**, **Monotonic Queue** problems
    
    ```python
    dq = deque()
    
    dq.append(1)       # right end
    dq.appendleft(2)   # left end
    
    dq.pop()           # from right
    dq.popleft()       # from left
    ```
    

## Priority Queue

### 🔹 1. What is a Priority Queue?

A **Priority Queue (PQ)** is an abstract data type similar to a queue but **each element has a priority**. Elements with **higher priority are dequeued before lower-priority ones**.

- **Min-PQ**: smallest value has highest priority
- **Max-PQ**: largest value has highest priority

---

### 🔹 2. Python Implementations of Priority Queue

✅ A. Using `heapq` (Min Heap — most common)

```python
import heapq

pq = []
heapq.heappush(pq, 5)
heapq.heappush(pq, 2)
heapq.heappush(pq, 8)

print(heapq.heappop(pq))  # 2
```

### ✅ B. Max Heap using heapq (Negate values)

```
heapq.heappush(pq, -val)
heapq.heappop(pq) * -1
```

✅ C. With Custom Priority (Tuples)

```python
heapq.heappush(pq, (priority, item))  # Min by priority
```

### ✅ D. Using `queue.PriorityQueue` (thread-safe)

```
from queue import PriorityQueue
pq = PriorityQueue()
pq.put((1, "high"))
pq.put((5, "low"))
pq.get()  # (1, 'high')
```

- Slower (has thread locks)
- Good for multithreaded apps

### E. Using `SortedList` from `sortedcontainers` (alternative)

```
pip install sortedcontainers

from sortedcontainers import SortedList
sl = SortedList()
sl.add(5)
sl.add(3)
print(sl[0])  # 3 (smallest)
```

- Maintains a sorted list at all times
- Not a heap, but it gives you priority-based access

### **F. Custom Class with `__lt__` or custom comparator**

- Used in competitive programming or object-priority problems

```python
class Task:
    def __init__(self, priority, name):
        self.priority = priority
        self.name = name

    def __lt__(self, other):
        return self.priority < other.priority

heapq.heappush(pq, Task(2, "taskA"))
```

### 🔹 3. Common Patterns & Use Cases

| Pattern / Use Case | Description |
| --- | --- |
| **Top K Elements** | Track top K frequent/largest/smallest |
| **Dijkstra’s Algorithm** | Use min heap for shortest path |
| **K-way Merge** | Merge K sorted lists using PQ |
| **Median Maintenance** | Two heaps (min + max) |
| **Job Scheduling / Event Queue** | Pick highest priority job/task |
| **Greedy selection** | Always pick the smallest/largest |
