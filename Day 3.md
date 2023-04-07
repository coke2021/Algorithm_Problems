# Day 3 Leetcode problem
Includes 203, 707, 26

+ ## Leetcode 203 Remove Linked List Elements

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

+ ## Leetcode 707 Design Linked List

#### problem link: https://leetcode.com/problems/design-linked-list/

#### Methods: 2 method

##### method 1: singly-linked list

Note: Be careful to point the added ListNode to the next listNode before make the listNode and the left(before) point the next to the new added listNode. Careful about the steps.
```
//singly-linked list
public class ListNode {
    int val;
    ListNode next;
    ListNode(){}
    ListNode(int val){
        this.val = val;
    }
    ListNode(int val, ListNode next){
        this.val = val;
        this.next = next;
    }
}

class MyLinkedList {
    int size;
    ListNode dummyHead;

    public MyLinkedList() {
        size = 0;
        //dummy head initialization
        dummyHead = new ListNode(-1);
    }
    
    public int get(int index) {
        if(index < 0 || index > size - 1){
            return -1;
        }
        ListNode currentNode = dummyHead;
        for(int i = 0; i <= index; i++){
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        if(index > size){
            return;
        } else if (index < 0){
            index = 0;
        }
        size++;
        ListNode toAdd = new ListNode(val);
        //find preceding element
        ListNode pred = dummyHead;
        for(int i = 0; i < index; i++){
            pred = pred.next;
        }
        toAdd.next = pred.next;
        pred.next = toAdd;
    }
    
    public void deleteAtIndex(int index) {
        if(index < 0 && index > size - 1){
            return;
        }
        size--;
        ListNode pred = dummyHead;
        for(int i = 0; i < index; i++){
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

##### method 2: double linked list

```
//doublly-linked list
public class ListNode {
    int val;
    ListNode next;
    ListNode prev;
    ListNode(){}
    ListNode(int val){
        this.val = val;
    }
    ListNode(int val, ListNode next, ListNode prev){
        this.val = val;
        this.next = next;
        this.prev = prev;
    }
}

class MyLinkedList {
    int size;
    ListNode dummyhead, dummytail;

    public MyLinkedList() {
        this.size = 0;
        //dummy head & tail initialization
        this.dummyhead = new ListNode(-1);
        this.dummytail = new ListNode(-1);
        dummyhead.next = dummytail;
        dummytail.prev = dummyhead;
    }
    
    public int get(int index) {
        if(index < 0 || index >= size){
            return -1;
        }
        ListNode currentNode = dummyhead;
        if(index < size / 2){
            for(int i = 0; i <= index; i++){
                currentNode = currentNode.next;
            }
        } else {
            currentNode = dummytail;
            for(int i = size - 1; i >= size - index; i--){
                currentNode = currentNode.prev;
            }
        }
        return currentNode.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        if(index > size){
            return;
        } else if (index < 0){
            index = 0;
        }
        size++;
        ListNode toAdd = new ListNode(val);
        //find preceding element
        ListNode pred = dummyhead;
        for(int i = 0; i < index; i++){
            pred = pred.next;
        }
        toAdd.next = pred.next;
        toAdd.prev = pred;
        pred.next.prev = toAdd;
        pred.next = toAdd;
    }
    
    public void deleteAtIndex(int index) {
        if(index < 0 && index > size - 1){
            return;
        }
        if(index < size / 2){
            ListNode pred = dummyhead;
            for(int i = 0; i < index; i++){
                pred = pred.next;
            }
            pred.next.next.prev = pred;
            pred.next = pred.next.next;
            size--;
        } else {
            ListNode pred = dummytail;
            for(int i = size - 1; i >= size - index; i--){
                pred = pred.prev;
            }
            pred.prev = pred.prev.prev;
            pred.prev.prev.next = pred;
            size--;
        }
        
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

+ ## Leetcode 206: Reverse Linked List

#### problem link: [https://leetcode.com/problems/design-linked-list/](https://leetcode.com/problems/reverse-linked-list/description/)

#### Methods: 2 method

##### method 1: brtual force

Note: use arraylist record every node values in linked list then build another new reversed linked list.

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
    public ListNode reverseList(ListNode head) {
        if(head == null){
            return null;
        }
        List<Integer> values = new ArrayList<>();
        ListNode currentNode = head;
        while(currentNode != null){
            values.add(currentNode.val);
            currentNode = currentNode.next;
        }
        int size = values.size();
        ListNode newNode = new ListNode(values.get(size - 1));
        currentNode = newNode;
        for(int i = 0; i < size - 1; i++){
            currentNode.next = new ListNode(values.get(size - 2 - i));
            currentNode = currentNode.next;
        }
        /* another delicate way
        ListNode newHead = new ListNode(values.get(values.size() - 1));
        ListNode curr = newHead;
        for (int i = values.size() - 2; i >= 0; i--) {
            ListNode newNode = new ListNode(values.get(i));
            curr.next = newNode;
            curr = curr.next;
        }
        */
        return newNode;
    }
}
```

##### method 2: double pointers

Note: use TEMP to record the value missed. pre set to NULL(do not use new ListNode(), set to 0 otherwise)

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
    public ListNode reverseList(ListNode head) {
        //two pointers
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            //temp recorder
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```

##### method 3: recursive function

Note: understand two pointers first. it is based on that. It automatically updates the previous list node and current list node by recusive functions.

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
    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }

    //recusion function
    private ListNode reverse(ListNode prev, ListNode cur){
        if(cur == null){
            // it is the brake for this recursive function, do not return NULL(wrong).
            return prev;
        }
        // temp recorder
        ListNode next = null;
        next = cur.next;
        // point for reverse
        cur.next = prev;
        // update prev、cur position
        // prev = cur;
        // cur = next;
        return reverse(cur, next);
    }
}
```

