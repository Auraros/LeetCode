# PriorityQueue

PriorityQueue 是抽象集合的一个子类，实现了Queue接口，一方面priority queue提供了如下方法：

- 提取：peek()、poll()
- 添加元素： add()、offer()

定义排序大小：

```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(nums.length, (a,b)-> b-a);  //大顶堆
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(nums.length, (a,b)-> a-b);  //小顶堆
```

使用匿名函数的方法定义优先队列：

```java
PriorityQueue<PQItemNonSortable> pq = new PriorityQueue<>(new Comparator<PQItemNonSortable>() {
            @Override
            public int compare(PQItemNonSortable o1, PQItemNonSortable o2) {
                return o1.getVal() - o2.getVal();
            }
     }); //小顶堆
```

当然还可以输入其他参数：

```java
PriorityQueue<int[]> queue = new PriorityQueue<>(
                (o1, o2) -> (nums1[o1[0]] + nums2[o1[1]]) - (nums1[o2[0]] + nums2[o2[1]]));
//输入第一个数组，数组按照小顶堆排序。
```

## 215. [数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```


示例 2:

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

### 思路

```
思路一：
使用快排算法，先进性排序，再进行提取出第k个元素。

思路二：
使用优先队列，设置大小为len的优先队列，将元素存入到队列中，然后，提取出前k-1个元素，第k个元素即是我们想要的。
```

### 代码

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(nums.length, (a,b)-> b-a);
        for (int i = 0; i < nums.length; i++) {
            maxHeap.add(nums[i]);
        }
        for (int i = 0; i < k - 1; i++) {
            maxHeap.poll();
        }
        return maxHeap.peek();
    }
}
```

## [373. 查找和最小的K对数字](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums/)

给定两个以升序排列的整形数组 nums1 和 nums2, 以及一个整数 k。

定义一对值 (u,v)，其中第一个元素来自 nums1，第二个元素来自 nums2。

找到和最小的 k 对数字 (u1,v1), (u2,v2) ... (uk,vk)。

示例 1:

```
输入: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
输出: [1,2],[1,4],[1,6]

解释: 返回序列中的前 3 对数：
     [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

示例 2:

```
输入: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
输出: [1,1],[1,1]
解释: 返回序列中的前 2 对数：
     [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```


示例 3:

```
输入: nums1 = [1,2], nums2 = [3], k = 3 
输出: [1,3],[2,3]
解释: 也可能序列中所有的数对都被返回:[1,3],[2,3]
```

### 思路

```
使用小顶堆的方法，使用(a,b)点之和比较大小的小顶堆。

第一步，堆先存放数组一中，前k个或者nums.length个索引;第二个坐标为0

第二步，将小顶堆存到数组中，然后加入第二个元素，放入堆
```

### 代码

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
         PriorityQueue<int[]> queue = new PriorityQueue<>(
                (o1, o2) -> (nums1[o1[0]] + nums2[o1[1]]) - (nums1[o2[0]] + nums2[o2[1]]));
        List<List<Integer>> res = new LinkedList<>();
        
        // 两个数组有一个为空，返回空
        if(nums1.length==0 || nums2.length == 0){
            return res;
        }

        // 将我们假想的每个数组的第一项加入小顶堆
        for (int i = 0; i < Math.min(nums1.length, k); i++) {
            queue.add(new int[] { i, 0 }); // 加入的是坐标，小顶堆的比较器也是基于坐标比较
        }

        // 循环K次或者堆空
        while (k > 0 && !queue.isEmpty()) {
            // 弹出堆顶元素
            int[] pair = queue.poll();
            List<Integer> item = new ArrayList<>();
            item.add(nums1[pair[0]]);
            item.add(nums2[pair[1]]);

            // 若我们假想的数组有下一个元素，则加入小顶堆
            if (pair[1] < nums2.length - 1) {
                queue.add(new int[] {pair[0], pair[1] + 1 });
            }
            res.add(item);
            k--;
        }
        return res;
    }
}
```

