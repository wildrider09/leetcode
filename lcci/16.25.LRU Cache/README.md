---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.25.LRU%20Cache/README_EN.md
---

<!-- problem:start -->

# [16.25. LRU Cache](https://leetcode.cn/problems/lru-cache-lcci)

[Chinese Version](/lcci/16.25.LRU%20Cache/README.md)

## Description

<!-- description:start -->

<p>Design and build a &quot;least recently used&quot; cache, which evicts the least recently used item. The cache should map from keys to values (allowing you to insert and retrieve a value associ&shy;ated with a particular key) and be initialized with a max size. When it is full, it should evict the least recently used item.</p>

<p>You should implement following operations:&nbsp;&nbsp;<code>get</code>&nbsp;and <code>put</code>.</p>

<p>Get a value by key:&nbsp;<code>get(key)</code> - If key is in the cache, return the value, otherwise return -1.<br />

Write a key-value pair to the cache:&nbsp;<code>put(key, value)</code> - If the key is not in the cache, then write its value to the cache. Evict the least recently used item before writing if necessary.</p>

<p><strong>Example:</strong></p>

<pre>

LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);

cache.put(2, 2);

cache.get(1);       // returns 1

cache.put(3, 3);    // evicts key 2

cache.get(2);       // returns -1 (not found)

cache.put(4, 4);    // evicts key 1

cache.get(1);       // returns -1 (not found)

cache.get(3);       // returns 3

cache.get(4);       // returns 4

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Doubly Linked List

We can implement an LRU (Least Recently Used) cache using a "hash table" and a "doubly linked list".

- Hash Table: Used to store the key and its corresponding node location.
- Doubly Linked List: Used to store node data, sorted by access time.

When accessing a node, if the node exists, we delete it from its original position and reinsert it at the head of the list. This ensures that the node stored at the tail of the list is the least recently used node. When the number of nodes exceeds the maximum cache space, we eliminate the node at the tail of the list.

When inserting a node, if the node exists, we delete it from its original position and reinsert it at the head of the list. If it does not exist, we first check if the cache is full. If it is full, we delete the node at the tail of the list and insert the new node at the head of the list.

The time complexity is $O(1)$, and the space complexity is $O(\textit{capacity})$.

<!-- tabs:start -->

#### Java

```java
class Node {
    int key;
    int val;
    Node prev;
    Node next;

    Node() {
    }

    Node(int key, int val) {
        this.key = key;
        this.val = val;
    }
}

class LRUCache {
    private Map<Integer, Node> cache = new HashMap<>();
    private Node head = new Node();
    private Node tail = new Node();
    private int capacity;
    private int size;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        Node node = cache.get(key);
        moveToHead(node);
        return node.val;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.val = value;
            moveToHead(node);
        } else {
            Node node = new Node(key, value);
            cache.put(key, node);
            addToHead(node);
            ++size;
            if (size > capacity) {
                node = removeTail();
                cache.remove(node.key);
                --size;
            }
        }
    }

    private void moveToHead(Node node) {
        removeNode(node);
        addToHead(node);
    }

    private void removeNode(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void addToHead(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next = node;
        node.next.prev = node;
    }

    private Node removeTail() {
        Node node = tail.prev;
        removeNode(node);
        return node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
