# Arrays

## One-Dimensional Array

At its core, an array is a **contiguous block of memory** used to store a collection of elements of the **same data type**. This contiguity is the key to its efficiency.

Eg: Imagine a street with houses numbered sequentially. Each house is the same size and stores the same type of item. To find a specific item, you just need its house number and the starting point of the street.

**Internal Structure:**

- **Base Address:** An array stores the memory address of its first element, known as the "base address."
- **Element Size:** It also knows the fixed size (in bytes) of each element it stores. For example, an integer might take 4 bytes, a character 1 byte, etc.
- Number of elements in an array: (Upper bound - Lower bound) + 1
- Size of Array: If each element takes more than 1 byte
    - Number of elements * size of each element in bytes.
- Address of the element at the kth index:
    - a[k] = Base address + Width * (k - lower bound)
    - This is a direct calculation, requiring only a few arithmetic operations, making access extremely fast.

### Fixed-Size vs. Dynamic-Size Arrays

The primary distinction lies in their ability to change size after creation.

### Fixed-Size Arrays

- **Definition:** The size of a fixed-size array is determined at the time of its creation and **cannot be changed** during runtime.
- **Internal Structure:** As discussed, they occupy a pre-allocated, contiguous block of memory.
- **Indexing:** Direct calculation using the base address and element size.
- **Advantages:**
    - **Predictable Memory Usage:** You know exactly how much memory will be consumed.
    - **Extremely Efficient Access:** Constant time O(1) for element access.
    - **No Overhead for Resizing:** Since they don't resize, there's no performance penalty associated with it.
- **Disadvantages:**
    - **Inflexibility:** If you underestimate the required size, you might run out of space. If you overestimate, you waste memory.
    - **No Easy Growth:** Adding more elements than the allocated size requires creating an entirely new, larger array and copying all elements, which is an expensive operation.
- **Examples:** C-style arrays (`int arr[10];`), `std::array` in C++.

### Dynamic-Size Arrays (or Resizable Arrays, ArrayLists, Vectors)

- **Definition:** These arrays can grow or shrink in size during runtime as elements are added or removed.
- **Internal Structure:** Internally, they typically use a **fixed-size array** as their underlying data structure. When the underlying array becomes full, a new, larger array is allocated, and the elements are copied over. This abstraction gives the user the illusion of a continuously resizable array.
- **Indexing:** Similar to fixed-size arrays, indexing within the currently allocated contiguous block is O(1).
- **Advantages:**
    - **Flexibility:** Adapts to varying data storage needs, no need to know the exact size beforehand.
    - **Memory Efficiency (potentially):** Can avoid excessive memory allocation if data size fluctuates.
- **Disadvantages:**
    - **Resizing Overhead:** The act of resizing (allocating new memory and copying elements) can be an expensive operation, potentially impacting performance.
    - **Less Predictable Memory Usage:** Memory usage can grow dynamically.
- **Examples:** Python lists, Java `ArrayList`, C++ `std::vector`.

### Time and Space Complexities

### Fixed-Size Arrays

- **Time Complexities:**
    - **Access (Read/Write) an element:** *O(1*) (Constant time) - Direct calculation.
    - **Insertion/Deletion (at end):** *O(1)* (if space available) - If not, requires a new array and copying.
    - **Insertion/Deletion (at beginning/middle):** *O(N)* (Linear time) - Requires shifting subsequent elements.
    - **Search (Unsorted):** *O(N)* (Linear time) - Must check every element in the worst case.
    - **Search (Sorted - Binary Search):** *O(logN) (*Logarithmic time)
- **Space Complexity:** *O(N)* (Linear space) - Where N is the fixed size of the array.

### Dynamic-Size Arrays

- **Time Complexities (Amortized):**
    - **Access (Read/Write) an element:** O(1) (Constant time) - Still direct indexing.
    - **Insertion (at end - `append`, `add`):**
        - Typically O(1) **amortized**.
        - Worst-case O(N) when a resize occurs (copying all N elements).
    - **Insertion/Deletion (at beginning/middle):** O(N) (Linear time) - Requires shifting subsequent elements.
    - **Search (Unsorted):** O(N) (Linear time).
    - **Search (Sorted - Binary Search):** O(logN) (Logarithmic time).
- **Space Complexity:** O(N) (Linear space) - Where N is the current number of elements. The underlying allocated capacity might be larger than N.

### Resizing Logic in Dynamic Arrays

The key to efficient dynamic arrays lies in their resizing strategy. Rather than growing by just one element when full, they typically grow by a larger factor to minimize the frequency of expensive copy operations.

**How Python Lists, Java ArrayLists, and C++ Vectors Grow:**

These implementations generally follow a strategy of **doubling their capacity** (or a similar growth factor).

