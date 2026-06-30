# Merge Sort

The core idea of merge sort is divide and conquer — splitting a complex problem into several subproblems to solve.

The algorithmic idea of merge sort is: divide the array in half into two subarrays, recursively keep dividing the subarrays into smaller arrays, and begin sorting when a subarray contains only one element. The sorting method is to merge two elements in order of magnitude. Then, following the recursion order, return step by step, continually merging the sorted arrays until the entire array is sorted.

**Merge sort algorithm template:**

```java
void mergeSort(int[] nums, int left, int right) {
    if (left >= right) {
        return;
    }
    int mid = (left + right) >>> 1;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) {
            tmp[k++] = nums[i++];
        } else {
            tmp[k++] = nums[j++];
        }
    }
    while (i <= mid) {
        tmp[k++] = nums[i++];
    }
    while (j <= right) {
        tmp[k++] = nums[j++];
    }
    for (i = left, j = 0; i <= right; ++i, ++j) {
        nums[i] = tmp[j];
    }
}
```

## Problem Description

You are given an integer sequence of length `n`.

Use merge sort to sort this sequence in ascending order.

Then output the sorted sequence in order.

**Input Format**

The input consists of two lines. The first line contains the integer n.

The second line contains n integers (all in the range 1∼10^9), representing the whole sequence.

**Output Format**

Output a single line containing n integers, representing the sorted sequence.

**Data Range**

1≤n≤100000

**Sample Input:**

```
5
3 1 2 4 5
```

**Sample Output:**

```
1 2 3 4 5
```

## Implementation

<!-- tabs:start -->

#### Java

```java
import java.util.Scanner;

public class Main {
    private static int[] tmp = new int[100010];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] nums = new int[n];
        for (int i = 0; i < n; ++i) {
            nums[i] = sc.nextInt();
        }
        mergeSort(nums, 0, n - 1);
        for (int i = 0; i < n; ++i) {
            System.out.printf("%d ", nums[i]);
        }
    }

    public static void mergeSort(int[] nums, int left, int right) {
        if (left >= right) {
            return;
        }
        int mid = (left + right) >>> 1;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid + 1, right);
        int i = left, j = mid + 1, k = 0;
        while (i <= mid && j <= right) {
            if (nums[i] <= nums[j]) {
                tmp[k++] = nums[i++];
            } else {
                tmp[k++] = nums[j++];
            }
        }
        while (i <= mid) {
            tmp[k++] = nums[i++];
        }
        while (j <= right) {
            tmp[k++] = nums[j++];
        }
        for (i = left, j = 0; i <= right; ++i, ++j) {
            nums[i] = tmp[j];
        }
    }
}
```

<!-- tabs:end -->
