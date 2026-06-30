# Counting Sort

Counting sort is a non-comparison-based sorting algorithm that trades space for time. It determines an element's position by its value, and is suitable for sorting non-negative integers (if negative numbers need to be sorted, add `0 - min` to all elements first and subtract that value afterward).

Algorithm description:

- Given the range `[min, max]` of element values in the original array
- Create a new array `c` of length `max - min + 1`, with every element defaulting to `0`
- Traverse the elements of the original array, using each element as an index into `c`, and the number of occurrences of that element as the value stored in `c`.
- Create a result array `r` with starting index `i`
- Traverse array `c`, find the elements greater than `0`, and fill their corresponding indices into `r` as values. Each time one is processed, decrement that element of `c` by `1`, until its value is no longer greater than `0`; then process the remaining elements of `c` in order.

## Algorithm Analysis

- Time complexity $O(n+k)$, where $n$ is the length of the array to be sorted and $k$ is the range of values in it. When $k \lt n$, the time complexity is $O(n)$.
- Space complexity $O(n+k)$, where $n$ is the length of the array to be sorted and $k$ is the range of values in it. When $k \lt n$, the space complexity is $O(n)$.