1. **When full:** When you try to add an element to a dynamic array that has reached its current capacity:
    - A new, larger contiguous block of memory is allocated.
    - The size of this new block is typically double the current capacity (or 1.5× in some implementations).
    - All existing elements are copied from the old memory location to the new one.
    - The old memory block is deallocated.
    - The new element is then added.
2. **Why doubling?** Doubling the capacity ensures that even though individual resize operations are O(N), the **amortized time complexity** for insertions at the end remains O(1).
- **Python Lists:** Python lists typically over-allocate, meaning they reserve more memory than immediately needed. When the list runs out of space, it usually **doubles its capacity**. This strategy helps in amortizing the cost of insertions.
    -  🧠 But… Python Lists Aren’t Always the Best for Everything
      
    Python’s list:
  
    - Backed by a resizable C array under the hood.
    - Fast for append() and pop() at the end — amortized O(1).
    - But slow for inserting/deleting from the front or middle — O(n) due to shifting.     
- **Java ArrayLists:** Java `ArrayList` also uses a similar strategy. When an `add` operation causes the `ArrayList` to exceed its current capacity, a new, larger array (typically 1.5× the current size, but can vary) is allocated, and elements are copied.
- **C++ `std::vector`:** `std::vector` also grows dynamically. While the exact growth factor is implementation-defined, it's very common for it to be a doubling strategy. When `push_back` or `insert` operations would exceed the current capacity, `std::vector` allocates a new, larger contiguous block of memory, copies existing elements, and then adds the new element. You can also manually reserve capacity using `reserve()`.

**Variations and Other Ideas:**

While doubling is common, other strategies exist:

- **Fixed Increment:** Growing by a fixed number of slots. This leads to frequent reallocations if the array grows large.
- **Linear Growth:** Growing by a constant factor. This is generally less efficient than exponential growth.
- **Application-Specific Growth:** For very specific use cases, a custom growth strategy might be employed based on anticipated usage patterns.

### Amortized Time Complexity Explanation

Amortized time complexity is a way to analyze the average performance of an algorithm over a sequence of operations, rather than focusing on the worst-case time of a single operation.

**The Concept:**

Imagine a dynamic array. Most `append` operations are O(1) because there's enough space. However, occasionally, an `append` triggers a resize, which costs O(N) because all N elements must be copied.

If we just look at the worst-case, we'd say `append` is O(N). But that's misleading because O(N) operations are rare. Amortized analysis distributes the cost of these expensive operations over a sequence of cheap operations.

**How it applies to Dynamic Arrays:**

Consider a sequence of $N$ `append` operations to an initially empty dynamic array that doubles its capacity when full.

- The first few appends are O(1).
- When the array needs to resize (e.g., from capacity 1 to 2, then 2 to 4, then 4 to 8, and so on), an O(N) copy operation occurs.
- However, after a resize, you have many more O(1) appends before the next resize.

The total cost of N insertions is approximately N+N/2+N/4+N/8+⋯+1=2N.

Since the total cost for N operations is O(N), the average cost per operation is O(N)/N=O(1).

**Think of it like this:** You pay a little extra (the cost of copying) every once in a while, but that extra payment "pays for" many subsequent cheap operations. When you average out the cost over a long sequence, the expensive operations are so infrequent that their cost is effectively spread out, making the average cost per operation constant.

This is why dynamic arrays, despite their occasional O(N) worst-case resize, are highly efficient for adding elements at the end, providing an **amortized O(1) time complexity** for such operations.

### Implementation:

