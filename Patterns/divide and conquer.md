## How to Come Up with Divide and Conquer Solutions?

### Core idea:

> If the problem can be broken down into subproblems of the same type — solve them independently and combine their results.
> 

Classic examples:

- Merge Sort
- Quick Sort
- Binary Search
- Closest pair of points
- K-th smallest/largest
- Maximum subarray (what you're doing now)

---

### 🔁 General Pattern:

```python
Function(arr, l, r):
    if base_case:
        return answer

    mid = (l + r) // 2

    left_ans = Function(arr, l, mid)
    right_ans = Function(arr, mid+1, r)
    cross_ans = Combine(left_ans, right_ans)

    return cross_ans
```

> Divide → Solve → Combine
> 

The trickiest part usually is **Combine**.

#### Important: Combine ≠ Merge

In merge sort, combine means to merge the sorted arrays. In max subarray, it means finding the max sum crossing `mid`.

### Recognizing Divide and Conquer Patterns in DSA
Look for problems where:

| Feature | D&C Good Fit? |
| --- | --- |
| Problem has **substructure** that can be split | ✅ |
| Problem asks for **global result** made of local decisions | ✅ |
| Problem resembles sorting, searching, k-th, closest, etc. | ✅ |
| Problem has **natural midpoint** like array index | ✅ |
| Naive recursion is expensive and optimization is needed | ✅ |

### Examples by Category:

| Problem | Type | Combine Step |
| --- | --- | --- |
| Merge Sort | Sorting | Merge arrays |
| Quick Sort | Sorting | Partitioning |
| Binary Search | Search | No combine |
| Kth smallest | Selection | Use quickselect (partition) |
| Max subarray | DP/Array | Max of left, right, cross |
| Closest pair of points | Geometry | Compare across midpoint |

## When to Use Divide and Conquer vs Other Approaches

| Situation | Use D&C if… | Prefer alternative if… |
| --- | --- | --- |
| Need to find something over **range** (min, max, kth) | D&C works well | Problem is better solved with heap or greedy |
| You can **split and combine** logically | D&C ideal | You need to make **sequential decisions** (use DP) |
| Input size is big and recursive splitting helps | D&C is fast | Input is small or non-recursive structure is clearer |

## TL;DR Mindset for Divide and Conquer:

> “Can I break this into independent subproblems of the same type, and then combine the results meaningfully?”
> 

If yes, then D&C might be the best lens.
