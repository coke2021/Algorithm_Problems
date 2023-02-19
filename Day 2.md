# Day 2 Leetcode problem

Includes 97, 209, 59

+ ## Leetcode 977 Squares of a Sorted Array

#### problem link: https://leetcode.com/problems/squares-of-a-sorted-array/
Credit to: 

#### Methods: 1 brutal force and 1 optimal solution

##### Brutal force:

Note: firstly, make the number in the array squared and sort them.
```
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            nums[i] = (int) Math.pow(nums[i], 2);
        }
        Arrays.sort(nums);
        return nums;
    }
}
```

##### Optimal solution

Note: use two pointers. Pointer 1 starts at 0 and pointer 2 starts at the end of array. The reason is the sorted array which includes negatives usually has the largest number at two side of array.

```
class Solution {
    public int[] sortedSquares(int[] nums) {
        //pointer 1 at the start
        int p1 = 0;
        //pointer 2 at end
        int p2 = nums.length - 1;
        //pointer for storage for result in new array
        int p3 = nums.length - 1;
        //new array for result
        int[] result = new int[nums.length];
        // why p1 = p2? have to store the last number in the array. At that time, p1 = p2
        while(p1 <= p2){
            int s1 = nums[p1] * nums[p1];
            int s2 = nums[p2] * nums[p2];
            if(s1 <= s2){
                result[p3] = s2;
                p2--;
                p3--;
            } else {
                result[p3] = s1;
                p1++;
                p3--;
            }
        }
        return result;
    }
}
```

+ ## Leetcode 209 Minimum Size Subarray Sum

#### problem link: https://leetcode.com/problems/minimum-size-subarray-sum/description/

#### Methods: 1 brutal force and 1 optimal solution

##### Brutal force:

Note: first two loop go through every subarray in the array 'nums'. Then the inner loop add up the numbers in each subarray. Then compare the sum to target and minimal length found now to new minLen.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int minLen = Integer.MAX_VALUE;
        int n = nums.length;
        int sum = 0;
        for(int i = 0; i < n; i++){
            for(int j = i; j < n; j++){
                for(int k = i; k <= j; k++){
                    sum += nums[k];
                }
                if(sum >= target && minLen > j - i + 1){
                    minLen = j - i + 1;
                }
                sum = 0;
            }
        }
        if(minLen == Integer.MAX_VALUE){
            return 0;
        } else {
            return minLen;
        }
    }
}
```

##### Optimal solution

Note: 