- Fixed-size array implementation: We'll simulate a fixed-size array using a Python list of a predefined size, initially filled with `None` or a placeholder.
    
    ```python
    class FixedSizeArray:
        def __init__(self, capacity):
            if capacity <= 0:
                raise ValueError("Capacity must be positive.")
            self.capacity = capacity
            self.array = [None] * capacity
            self.size = 0  # Number of actual elements in the array
    
        def __str__(self):
            return f"FixedSizeArray(capacity={self.capacity}, size={self.size}, data={self.array[:self.size]})"
    
        def __len__(self):
            return self.size
    
        def is_empty(self):
            return self.size == 0
    
        def is_full(self):
            return self.size == self.capacity
    
        def get_at(self, index):
            if not (0 <= index < self.size):
                raise IndexError("Index out of bounds.")
            return self.array[index]
    
        def set_at(self, index, value):
            if not (0 <= index < self.size):
                raise IndexError("Index out of bounds.")
            self.array[index] = value
    
        def insert(self, index, value):
            if self.is_full():
                raise Exception("Array is full, cannot insert.")
            if not (0 <= index <= self.size): # Can insert at size to append
                raise IndexError("Index out of bounds.")
    
            # Shift elements to the right
            for i in range(self.size, index, -1):
                self.array[i] = self.array[i-1]
            self.array[index] = value
            self.size += 1
            print(f"  Inserted {value} at index {index}. Current array: {self.array[:self.size]}")
    
        def delete(self, index):
            if self.is_empty():
                raise Exception("Array is empty, cannot delete.")
            if not (0 <= index < self.size):
                raise IndexError("Index out of bounds.")
    
            deleted_value = self.array[index]
            # Shift elements to the left
            for i in range(index, self.size - 1):
                self.array[i] = self.array[i+1]
            self.array[self.size - 1] = None # Clear the last element
            self.size -= 1
            print(f"  Deleted {deleted_value} from index {index}. Current array: {self.array[:self.size]}")
            return deleted_value
    
    # --- Usage Example: Fixed-Size Array ---
    print("--- Fixed-Size Array Simulation ---")
    fixed_array = FixedSizeArray(5)
    print(fixed_array)
    
    fixed_array.insert(0, 10) # Insert at beginning
    fixed_array.insert(1, 20) # Insert in middle
    fixed_array.insert(2, 30)
    fixed_array.insert(3, 40)
    fixed_array.insert(4, 50) # Insert at end
    print(fixed_array)
    
    # Try inserting into a full array
    try:
        fixed_array.insert(0, 60)
    except Exception as e:
        print(f"  Error: {e}")
    
    print(f"Element at index 2: {fixed_array.get_at(2)}")
    fixed_array.set_at(2, 35)
    print(f"Element at index 2 after set: {fixed_array.get_at(2)}")
    print(fixed_array)
    
    fixed_array.delete(1) # Delete from middle
    fixed_array.delete(0) # Delete from beginning
    print(fixed_array)
    
    # Try deleting from empty array
    fixed_array.delete(0)
    fixed_array.delete(0)
    fixed_array.delete(0)
    try:
        fixed_array.delete(0)
    except Exception as e:
        print(f"  Error: {e}")
    print("-" * 40)
    ```
    
- Explanation:
    - **`__init__`**: Initializes the array with a fixed `capacity` using a Python list filled with `None`. `self.size` tracks the actual number of elements present.
    - **`insert(index, value)`**:
        - Checks if the array is full.
        - Checks for valid `index`.
        - The crucial part is the `for` loop that shifts elements to the right starting from `self.size` down to `index`. This makes insertion O(N) because, in the worst case (inserting at the beginning), all existing elements need to be shifted.
    - **`delete(index)`**:
        - Checks if the array is empty.
        - Checks for valid `index`.
        - The `for` loop shifts elements to the left, overwriting the deleted element. This is also an O(N) operation for the same reason as insertion.
    - **`get_at` / `set_at`**: These are O(1) operations as they directly access the element at the given `index` without shifting.
