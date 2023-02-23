# Day 3 Leetcode problem
Includes 203, 707, 26

+ ## Leetcode 977 Squares of a Sorted Array

#### problem link: https://leetcode.com/problems/remove-linked-list-elements/submissions/901892468/

#### Methods: 1 unified method and 1 ununified method and 1 method(not use prev)

##### ununified method

Note: This method likes two pointer method: current keep traverse through the linked list and previous stop when the current is equal to the target value.

During each iteration of the while loop, the "current" pointer is updated to point to the next node in the list, and the "previous" pointer is updated to point to the node that was just visited by the "current" pointer. This way, the "previous" pointer always points to the node that precedes the "current" node in the list.

When a node is found that matches the value "val", the "previous" pointer is used to change the "next" reference of the previous node to point to the next node after the current node. This effectively removes the current node from the list.
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
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        ListNode current = head;
        ListNode previous = null;
        while(current != null){
            if(current.val == val){
                //ununified, check if the previous is null
                //at start previous can be null
                if(previous == null) {
                    head = current.next;
                } else {
                    //change the address! not value
                    previous.next = current.next;
                }
            } else {
                previous = current;
            }
            current = current.next;
        }
        return head;
    }
}
```

##### unified method (dummy head)
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
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        ListNode dummyHead = new ListNode(-1, head);
        ListNode current = head;
        ListNode previous = dummyHead;
        while(current != null){
            if(current.val == val){
                //change the address! not value
                previous.next = current.next;
            } else {
                previous = current;
            }
            current = current.next;
        }
        return dummyHead.next;
    }
}
```

##### method 3

Note: not use the prev. To deal with the head value, it write a while loop to check the head. If head is the element need to be deleted, 
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
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        while (head != null && head.val == val) {
            head = head.next;
        }
        ListNode current = head;
        while(current != null){
            while(current.next != null && current.next.val == val){
                current.next = current.next.next;
            }
            current = current.next;
        }
        return head;
    }
}
```

+ ## Leetcode 707 Squares of a Sorted Array

#### problem link: https://leetcode.com/problems/remove-linked-list-elements/submissions/901892468/

#### Methods: 1 unified method and 1 ununified method

##### ununified method:
```
