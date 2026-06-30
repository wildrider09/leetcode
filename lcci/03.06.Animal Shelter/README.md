---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/03.06.Animal%20Shelter/README_EN.md
---

<!-- problem:start -->

# [03.06. Animal Shelter](https://leetcode.cn/problems/animal-shelter-lcci)

[Chinese Version](/lcci/03.06.Animal%20Shelter/README.md)

## Description

<!-- description:start -->

<p>An animal shelter, which holds only dogs and cats, operates on a strictly&quot;first in, first out&quot; basis. People must adopt either the&quot;oldest&quot; (based on arrival time) of all animals at the shelter, or they can select whether they would prefer a dog or a cat (and will receive the oldest animal of that type). They cannot select which specific animal they would like. Create the data structures to maintain this system and implement operations such as <code>enqueue</code>, <code>dequeueAny</code>, <code>dequeueDog</code>, and <code>dequeueCat</code>. You may use the built-in Linked list data structure.</p>

<p><code>enqueue</code> method has a <code>animal</code> parameter, <code>animal[0]</code> represents the number of the animal, <code>animal[1]</code> represents the type of the animal, 0 for cat and 1 for dog.</p>

<p><code>dequeue*</code> method returns <code>[animal number, animal type]</code>, if there&#39;s no animal that can be adopted, return <code>[-1, -1]</code>.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: 

[&quot;AnimalShelf&quot;, &quot;enqueue&quot;, &quot;enqueue&quot;, &quot;dequeueCat&quot;, &quot;dequeueDog&quot;, &quot;dequeueAny&quot;]

[[], [[0, 0]], [[1, 0]], [], [], []]

<strong> Output</strong>: 

[null,null,null,[0,0],[-1,-1],[1,0]]

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: 

[&quot;AnimalShelf&quot;, &quot;enqueue&quot;, &quot;enqueue&quot;, &quot;enqueue&quot;, &quot;dequeueDog&quot;, &quot;dequeueCat&quot;, &quot;dequeueAny&quot;]

[[], [[0, 0]], [[1, 0]], [[2, 1]], [], [], []]

<strong> Output</strong>: 

[null,null,null,null,[2,1],[0,0],[1,0]]

</pre>

<p><strong>Note:</strong></p>

<ol>
	<li>The number of animals in the shelter will not exceed 20000.</li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Array of Queues

We define an array $q$ of length $2$ to store the queues of cats and dogs.

In the `enqueue` operation, assuming the animal number is $i$ and the animal type is $j$, we enqueue $i$ into $q[j]$.

In the `dequeueAny` operation, we check whether $q[0]$ is empty, or $q[1]$ is not empty and $q[1][0] < q[0][0]$. If so, we call `dequeueDog`, otherwise we call `dequeueCat`.

In the `dequeueDog` operation, if $q[1]$ is empty, we return $[-1, -1]$, otherwise we return $[q[1].pop(), 1]$.

In the `dequeueCat` operation, if $q[0]$ is empty, we return $[-1, -1]$, otherwise we return $[q[0].pop(), 0]$.

The time complexity of the above operations is $O(1)$, and the space complexity is $O(n)$, where $n$ is the number of animals in the animal shelter.

<!-- tabs:start -->

#### Java

```java
class AnimalShelf {
    private Deque<Integer>[] q = new Deque[2];

    public AnimalShelf() {
        Arrays.setAll(q, k -> new ArrayDeque<>());
    }

    public void enqueue(int[] animal) {
        q[animal[1]].offer(animal[0]);
    }

    public int[] dequeueAny() {
        if (q[0].isEmpty() || (!q[1].isEmpty() && q[1].peek() < q[0].peek())) {
            return dequeueDog();
        }
        return dequeueCat();
    }

    public int[] dequeueDog() {
        return q[1].isEmpty() ? new int[] {-1, -1} : new int[] {q[1].poll(), 1};
    }

    public int[] dequeueCat() {
        return q[0].isEmpty() ? new int[] {-1, -1} : new int[] {q[0].poll(), 0};
    }
}

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * AnimalShelf obj = new AnimalShelf();
 * obj.enqueue(animal);
 * int[] param_2 = obj.dequeueAny();
 * int[] param_3 = obj.dequeueDog();
 * int[] param_4 = obj.dequeueCat();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