- Dynamic-size array implementation: This implementation will demonstrate the `append`, `pop`, `insert`, and `resize` logic, along with tracking copies for visualization.
    
    ```python
    class DynamicArray:
        def __init__(self, initial_capacity=1):
            if initial_capacity <= 0:
                raise ValueError("Initial capacity must be positive.")
            self.capacity = initial_capacity
            self.array = [None] * self.capacity
            self.size = 0  # Number of actual elements
            self.total_copies_due_to_resize = 0
            self.resize_count = 0
    
        def __str__(self):
            return (f"DynamicArray(size={self.size}, capacity={self.capacity}, "
                    f"data={self.array[:self.size]}, copies_due_to_resize={self.total_copies_due_to_resize})")
    
        def __len__(self):
            return self.size
    
        def __getitem__(self, index):
            if not (0 <= index < self.size):
                raise IndexError("Index out of bounds.")
            return self.array[index]
    
        def __setitem__(self, index, value):
            if not (0 <= index < self.size):
                raise IndexError("Index out of bounds.")
            self.array[index] = value
    
        def _resize(self):
            print(f"\n--- Resizing: Current capacity {self.capacity}, size {self.size} ---")
            self.resize_count += 1
            new_capacity = self.capacity * 2 # Doubling strategy
            # In some languages like Java/C++, this involves explicit memory allocation.
            # In Python, creating a new list effectively allocates new memory.
            new_array = [None] * new_capacity
    
            print(f"  Allocating new array with capacity: {new_capacity}")
            print("  Copying elements...")
            for i in range(self.size):
                new_array[i] = self.array[i]
                self.total_copies_due_to_resize += 1 # Track copies
            print(f"  Copied {self.size} elements.")
            print(f"  Old array (memory address simulation): {id(self.array)}")
            self.array = new_array # Point to the new array
            print(f"  New array (memory address simulation): {id(self.array)}")
            self.capacity = new_capacity
            print(f"--- Resizing Complete. New capacity: {self.capacity} ---\n")
    
        def append(self, value):
            if self.size == self.capacity:
                self._resize()
            self.array[self.size] = value
            self.size += 1
            # print(f"  Appended {value}. Current array: {self.array[:self.size]}")
    
        def pop(self):
            if self.size == 0:
                raise IndexError("Pop from empty array.")
            self.size -= 1
            popped_value = self.array[self.size]
            self.array[self.size] = None # Clear reference
            # Optional: Shrink if capacity is much larger than size
            # if self.size < self.capacity // 4 and self.capacity > self.initial_capacity:
            #     self._shrink() # Not implemented for simplicity, but a common optimization
            # print(f"  Popped {popped_value}. Current array: {self.array[:self.size]}")
            return popped_value
    
        def insert(self, index, value):
            if not (0 <= index <= self.size):
                raise IndexError("Index out of bounds.")
            if self.size == self.capacity:
                self._resize()
    
            # Shift elements to the right
            for i in range(self.size, index, -1):
                self.array[i] = self.array[i-1]
            self.array[index] = value
            self.size += 1
            # print(f"  Inserted {value} at index {index}. Current array: {self.array[:self.size]}")
    
    # --- Usage Example: Dynamic Array ---
    print("--- Dynamic Array (Custom List/Vector) Simulation ---")
    dyn_array = DynamicArray(initial_capacity=2)
    print(dyn_array)
    
    print("\n--- Appending elements and observing resizing ---")
    elements_to_append = 10
    for i in range(1, elements_to_append + 1):
        print(f"Appending {i}...")
        dyn_array.append(i * 10)
        print(dyn_array)
    
    print(f"\nTotal copies due to resizing: {dyn_array.total_copies_due_to_resize}")
    print(f"Total resize operations: {dyn_array.resize_count}")
    print("-" * 40)
    
    print("\n--- Appending 1000 elements and tracking resizes ---")
    dyn_array_1000 = DynamicArray(initial_capacity=1)
    num_elements_to_add = 1000
    for i in range(1, num_elements_to_add + 1):
        dyn_array_1000.append(i)
    
    print(f"\nAfter appending {num_elements_to_add} elements:")
    print(f"  Total copies due to resizing: {dyn_array_1000.total_copies_due_to_resize}")
    print(f"  Total resize operations: {dyn_array_1000.resize_count}")
    print(f"  Final capacity: {dyn_array_1000.capacity}")
    print("-" * 40)
    
    print("\n--- Testing insert and pop ---")
    test_array = DynamicArray(initial_capacity=3)
    test_array.append(10)
    test_array.append(20)
    test_array.append(30)
    print(test_array)
    
    test_array.insert(1, 15) # Insert in middle (will trigger resize)
    print(test_array)
    
    test_array.pop() # Pop from end
    print(test_array)
    
    test_array.insert(0, 5) # Insert at beginning
    print(test_array)
    
    print(f"Element at index 2: {test_array[2]}")
    test_array[2] = 25
    print(f"Element at index 2 after set: {test_array[2]}")
    print(test_array)
    print("-" * 40)
    ```
    
- Explanation:
    - **`__init__`**: Starts with an `initial_capacity`. `self.total_copies_due_to_resize` and `self.resize_count` are added to track the performance of resizing.
    - **`_resize()`**:
        - This is the core of the dynamic array. When called, it prints messages to visualize the resizing process.
        - `new_capacity = self.capacity * 2`: Implements the **doubling strategy**.
        - `new_array = [None] * new_capacity`: A new, larger Python list (simulating a new memory block) is created.
        - The `for` loop copies all `self.size` existing elements from the `self.array` to `new_array`. Each copy increments `self.total_copies_due_to_resize`.
        - `id(self.array)`: In Python, `id()` returns the "identity" of an object, which can be thought of as its memory address. Printing this before and after `self.array = new_array` shows that the dynamic array is now pointing to a *different* memory location (a new array).
        - `self.array = new_array`: This is the crucial step where the dynamic array "switches" to using the new, larger memory block.
    - **`append(value)`**:
        - Checks if `self.size == self.capacity`. If true, it calls `_resize()` to expand the underlying array.
        - Then, it adds the `value` at the current `self.size` position and increments `self.size`.
        - **Amortized O(1)**: Most appends are O(1). The O(N) cost of `_resize` is rare and spread out over many O(1) operations, resulting in an amortized O(1) average.
    - **`pop()`**: Removes the last element. An optional `_shrink` method could be added to reduce capacity if the array becomes very sparse, but it's omitted here for simplicity.
    - **`insert(index, value)`**: Similar to the fixed-size array, it shifts elements. If `self.size == self.capacity`, it also triggers a `_resize()` *before* shifting. This makes `insert` an O(N) operation even with amortized resizing.
