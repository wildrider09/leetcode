# Heap Sort

**Heap sort algorithm template:**

```java
// h stores the values in the heap, h[1] is the top of the heap, the left child of h[x] is 2x and the right child is 2x+1
int[] h = new int[N];

// sift down
void down(int u) {
    int t = u;
    if (u * 2 <= size && h[u * 2] < h[t]) {
        t = u * 2;
    }
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) {
        t = u * 2 + 1;
    }
    if (t != u) {
        swap(t, u);
        down(t);
    }
}

// sift up
void up(int u) {
    while (u / 2 > 0 && h[u / 2] > h[u]) {
        swap(u / 2, u);
        u /= 2;
    }
}

// O(n) build heap
for (int i = n / 2; i > 0; --i) {
    down(i);
}
```

## Problem Description

Given an integer sequence of length n, output the smallest m numbers in ascending order.

**Input Format**

The first line contains the integers n and m.

The second line contains n integers representing the integer sequence.

**Output Format**

A single line containing m integers, representing the smallest m numbers in the sequence.

**Data Range**

- 1 ≤ m ≤ n ≤ 10^5
- 1 ≤ each element in the sequence ≤ 10^9

**Sample Input:**

```
5 3
4 5 1 3 2
```

**Sample Output:**

```
1 2 3
```

## Implementation

<!-- tabs:start -->

#### Java

```java
import java.util.Scanner;

public class Main {
    private static int[] h = new int[100010];
    private static int size;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(), m = sc.nextInt();
        for (int i = 1; i <= n; ++i) {
            h[i] = sc.nextInt();
        }
        size = n;
        for (int i = n / 2; i > 0; --i) {
            down(i);
        }
        while (m-- > 0) {
            System.out.print(h[1] + " ");
            h[1] = h[size--];
            down(1);
        }
    }

    public static void down(int u) {
        int t = u;
        if (u * 2 <= size && h[u * 2] < h[t]) {
            t = u * 2;
        }
        if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) {
            t = u * 2 + 1;
        }
        if (t != u) {
            swap(t, u);
            down(t);
        }
    }

    public static void up(int u) {
        while (u / 2 > 0 && h[u / 2] > h[u]) {
            swap(u / 2, u);
            u /= 2;
        }
    }

    public static void swap(int i, int j) {
        int t = h[i];
        h[i] = h[j];
        h[j] = t;
    }
}
```

<!-- tabs:end -->

## Solutions

### Solution 1

<!-- tabs:start -->

#### Java

```java
import java.util.Scanner;

public class Main {
    private static int[] h = new int[100010];
    private static int size;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(), m = sc.nextInt();
        for (int i = 1; i <= n; ++i) {
            h[i] = sc.nextInt();
        }
        size = n;
        for (int i = n / 2; i > 0; --i) {
            down(i);
        }
        while (m-- > 0) {
            System.out.print(h[1] + " ");
            h[1] = h[size--];
            down(1);
        }
    }

    public static void down(int u) {
        int t = u;
        if (u * 2 <= size && h[u * 2] < h[t]) {
            t = u * 2;
        }
        if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) {
            t = u * 2 + 1;
        }
        if (t != u) {
            swap(t, u);
            down(t);
        }
    }

    public static void up(int u) {
        while (u / 2 > 0 && h[u / 2] > h[u]) {
            swap(u / 2, u);
            u /= 2;
        }
    }

    public static void swap(int i, int j) {
        int t = h[i];
        h[i] = h[j];
        h[j] = t;
    }
}
```

<!-- tabs:end -->
