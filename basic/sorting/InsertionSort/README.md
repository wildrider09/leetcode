# Insertion Sort

Let's start with a question. Given a sorted array, after adding a new element to it, how do we keep the data sorted? It's simple: we just traverse the array, find the position where the element should be inserted, and insert it there.

This is a dynamic sorting process — dynamically adding data into an ordered set. Using this method we can keep the data in the set sorted at all times. And for a set of static data, we can borrow the insertion method described above to sort it, which gives us the insertion sort algorithm.

So how exactly does insertion sort use this idea to sort? 

First, we divide the data in the array into two regions: the **sorted region** and the **unsorted region**. Initially the sorted region has only one element — the first element of the array. The core idea of the insertion algorithm is to take an element from the unsorted region, find the proper insertion position for it in the sorted region, insert it there, and keep the sorted region ordered at all times. Repeat this process until the unsorted region is empty, and the algorithm ends.

Compared with bubble sort:

- In bubble sort, after each pass the numbers at the back of the array are sorted.
- In insertion sort, after each pass the numbers at the front of the array are sorted.

## Code Examples

<!-- tabs:start -->

#### Java

```java
import java.util.Arrays;

public class InsertionSort {

    private static void insertionSort(int[] nums) {
        for (int i = 1, j, n = nums.length; i < n; ++i) {
            int num = nums[i];
            for (j = i - 1; j >= 0 && nums[j] > num; --j) {
                nums[j + 1] = nums[j];
            }
            nums[j + 1] = num;
        }
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 7, 9, 5, 8};
        insertionSort(nums);
        System.out.println(Arrays.toString(nums));
    }
}
```

<!-- tabs:end -->