- **Resizing Mechanism & Performance Visualization:**
    
    When you run the dynamic array code, you'll see:
    
    - **Initial Appends:** No resizing.
    - **When capacity is reached:** Detailed print statements show the old capacity, new capacity (doubled), and the copying process. You'll see the `total_copies_due_to_resize` increasing.
    - **1000 Appends Log:** Notice how `resize_count` is significantly less than 1000. Each resize doubles the capacity, so fewer resizes are needed for a large number of appends. The `total_copies_due_to_resize` will be roughly 2N (for N=1000, about 2000 copies), which confirms the amortized O(1) cost per append.

### Space-Time tradeoffs Example:

These examples illustrate how different algorithmic approaches can exchange space for time efficiency or vice-versa.

- Fibonacci Sequence
    
    **Definition:** F(0)=0, F(1)=1, F(n)=F(n−1)+F(n−2) for n>1. 
    
    ```python
    import functools
    import time
    
    print("\n--- Fibonacci Sequence Implementations (Space-Time Tradeoffs) ---")
    
    # 1. Plain Recursion (Exponential Time: O(2^n), Space: O(n) for call stack)
    # Very inefficient due to redundant calculations.
    def fib_recursive_plain(n):
        if n <= 1:
            return n
        return fib_recursive_plain(n-1) + fib_recursive_plain(n-2)
    
    # 2. Recursion with Memoization (Top-Down Dynamic Programming)
    # Time: O(n), Space: O(n) for memoization table + O(n) for call stack
    @functools.lru_cache(None) # Python's built-in memoization decorator
    def fib_recursive_memoized(n):
        if n <= 1:
            return n
        return fib_recursive_memoized(n-1) + fib_recursive_memoized(n-2)
    
    # Manual memoization for clarity (without lru_cache)
    memo = {}
    def fib_recursive_memoized_manual(n):
        if n in memo:
            return memo[n]
        if n <= 1:
            return n
        result = fib_recursive_memoized_manual(n-1) + fib_recursive_memoized_manual(n-2)
        memo[n] = result
        return result
    
    # 3. Iterative (Bottom-Up Dynamic Programming)
    # Time: O(n), Space: O(n) for storing all previous results (or O(1) if optimized)
    def fib_iterative(n):
        if n <= 1:
            return n
        dp = [0] * (n + 1)
        dp[0] = 0
        dp[1] = 1
        for i in range(2, n + 1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
    
    # 4. Space-Optimized Iterative
    # Time: O(n), Space: O(1)
    def fib_space_optimized(n):
        if n <= 1:
            return n
        a, b = 0, 1
        for _ in range(2, n + 1):
            a, b = b, a + b
        return b
    
    # --- Test Fibonacci ---
    n_fib = 35 # Higher for plain recursion to show slowness, but not too high
    print(f"\nCalculating Fibonacci({n_fib}):")
    
    start_time = time.perf_counter()
    # result_plain = fib_recursive_plain(n_fib) # Uncomment to run, will be very slow for N=35
    # end_time = time.perf_counter()
    # print(f"  Plain Recursion: {result_plain} (Time: {end_time - start_time:.6f} seconds)")
    
    start_time = time.perf_counter()
    result_memoized = fib_recursive_memoized(n_fib)
    end_time = time.perf_counter()
    print(f"  Recursion + Memoization: {result_memoized} (Time: {end_time - start_time:.6f} seconds)")
    fib_recursive_memoized.cache_clear() # Clear cache for re-runs
    
    # Manual memoization test
    memo = {} # Reset memo
    start_time = time.perf_counter()
    result_memoized_manual = fib_recursive_memoized_manual(n_fib)
    end_time = time.perf_counter()
    print(f"  Manual Memoization: {result_memoized_manual} (Time: {end_time - start_time:.6f} seconds)")
    
    start_time = time.perf_counter()
    result_iterative = fib_iterative(n_fib)
    end_time = time.perf_counter()
    print(f"  Iterative (O(n) space): {result_iterative} (Time: {end_time - start_time:.6f} seconds)")
    
    start_time = time.perf_counter()
    result_space_optimized = fib_space_optimized(n_fib)
    end_time = time.perf_counter()
    print(f"  Space-Optimized (O(1) space): {result_space_optimized} (Time: {end_time - start_time:.6f} seconds)")
    print("-" * 40)
    ```
    
    - **Explanation for Fibonacci Variants:**
        - **Plain Recursion (O(2N) time, O(N) space):**
            - This is the most straightforward implementation directly from the definition.
            - **Problem:** It recalculates the same Fibonacci numbers many times (e.g., `fib(3)` is calculated multiple times when computing `fib(5)`). This leads to exponential time complexity.
            - **Space:** The recursion depth leads to O(N) space usage on the call stack.
        - **Recursion + Memoization (O(N) time, O(N) space):**
            - **Idea:** Store the results of expensive function calls and return the cached result when the same inputs occur again. This is "top-down" dynamic programming.
            - **Time:** Each `fib(i)` is calculated only once. Subsequent calls retrieve the value from `memo`. This brings the time complexity down to O(N).
            - **Space:** We use O(N) space for the `memo` dictionary/array to store results, plus O(N) for the recursion call stack.
        - **Iterative (O(N) time, O(N) space):**
            - **Idea:** Build up the solution from the bottom, storing all previous results in a `dp` array. This is "bottom-up" dynamic programming.
            - **Time:** A simple loop runs N times, performing constant work in each iteration. O(N).
            - **Space:** Uses an array of size N+1 to store all intermediate Fibonacci numbers, hence O(N) space.
        - **Space-Optimized Iterative (O(N) time, O(1) space):**
            - **Idea:** Observe that to calculate `fib(n)`, you only need `fib(n-1)` and `fib(n-2)`. You don't need the entire history. So, we can just keep track of the last two Fibonacci numbers.
            - **Time:** Still O(N) because the loop runs N times.
            - **Space:** Uses only two variables (`a` and `b`), making it O(1) space.
            - **Tradeoff:** This is the most efficient in terms of space, achieving O(1) by sacrificing the ability to easily access any previous Fibonacci number (which the O(N) iterative version can do).
