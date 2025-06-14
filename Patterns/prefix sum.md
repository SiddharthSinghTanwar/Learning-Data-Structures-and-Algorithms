The Prefix Sum technique is a fundamental concept in DSA used for efficient querying of the sum over ranges in an array. It works by pre-computing the sum of all elements up to each index, storing these sums in a "prefix sum array.”

### **Key Idea:**

If you need to find the sum of elements from index `i` to `j` (inclusive) in the original array `arr`, you can simply calculate `prefix_sum[j] - prefix_sum[i-1]`. If `i` is 0, the sum is `prefix_sum[j]`.

### **Construction of Prefix Sum Array:**

Let `arr` be the original array of size `N`.
Let `prefix_sum` be the prefix sum array of size `N`.

- `prefix_sum[0] = arr[0]`
- For `k > 0`, `prefix_sum[k] = prefix_sum[k-1] + arr[k]`

### **Time and Space Complexity:**

**For the Classic 1D Prefix Sum (Array)**
- **Construction:**
  - **Build Time:** O(N) — single pass to construct prefix array.
  - **Space:** O(N) — stores N prefix sums.
- **Query:** O(1) time — `sum[L..R] = prefix[R] - prefix[L-1]` (with a guard for L = 0).

**Multi-dimensional Prefix Sum (2D or 3D)**

- **2D prefix sum grid:**
    `prefix[i][j] = sum of submatrix from (0,0) to (i,j)`

- **Build Time:** O(N*M)

- **Space:** O(N*M)

- **Query Time:** O(1) using the inclusion-exclusion formula.

Still constant time queries, but more space and build time.

**Sparse Data / Hash Map Based Prefix Sum**

- E.g., in problems involving subarrays with target sums (like "Subarray Sum Equals K"), you might store prefix sums in a hash map.

- **Build Time:** Still O(N)

- **Space:** Up to O(N) for hashmap

- **Query Time:** Varies, but not usually constant per query.

**Dynamic Updates (Mutable Array)**

If the underlying array can change and you still want fast range queries:

- Prefix sum breaks — you can no longer answer queries in O(1) without rebuilding.

- You need a Fenwick Tree (BIT) or a Segment Tree:

  - **Update:** O(log N)

  - **Query:** O(log N)

  - **Space:** O(N)

**Space Optimization**

You can build a prefix sum in-place in the original array:

  - Overwrite arr[i] with the prefix up to i.

  - Space: O(1) extra (excluding input).

  - But this destroys the original array.

**Advantages:**

- **Fast Range Sum Queries:** Once the prefix sum array is built, any range sum query takes constant time. This is a significant improvement over iterating through the range, which would take O(k) time for a range of size k.
- **Reduces Redundant Calculations:** Avoids recalculating sums for overlapping ranges.

**Disadvantages:**

- **Static Array:** Best suited for arrays where elements do not change frequently. If elements are modified, the prefix sum array needs to be recomputed or updated, which can be costly.
- **Extra Space:** Requires additional space to store the prefix sum array.

**Common Applications:**

- **Finding Subarray Sums:**
    - Find a subarray with a given sum.
    - Count subarrays with a given sum.
    - Maximum subarray sum (Kadane's Algorithm is often more efficient for this, but prefix sum can be used for variations).
- **2D Prefix Sum (Matrix Sums):** Extendable to 2D arrays (matrices) to find the sum of elements within a rectangular sub-matrix in O(1) time.
- **Equilibrium Index/Point:** An index where the sum of elements to its left is equal to the sum of elements to its right.
- **Problems involving "at most K" or "exactly K" sums.**

**Example (1D Prefix Sum):**

`arr = [1, 2, 3, 4, 5]`

`prefix_sum = [1, 3, 6, 10, 15]`

Sum from index 1 to 3 (inclusive): `arr[1] + arr[2] + arr[3] = 2 + 3 + 4 = 9`
Using prefix sum: `prefix_sum[3] - prefix_sum[1-1] = prefix_sum[3] - prefix_sum[0] = 10 - 1 = 9`

**Ideal usage:** 
Ideal when you need to answer **multiple range sum queries** efficiently on a **static array**. It's a pre-computation technique. Think "what's the sum from A to B?".
