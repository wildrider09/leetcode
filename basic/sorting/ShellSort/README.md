# Shell Sort

Shell sort, also called diminishing increment sort, is an improved version of simple insertion sort.

- In insertion sort, if an element in the sequence to be sorted is very far from its insertion position in the sorted sequence, it takes many comparisons to reach that position. This is because the element to be inserted is locally very unordered. For example, in `[2, 3, 4, 5, 6, 7, 8, 1, ...]`, to insert 1 we must compare it against each of the preceding values 2–8 — precisely because the area around 1 is very unordered. Imagine instead that the area around the element to be inserted is fairly ordered; then insertion sort would only need a very few comparisons to place it in the correct position.
- Shell sort first makes the whole sequence relatively ordered, so that when insertion sort is then performed, the number of required comparisons becomes very small.
- The increment (gap) of insertion sort is 1. Shell sort is equivalent to reducing this gap from a maximum of half the array length all the way down to 1, which is clearly reflected in the code. When the gap is large, the number of comparisons is small; after a few large-gap insertion sorts, the sequence is already partially ordered, so subsequent small-gap insertion sorts naturally require few comparisons.
- Shell sort is the process of extracting elements at the same gap, performing insertion sort on them separately, and gradually reducing the gap down to 1.

## Code Examples

<!-- tabs:start -->

#### Java

```java
import java.util.Arrays;

public class ShellSort {

    private static int[] shellSort(int[] arr) {
        int n = arr.length;

        for (int gap = n / 2; gap > 0; gap /= 2) {
            for (int i = gap; i < n; i++) {
                int key = arr[i];
                int j = i;
                while (j >= gap && arr[j - gap] > key) {
                    arr[j] = arr[j - gap];
                    j -= gap;
                }
                arr[j] = key;
            }
        }
        return arr;
    }

    public static void main(String[] args) {
        System.out.println(Arrays.toString(shellSort(new int[] {1, 2, 7, 9, 5, 8})));
    }
}
```

<!-- tabs:end -->