- Factorial Variants
    - **Definition:** n!=n×(n−1)×⋯×1. 0!=1.
    
    ```python
    print("\n--- Factorial Variants ---")
    
    # 1. Iterative Factorial (Time: O(n), Space: O(1))
    def factorial_iterative(n):
        if n < 0:
            raise ValueError("Factorial not defined for negative numbers.")
        if n == 0:
            return 1
        result = 1
        for i in range(1, n + 1):
            result *= i
        return result
    
    # 2. Recursive Factorial (Time: O(n), Space: O(n) for call stack)
    def factorial_recursive(n):
        if n < 0:
            raise ValueError("Factorial not defined for negative numbers.")
        if n == 0:
            return 1
        return n * factorial_recursive(n-1)
    
    # 3. Tail-Recursive Factorial (Concept - Python doesn't optimize tail calls)
    # This pattern is important for languages like Scheme, Scala, etc.
    # In Python, it still builds up the call stack, so its space complexity is O(N).
    def factorial_tail_recursive(n, accumulator=1):
        if n < 0:
            raise ValueError("Factorial not defined for negative numbers.")
        if n == 0:
            return accumulator
        return factorial_tail_recursive(n - 1, accumulator * n)
    
    # --- Test Factorial ---
    n_fact = 10
    
    print(f"\nCalculating Factorial({n_fact}):")
    
    start_time = time.perf_counter()
    result_iterative = factorial_iterative(n_fact)
    end_time = time.perf_counter()
    print(f"  Iterative: {result_iterative} (Time: {end_time - start_time:.6f} seconds)")
    
    start_time = time.perf_counter()
    result_recursive = factorial_recursive(n_fact)
    end_time = time.perf_counter()
    print(f"  Recursive: {result_recursive} (Time: {end_time - start_time:.6f} seconds)")
    
    start_time = time.perf_counter()
    result_tail_recursive = factorial_tail_recursive(n_fact)
    end_time = time.perf_counter()
    print(f"  Tail Recursive (Python): {result_tail_recursive} (Time: {end_time - start_time:.6f} seconds)")
    
    print("-" * 40)
    ```
    
    - **Explanation for Factorial Variants:**
        - **Iterative Factorial (O(N) time, O(1) space):**
            - Uses a simple loop to multiply numbers.
            - No extra data structures are created, and no recursion stack is used, making it O(1) space.
        - **Recursive Factorial (O(N) time, O(N) space):**
            - Defines factorial in terms of itself.
            - Each recursive call adds a new frame to the call stack. For `n!`, there will be N active frames, leading to O(N) space.
        - **Tail-Recursive Factorial (Conceptual O(N) time, O(N) space in Python; O(1) in optimized languages):**
            - **Tail Recursion:** A recursive call is "tail recursive" if it's the very last operation performed in the function before returning. This means the current function's stack frame is no longer needed after the recursive call, so the compiler/interpreter can optimize it away, effectively turning recursion into iteration.
            - **Python's Behavior:** Python **does not** perform tail-call optimization. Even though `factorial_tail_recursive` is tail-recursive in *form*, Python's interpreter still creates a new stack frame for each call, leading to O(N) space complexity.
            - **Significance:** In languages that *do* optimize tail calls (e.g., Scheme, Haskell, some C++ compilers with specific flags), `factorial_tail_recursive` would achieve O(1) space, essentially behaving like the iterative version in terms of memory. This is a classic space-time tradeoff example where the language's runtime environment plays a crucial role.
