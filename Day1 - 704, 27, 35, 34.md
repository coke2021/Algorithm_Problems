# Day 1 Leetcode problem

Includes 704, 27, 34, 35

+ ## Leetcode 704 binary search

#### problem link: https://leetcode.com/problems/binary-search/
Credit to: https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#_704-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE

#### Methods: 1 brutal force and 2 optimal solution

##### Brutal force:
Note: simply traverse through the array "nums" and find the target.
```
class Solution {
    public int search(int[] nums, int target) {
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == target){
                return i;
            }
        }
        return -1;
    }
}
```

##### Optimal solution
It's important to choose between [left, right] and [left, right) before solving the problem.
Note: Let the middle number of the array "nums" compare to the target. This methods let the new modified array includes the most left number and the most right number, which is [left, right].
```
class Solution {
    public int search(int[] nums, int target) {
        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        int left = 0;
        int right = nums.length - 1;
        // when left==right，the interval [left, right] is still valid.
        while(left <= right){
            int middle = left + (right - left) / 2;
            if(target < nums[middle]){
                right = middle - 1;
            } else if (target > nums[middle]){
                left = middle + 1;
            } else if (target == nums[middle]){
                return middle;
            }
        }
        return -1;
    }
}
```

Note: Let the middle number of the array "nums" compare to the target. This methods let the new modified array includes the most left number and the number that is greater than the most right number in the range, which is [left, right).
```
class Solution {
    public int search(int[] nums, int target) {
        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        int left = 0;
        int right = nums.length;
        // when left==right，the interval [left, right) is not valid.
        while(left < right){
            int middle = left + (right - left) / 2;
            if(target < nums[middle]){
                right = middle;
            } else if (target > nums[middle]){
                left = middle + 1;
            } else if (target == nums[middle]){
                return middle;
            }
        }
        return -1;
    }
}
```

+ ## Leetcode 27 Remove Element

#### problem link: https://leetcode.com/problems/remove-element/description/
Credit to: https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html#%E6%80%9D%E8%B7%AF

#### Methods: 1 brutal force and 1 optimal solution

##### Brutal force:
Note: It's brutal force with double for loops.
Time complexity: O(n^2)
Space complexity: O(1)
```
class Solution {
    public int removeElement(int[] nums, int val) {
        int length = nums.length;
        for(int i = 0; i < length; i++){
            //if find tbe 
            if(nums[i] == val){
                for(int j = i; j < length - 1; j++){
                    nums[j] = nums[j + 1];
                }
                length--;
                i--;
            }
        }
        return length;
    }
}
```

##### Optimal solution
Note: There are double pointer (often used in Array, Linked list, String). The fast pointer traverse through the array and check if the travered number is equal to the target value. Otherwise, the slow pointer increments when the fast pointer meet the target and store the number not equal to the target in the array, which means the target value will be skipped.
```
class Solution {
    public int removeElement(int[] nums, int val) {
        int slowPointer = 0;
        for(int fastPointer = 0; fastPointer < nums.length; fastPointer++){
            if(nums[fastPointer] != val){
                nums[slowPointer] = nums[fastPointer];
                slowPointer++;
            }
        }
        return slowPointer;
    }
}
```

+ ## Leetcode 34 Find First and Last Position of Element in Sorted Array

#### problem link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
Credit to: https://programmercarl.com/0034.%E5%9C%A8%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E6%9F%A5%E6%89%BE%E5%85%83%E7%B4%A0%E7%9A%84%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%92%8C%E6%9C%80%E5%90%8E%E4%B8%80%E4%B8%AA%E4%BD%8D%E7%BD%AE.html#%E6%80%9D%E8%B7%AF

#### Methods: waited to be done:

+ ## Leetcode 35 Find First and Last Position of Element in Sorted Array

#### problem link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
Credit to: https://programmercarl.com/0035.%E6%90%9C%E7%B4%A2%E6%8F%92%E5%85%A5%E4%BD%8D%E7%BD%AE.html

#### Methods: 1 brutal force and 2 optimal solution

##### Brutal force:
Note: Simply traverse through the sorted array and check the right position to be placed
```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int pos = 0;
        if(target < nums[0]){
            pos = 0;
        } else if (target > nums[nums.length - 1]){
            pos = nums.length;
        } else {
            for(int i = 0; i < nums.length; i++){
                if(nums[i] == target){
                    pos = i;
                } else if(target > nums[i] && target < nums[i + 1]){
                    pos = i + 1;
                }
            }
        }
        return pos;
    }
}
```

##### Optimal solution
Note:Binary search - include left & right

```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int pos;
        int left = 0;
        int right = nums.length - 1;
        if (target < nums[0]) {
            return pos = 0;
        } else if (target > nums[nums.length - 1]) {
            return pos = nums.length;
        } else {
            while (left <= right){
                int middle = left + (right - left) / 2;
                if(target < nums[middle]){
                    right = middle - 1;
                } else if (target > nums[middle]){
                    left = middle + 1;
                } else if (target == nums[middle]){
                    return pos = middle;
                }
            }
            return pos = left;
        }
        
    }
}
```

Note: Binary search - include left and exclude right
```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        if (target < nums[0]) {
            return 0;
        } else if (target > nums[nums.length - 1]) {
            return nums.length;
        } else {
            while (left < right){
                int middle = left + (right - left) / 2;
                if(target < nums[middle]){
                    right = middle;
                } else if (target > nums[middle]){
                    left = middle + 1;
                } else if (target == nums[middle]){
                    return middle;
                }
            }
            return left;
        }
    }
}
```
