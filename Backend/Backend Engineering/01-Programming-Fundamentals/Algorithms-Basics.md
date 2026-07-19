# Algorithms Basics for Backend Engineers

## What is it?
Systematic approaches to solving problems efficiently — sorting, searching, recursion, and understanding time/space complexity (Big-O notation).

## Why care?
Backend systems process data at scale — an O(n²) algorithm that's "fine" on 100 rows can bring a production service to its knees at 10 million rows. Understanding complexity is what separates code that "works on my machine" from code that survives real traffic.

## Big-O Notation — the core vocabulary
```
O(1)        constant       — hash map lookup
O(log n)    logarithmic    — binary search, balanced tree operations
O(n)        linear         — a single loop through data
O(n log n)  linearithmic   — efficient sorting (merge sort, quicksort average case)
O(n²)       quadratic      — nested loops over the same data (watch out for this!)
O(2ⁿ)       exponential    — naive recursive solutions without memoization
```

## Searching
- **Linear search**: O(n) — check every element. Fine for small/unsorted data.
- **Binary search**: O(log n) — requires SORTED data, repeatedly halves the search space.
```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

## Sorting (know the trade-offs, not just how to implement)
- **Quicksort/Mergesort**: O(n log n) average — what real languages' built-in sort uses under the hood.
- **Never write your own sort in production** — use the language's built-in (`sorted()`, `Array.sort()`), which is heavily optimized. Understanding the theory matters for interviews and for reasoning about performance, not for reinventing it.

## Recursion and memoization
```python
# Naive recursive Fibonacci — O(2^n), catastrophically slow for large n
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

# Memoized — O(n), because we cache repeated subproblem results
def fib_memo(n, cache={}):
    if n in cache: return cache[n]
    if n <= 1: return n
    cache[n] = fib_memo(n-1, cache) + fib_memo(n-2, cache)
    return cache[n]
```
This exact pattern — caching repeated work — is the algorithmic idea behind application-level caching (`06-Caching`) at a much larger scale.

## In backend engineering
- **N+1 query problems** (a very common real backend bug) are literally an algorithmic complexity mistake: looping over N records and issuing 1 database query per record (O(n) queries) instead of one batched query — understanding Big-O is what lets you spot and explain this bug.
- **Pagination** exists specifically because loading/sorting an entire dataset (O(n log n) or worse) doesn't scale — returning results in efficient, indexed chunks is a direct, practical application of complexity awareness.
- **Rate limiting algorithms** (token bucket, sliding window — see `12-Scalability-and-Performance/Rate-Limiting.md`) are themselves specific, well-known algorithms with clear complexity/memory trade-offs.