- **Explanation of the exponential time-complexity of Fibonacci plain recursion series:**
    
    Let's break down why plain recursive Fibonacci is O(2^n) using a **recursion tree**.
    
    The Fibonacci sequence is defined as:
    F(0)=0F(1)=1F(n)=F(n−1)+F(n−2) for n>1 
    
    ```python
                      fib(5)
                     /      \
                 fib(4)      fib(3)
                /    \      /    \
            fib(3)  fib(2)  fib(2)  fib(1)
           /    \    /  \    /  \
       fib(2) fib(1) fib(1) fib(0) fib(1) fib(0)
      /  \
    fib(1) fib(0)
    ```
    
    **Analyzing the Redundant Calculations:**
    
    Look at this tree:
    
    - To calculate `fib(5)`, we need `fib(4)` and `fib(3)`.
    - To calculate `fib(4)`, we need `fib(3)` and `fib(2)`.
    - Notice that `fib(3)` is calculated **twice** (once for `fib(5)` and once for `fib(4)`).
    - `fib(2)` is calculated **three times**.
    - `fib(1)` and `fib(0)` (the base cases) are calculated many, many times.
    
    **The Exponential Growth:**
    
    The key insight is that **each call to `fib(n)` generates (roughly) two more recursive calls.** This branching behavior leads to an exponential increase in the number of function calls as `n` grows.
    
    - `fib(n)` calls `fib(n-1)` and `fib(n-2)`.
    - `fib(n-1)` calls `fib(n-2)` and `fib(n-3)`.
    - `fib(n-2)` calls `fib(n-3)` and `fib(n-4)`.
    
    This creates a call tree where, at each level, the number of operations roughly doubles.
    
    **A Simpler Lower Bound:**
    
    Consider just one side of the recursion tree, say the calls to `fib(n-1)`, then `fib(n-2)`, then `fib(n-3)`, and so on, down to `fib(0)` or `fib(1)`. This path has a length of approximately N.
    
    Now, consider the other side, `fib(n-2)`. This also creates a long chain of calls.
    
    Since `fib(n)` makes at least two calls (`fib(n-1)` and `fib(n-2)`), and each of those makes at least two calls (until they hit the base cases), the number of nodes in the recursion tree grows very rapidly.
    
    **More Formal Reasoning (Upper Bound):**
    
    Let T(n) be the number of operations to compute F(n).
    We can write a recurrence relation for T(n):
    T(n)=T(n−1)+T(n−2)+C (where C is a constant for the addition and function call overhead)
    T(0)=C0T(1)=C1
    
    This recurrence relation is very similar to the Fibonacci sequence itself.
    A common way to get an upper bound for such a recurrence is to say that T(n−1) is larger than T(n−2). So, we can approximate:
    T(n)>T(n−2)+T(n−2)=2×T(n−2)
    
    If we repeatedly apply this approximation:
    T(n)>2×T(n−2)T(n)>2×(2×T(n−4))=22×T(n−4)T(n)>2k×T(n−2k)
    
    If n−2k=0 (or 1), then k=n/2.
    So, T(n)>2n/2×T(0 or 1), which means T(n) is at least O(2
    
    [](data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="400em" height="1.08em" viewBox="0 0 400000 1080" preserveAspectRatio="xMinYMin slice"><path d="M95,702
    c-2.7,0,-7.17,-2.7,-13.5,-8c-5.8,-5.3,-9.5,-10,-9.5,-14
    c0,-2,0.3,-3.3,1,-4c1.3,-2.7,23.83,-20.7,67.5,-54
    c44.2,-33.3,65.8,-50.3,66.5,-51c1.3,-1.3,3,-2,5,-2c4.7,0,8.7,3.3,12,10
    s173,378,173,378c0.7,0,35.3,-71,104,-213c68.7,-142,137.5,-285,206.5,-429
    c69,-144,104.5,-217.7,106.5,-221
    l0 -0
    c5.3,-9.3,12,-14,20,-14
    H400000v40H845.2724
    s-225.272,467,-225.272,467s-235,486,-235,486c-2.7,4.7,-9,7,-19,7
    c-6,0,-10,-1,-12,-3s-194,-422,-194,-422s-65,47,-65,47z
    M834 80h400000v40h-400000z"></path></svg>)
    
    n) which is still exponential.A more precise analysis using the characteristic equation shows that the runtime is proportional to the n-th Fibonacci number itself, which is approximately ϕn/5
    
    [](data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="400em" height="1.08em" viewBox="0 0 400000 1080" preserveAspectRatio="xMinYMin slice"><path d="M95,702
    c-2.7,0,-7.17,-2.7,-13.5,-8c-5.8,-5.3,-9.5,-10,-9.5,-14
    c0,-2,0.3,-3.3,1,-4c1.3,-2.7,23.83,-20.7,67.5,-54
    c44.2,-33.3,65.8,-50.3,66.5,-51c1.3,-1.3,3,-2,5,-2c4.7,0,8.7,3.3,12,10
    s173,378,173,378c0.7,0,35.3,-71,104,-213c68.7,-142,137.5,-285,206.5,-429
    c69,-144,104.5,-217.7,106.5,-221
    l0 -0
    c5.3,-9.3,12,-14,20,-14
    H400000v40H845.2724
    s-225.272,467,-225.272,467s-235,486,-235,486c-2.7,4.7,-9,7,-19,7
    c-6,0,-10,-1,-12,-3s-194,-422,-194,-422s-65,47,-65,47z
    M834 80h400000v40h-400000z"></path></svg>)
    
    , where ϕ≈1.618 (the golden ratio). Since ϕ>1, this is indeed exponential.
    
    So, while it's not exactly 2n operations (because one branch is always slightly shorter than the other), it is bounded by 2n and grows exponentially. The constant base of the exponent is ≈1.618. When we talk about Big O, we often simplify to the most straightforward exponential base, so O(2n) is a commonly used (and correct) upper bound.
    

### Gotcha’s

- Big O notation describes how the *runtime scales with the input size (`n`)*.
- **Deletion from the Middle:** If you "delete" an element from the middle of a fixed-size array, you typically mean that the element at that position is no longer considered part of the valid data. To maintain the contiguity of the "valid" elements and avoid gaps, you usually have two options:
    1. **Shift elements:** You would shift all subsequent elements one position to the left to fill the gap created by the deletion. This operation takes O(n) time in the worst case (deleting the first element), where n is the number of elements after the deletion point.
    2. **Mark as "deleted":** You could set the value at that position to a special "null" or "deleted" marker. While this is O(1), it means your array will have "holes," and subsequent operations (like iteration or searching) might need to skip these marked elements, or you might need a separate mechanism to track valid elements. This isn't a true deletion in the sense of removing the element and shrinking the conceptual "list" of valid data.
    - **Insertion into the Middle:** To insert an element into the middle of a fixed-size array, you must first make space for it. This involves shifting all subsequent elements one position to the right. This operation also takes O(n) time in the worst case (inserting at the beginning). If the array is already full, you cannot insert without resizing (which is typically not an option for a strictly fixed-size array or involves creating a new, larger array and copying, which is O(n)).
- **The problem with insertion/deletion in the middle of a "list" within an array:**
    
    The issue of shifting elements arises when you are using an array to represent a **sequential list of data where order matters and gaps are not allowed (or undesirable)**.
    
- **Key takeaway:** The shifting happens not because the *memory* isn't contiguous, but because you are trying to maintain a **contiguous block of *valid data*** within that fixed-size memory block. If your application logic allows for "holes" (like in a hash table or a sparse array), then you don't need to shift. But if you're managing a compact list of items, you do.

## Two-Dimensional Array

In computing, row-major order and column-major order are methods for storing a multidimensional array in linear storage.
    
    ```c
    int disp[2][4] = {
    	{1,2,3,4},
    	{5,6,7,8}
    };
    // or
    // in row-major implementation
    int disp[2][4] = {1,2,3,4,5,6,7,8};
    ```
    
- Address of a[ i ][ j ] = B + W * [ (Number of columns) * (Number of rows before us) + (Number of elements before us) ]

## 3-Dimensional Array or N-Dimensional Array

- Element address formula:
  ```
    N-Dimensional array
    A(([L₁] --- [U₁]), ([L₂] --- [U₂]), ([L₃] --- [U₃]), ([L₄] --- [U₄]) -------- ([Lₙ] --- [Uₙ]))

    Location of A [i, j, k, ----, x] =
        B + (i - L₁)(U₂ - L₂ + 1)(U₃ - L₃ + 1)(U₄ - L₄ + 1) ---- (Uₙ - Lₙ + 1)
        + (j - L₂)(U₃ - L₃ + 1)(U₄ - L₄ + 1) ---- (Uₙ - Lₙ + 1)
        + (k - L₃)(U₄ - L₄ + 1) ---- (Uₙ - Lₙ + 1)
        +
        +
        +
        + (x - Lₙ)
  ```
