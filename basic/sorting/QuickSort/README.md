# Quick Sort

Quick sort also adopts the divide-and-conquer idea: partition the original array into a smaller and a larger subarray, then recursively sort the two subarrays.

**Quick sort algorithm template:**

```java
void quickSort(int[] nums, int left, int right) {
    if (left >= right) {
        return;
    }
    int i = left - 1, j = right + 1;
    int x = nums[left];
    while (i < j) {
        while (nums[++i] < x)
            ;
        while (nums[--j] > x)
            ;
        if (i < j) {
            int t = nums[i];
            nums[i] = nums[j];
            nums[j] = t;
        }
    }
    quickSort(nums, left, j);
    quickSort(nums, j + 1, right);
}
```

## Problem Description

You are given an integer sequence of length `n`.

Use quick sort to sort this sequence in ascending order.

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
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] nums = new int[n];
        for (int i = 0; i < n; ++i) {
            nums[i] = sc.nextInt();
        }
        quickSort(nums, 0, n - 1);
        for (int i = 0; i < n; ++i) {
            System.out.print(nums[i] + " ");
        }
    }

    public static void quickSort(int[] nums, int left, int right) {
        if (left >= right) {
            return;
        }
        int i = left - 1, j = right + 1;
        int x = nums[(left + right) >> 1];
        while (i < j) {
            while (nums[++i] < x)
                ;
            while (nums[--j] > x)
                ;
            if (i < j) {
                int t = nums[i];
                nums[i] = nums[j];
                nums[j] = t;
            }
        }
        quickSort(nums, left, j);
        quickSort(nums, j + 1, right);
    }
}
```

<!-- tabs:end -->
