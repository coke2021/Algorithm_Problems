# Day 4 Leetcode problem
Includes 24, 19, 142, 160

+ ## Leetcode 24 Swap Nodes in Pairs

#### problem link: https://leetcode.com/problems/swap-nodes-in-pairs/description/
#### Methods: 2 methods

##### method 1: one pointer

Note: remember to use temp to store the nodes will be missing. Use dummyHead(a node before 2 swapped nodes). So it is a 'unified' answer.

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyHead = new ListNode();
        dummyHead.next = head;
        //pointer
        ListNode cur = dummyHead;
        ListNode temp; // store the nodes after the 2 nodes that waited to swap.
        ListNode node1;
        ListNode node2;
        while(cur.next != null && cur.next.next != null){
            node1 = cur.next;
            node2 = cur.next.next;
            temp = cur.next.next.next;
            // start to swap
            cur.next = node2; // point the dummyhead to node 2
            cur.next.next = node1; // point the node2 to node1
            cur.next.next.next = temp; // point the node1 to temp
            cur = cur.next.next; // shift the pointer to the node before two nodes that waited to be swapped
        }
        return dummyHead.next;
    }
}
```

##### method 2: recursive method

Note: Think it like cups in cups. open the cups one by one. Finally get answer in the inner most one. Then, use that answer to answer the question back one by one.

```
Let's say we have the following linked list:

1 -> 2 -> 3 -> 4 -> null

We want to swap every two adjacent nodes, so the expected result is:

2 -> 1 -> 4 -> 3 -> null

Now, let's go through the recursive process:

Initial call: swapPairs(1 -> 2 -> 3 -> 4 -> null)

head = 1
next = 2
newNode = swapPairs(3 -> 4 -> null)
First recursive call: swapPairs(3 -> 4 -> null)

head = 3
next = 4
newNode = swapPairs(null) (base case)
Second recursive call (base case): swapPairs(null)

head == null, so return head (i.e., null)
Now, the base case has been reached, and the recursion starts unwinding.

First recursive call (continuation): swapPairs(3 -> 4 -> null)

newNode = null (from the base case)
next.next = head (4 -> 3)
head.next = newNode (3 -> null)
return next (4 -> 3 -> null)
Initial call (continuation): swapPairs(1 -> 2 -> 3 -> 4 -> null)

newNode = 4 -> 3 -> null (from the first recursive call)
next.next = head (2 -> 1)
head.next = newNode (1 -> 4 -> 3 -> null)
return next (2 -> 1 -> 4 -> 3 -> null)
The recursion has unwound, and we have the final swapped list:

2 -> 1 -> 4 -> 3 -> null
```

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode node1) {
        // base case
        // think it simple. If it last one or no node, return it.
        if(node1 == null || node1.next == null){
            return node1;
        }
        ListNode node2 = node1.next;
        // It's like a black box
        ListNode newNode = swapPairs(node2.next);
        node2.next = node1;
        node1.next = newNode;
        return node2;
    }
}
```

+ ## Leetcode 19 Swap Nodes in Pairs

#### problem link: https://leetcode.com/problems/swap-nodes-in-pairs/description/
#### Methods: 2 methods

##### method 1: 

Note:
