---
title: ARTS for 1st week！
tags: ARTS
date: 2019-01-20
---


# What's ARTS? 

>Algorithm：每周至少做一个 LeetCode 的算法题；
Review：阅读并点评至少一篇英文技术文章；
Tip/Techni：学习至少一个技术技巧；
Share：分享一篇有观点和思考的技术文章。

# Algorithm ---- 链表相关

本周回顾了几个经典的 **链表** 相关算法题：

![](https://static001.geekbang.org/resource/image/aa/25/aa5000d3a2079c75a5cbe32772d52625.jpg#pic_center)


##  LeetCode-19. Remove Nth Node From End of List（删除链表的倒数第 N 个结点）
[Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

**题目分析**：由于是单链表，要删除某个结点，就必须得知道这个结点的前一个结点，所以问题就转变为找倒数第 N+1 个结点。在不知道链表长度的情况下，如何通过一次遍历来定位倒数第 N+1 个结点呢？通过设置两个指针（或者说引用）first , second ，让两个指针距离合适的长度，然后同步移动，就能定位倒数第 N+1 个结点了。代码如下：
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return null;
        }
        // 哨兵结点的存在，使得删除第一个结点和删除其他结点都可以统一为相同的代码实现逻辑
        ListNode start = new ListNode(0);   // 哨兵结点
        ListNode first = start;
        ListNode second = start;
        start.next = head;
        // 移动 first , 使得 first 和 second 相距 n+1 个结点
        for (int i = 1; i <= n + 1; i++) {
            first = first.next;
        }
        // first 和 second 同步向后移动
        while (first != null) { // while 循环结束后，second指针就指向了第倒数 n + 1 个结点，即待删除结点的前一个结点
            second = second.next;
            first = first.next;
        }
        // 删除倒数第 n 个节点
        second.next = second.next.next;            
        return start.next;
    }
}
```

这里采用哨兵结点，来使得删除第一个结点跟删除其他结点都能统一为相同的代码实现逻辑。

##  LeetCode-141.Linked List Cycle（判断链表是否有环）
[Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

**题目分析**：经典的实现就是采用 **快慢指针** ， 如果链表存在环的话，那么快指针 fast 跟慢指针 slow 是一定会相遇的！代码如下：

```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) { // 快慢指针相遇，说明有环
                return true;
            }
        }
        return false;
    }
}
```

这里要警惕指针丢失，while 判断里头不要丢了 fast.next != null 

##  LeetCode-142.Linked List Cycle II（找到带环链表的环入口）

[Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

**题目分析**：判断链表有环很容易，那么环的入口如何知道呢？敲黑板，划重点：**快慢指针相遇的结点 meet 到环入口结点的距离 等于 头结点 head 到环入口结点的距离**。代码如下：

```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        ListNode meet = null;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                meet = slow;    // 相遇点
                break;
            }
        }
        
        // 快慢指针相遇的结点 meet 到环起点的距离 = head到环起点的距离
        if (meet != null) {
            while (meet != head) {  
                meet = meet.next;
                head = head.next;
            }
        }
        return meet;
    }
}
```

##  LeetCode-206.Reverse Linked List（链表反转）
[Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

**题目分析**：相当经典的一道题，面试官也是喜欢考。这个题也是通过 3 个指针的移动、变换，从而能通过一次遍历，就能反转链表。

>head：当前结点
pre：当前结点的前一个结点
next：当前结点的下一个结点

再反转指针指向之前，一定要把 下一个结点 先保存起来，不然弄丢了，你去哪儿找，对应代码的第 17 行。这个题，比较抽象，用手画图理解是个好办法。

代码如下：


```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode pre = null;
        ListNode next = null;
        while (head != null) {
            next = head.next;   // next 保存 head 的下一个结点
            head.next = pre;    // 反转
            pre = head;     // pre 指针右移
            head = next;    // head 指针右移
        }
        return pre;     // while 循环走完后，pre 会走到旧链表的链尾，即新链表的链头
    }
}
```

##  LeetCode-876. Middle of the Linked List（找链表的中间结点）

[ Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

**题目分析**：根据题目描述，如果能知道链表的长度 N，那么中间结点就是第 N/2 + 1  个结点。所以，一种实现办法就是，先遍历链表得到链表长度 N , 然后再从头结点找 第 N/2 + 1 个结点。但是，这种实现需要遍历链表两次，有点难受，实际上，这里也可以采用快慢指针来解决。代码如下：

解法一：
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        if (head == null) {
            return null;
        }
        if (head.next == null) {
            return head;
        }
        int len = 0;  // 链表长度
        ListNode newHead = head;
        while (head != null) {
            head = head.next;
            len++;
        }
        int mid = len / 2 + 1;  // 链表的中间位置
        for (int i = 0; i < mid - 1; ++i) {
            newHead = newHead.next;
        }
        return newHead;
    }
}
```

解法二：快慢指针
```
/**
* 解法二：快慢指针，优秀哇，一次遍历就搞定了！
*/
public ListNode middleNode(ListNode head) {
     ListNode slow = head, fast = head;
     while (fast != null && fast.next != null) {
         slow = slow.next;
         fast = fast.next.next;
    }
    return slow;
}
```

# Review ---- Software Engineering at Google 
前几天，在 [湾区日报](https://wanqu.co/) 上看到推送了这篇《Software Engineering at Google 》，比较感兴趣，想了解了解世界一流的互联网公司的开发流程及规范。文章链接 [在这儿](https://arxiv.org/ftp/arxiv/papers/1702/1702.01715.pdf) ，推荐大家有空时也读一读。

这篇文章是 Google的 骨灰级码农 Fergus Henderson 写的。比较详细的介绍了 Google 公司的 **软件管理、项目管理、人员管理** 。

## Software development

主要了解下软件管理，从以下几个维度对 Google 公司的开发流程及规范做了介绍：

>1、Google 的代码管理
2、Blaze：分布式构建系统
3、Code Review：代码审查
4、Testing：单元、集成、回归测试
5、Bug tracking：Buganizer ---- Bug tracking system.
6、Programming languages：5种编程语言，代码风格一致
7、Debugging and Profiling tools：调试和分析工具
8、Release engineering：发布工程
9：Launch approval 启动批准
10、Post-mortems：事故复盘，总结经验教训
11：Frequent rewrites：经常重写项目

## 总结：
Google 作为世界一流的互联网企业，其软件开发流程是非常规范的，毕竟人家是这个行业的先行者，是这个行业的标杆。Code Management 和 Code Review 让人印象深刻！事实上，也能从国内的互联网公司的开发流程中看到 Google 软件管理的影子。

# Tip/Tech

解决链表相关算法题的一些技巧：

>- 1、理解指针或引用的含义
>- 2、警惕指针丢失和内存泄漏
>- 3、利用哨兵简化实现难度
>- 4、重点留意边界条件处理
如果链表为空时，代码能否正常工作？
如果链表只包含 1 个结点时，代码能否正常工作？
如果链表只包含 2 个结点时，代码能否正常工作？
代码逻辑在处理头结点和尾结点时，能否正常工作？
>- 5、举例画图，辅助思考

# Share

分享一个系列文章：Google Chrome 的调试工具技巧：[Chrome 的调试工具技巧](https://juejin.im/post/5c26f4a36fb9a049f3622e10)







