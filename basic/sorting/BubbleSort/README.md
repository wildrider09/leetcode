# Bubble Sort

Define a boolean variable `hasChange` to mark whether any swap was performed in each pass. At the start of each pass, set `hasChange` to false.

If no swap occurs during a pass, it means the array is already sorted in ascending order and `hasChange` remains false. At this point the outer loop exits directly and the sort is finished.

## Code Examples

<!-- tabs:start -->

#### Java

```java
import java.util.Arrays;

public class BubbleSort {

    private static void bubbleSort(int[] nums) {
        boolean hasChange = true;
        for (int i = 0, n = nums.length; i < n - 1 && hasChange; ++i) {
            hasChange = false;
            for (int j = 0; j < n - i - 1; ++j) {
                if (nums[j] > nums[j + 1]) {
                    swap(nums, j, j + 1);
                    hasChange = true;
                }
            }
        }
    }

    private static void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 7, 9, 5, 8};
        bubbleSort(nums);
        System.out.println(Arrays.toString(nums));
    }
}
```

<!-- tabs:end -->
