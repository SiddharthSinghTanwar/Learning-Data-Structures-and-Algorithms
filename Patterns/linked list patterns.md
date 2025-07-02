### 📌 Pattern: **Merge Two Sorted Linked Lists**

**Use Case**: Merge two sorted lists into one sorted list.

**Techniques**:

- Use a dummy head and iterate while comparing nodes.
- Can be done iteratively or recursively.

**Time Complexity**: O(n + m)

**Space**:

- Iterative: O(1)
- Recursive: O(n + m) (due to call stack)

Template (Iterative):

```python
def merge(l1, l2):
    dummy = ListNode(-1)
    tail = dummy
    while l1 and l2:
        if l1.val < l2.val:
            tail.next, l1 = l1, l1.next
        else:
            tail.next, l2 = l2, l2.next
        tail = tail.next
    tail.next = l1 or l2
    return dummy.next
```

### 📌 Pattern: **Reversal of Linked List (Entire / In Parts)**

**Use Case**: Full reverse or reverse in k-groups / subranges

**Techniques**:

- Track `prev`, `curr`, and `next`
- For groups, reverse one group and recursively call for the rest

**Time Complexity**: O(n)

**Space**: O(1)

**Template** (Full Reverse):

```python
def reverse(head):
    prev = None
    curr = head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt
    return prev
```

### 📌 Pattern: **Pointer Rewiring**

**Use Case**: Problems where you restructure nodes (e.g., delete, reorder)

**Examples**:

- Remove N-th node from end
- Reorder list (1→n→2→n-1...)
- Split and merge parts of the list

**Core Idea**:

- You don't "move values" — you **change `.next` pointers**
- Dry-run and draw pointers to avoid errors

**Pro Tip**:

- Use dummy node often to avoid edge case headaches

### 📌 Pattern: **Clone Linked List with Random Pointers**

**Use Case**: Create a deep copy of a linked list with `.random` pointers.

**Techniques**:

1. First pass: interleave clone nodes after original nodes
2. Second pass: set `.random` pointers
3. Third pass: separate clone list from original

**Time**: O(n)

**Space**: O(1) if interleaved; O(n) if using hashmap

### 📌 Pattern: **Cycle Detection and Entry Point**

**Use Case**: Check if a cycle exists and find where it begins

**Algorithm**: Floyd’s Tortoise and Hare

**Steps**:

1. Fast = fast.next.next, slow = slow.next
2. If they meet, cycle exists.
3. To find entry: set one pointer to head, move both by 1 step

**Time**: O(n)

**Space**: O(1)

---

### 📌 Pattern: **LRU Cache (DLL + Hashmap)**

**Use Case**: O(1) insert, delete, access

**Data Structures**:

- DLL for ordering
- Hashmap for fast lookup

**Operations**:

- On access: remove from DLL, move to front
- On insert: if capacity, remove tail; add to front
- Update hashmap accordingly

**Time**: O(1) for all operations

**Space**: O(n)

---

### 📌 Pattern: **Reordering / Reversing Subparts**

**Use Case**: Reorder as 1→n→2→n−1→… or reverse parts in k-groups

**Steps for Reorder**:

1. Find middle
2. Reverse second half
3. Merge two halves

**Steps for k-group reversal**:

1. Check if group has k nodes
2. Reverse k nodes
3. Recursively connect remaining list

**Time**: O(n)

**Space**: O(1)
