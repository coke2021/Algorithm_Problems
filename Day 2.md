# Day 2 Leetcode problem

Includes 97, 209, 59, (904, 76)

+ ## Leetcode 977 Squares of a Sorted Array

#### problem link: https://leetcode.com/problems/squares-of-a-sorted-array/

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
Note: + First while loop, the right pointer go first find the minimal length of numbers that sum up equal to or larger than the target.
      - In the first while loop, there are another while loop. The left pointer keep incrementing by 1 utill the sum is less than the target. Meanwhile, the sum minus by the the number in the array which passed by the left pointer. At the same time, record the minimal length. 
      * sliding window technique where two pointers are used to maintain a subarray and the subarray is expanded or contracted based on the sum.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int minLen = Integer.MAX_VALUE;
        int n = nums.length;
        //right pointer
        int p1 = 0;
        //left pointer
        int p2= 0;
        int sum = 0;
        while(p1 < n && p2 < n){
            sum += nums[p1];
            while(sum >= target){
                minLen = Math.min(minLen, p1 - p2 + 1);
                sum -= nums[p2];
                p2++;
            }
            p1++;
        }
        if(minLen == Integer.MAX_VALUE){
            return 0;
        } else {
            return minLen;
        }
    }
}
```

+ ## Leetcode 59 Spiral Matrix II

#### problem link: https://leetcode.com/problems/spiral-matrix-ii/

#### Methods: 1 normal solution and 1 optimal solution

##### normal solution:

Note: Traverse through the 2d array with not same rule. Easy to make mistakes.

```
class Solution {
    public int[][] generateMatrix(int n) {
        if(n == 1){
            int[][] res = new int[1][1];
            res[0][0] = 1;
            return res;
        }
        int[][] res = new int[n][n];
        int num = 1;
        int rowStart = 0;
        int rowEnd = n - 1;
        int colStart = 0;
        int colEnd = n - 1;
        int count = 0;
        while (count < n * n) {
            for (int j = colStart; j <= colEnd; j++) {
                res[rowStart][j] = num++;
                count++;
            }
            rowStart++;
            for (int i = rowStart; i <= rowEnd; i++) {
                res[i][colEnd] = num++;
                count++;
            }
            colEnd--;    
            for (int j = colEnd; j >= colStart; j--) {
                    res[rowEnd][j] = num++;
                    count++;
            }
                rowEnd--;
            for (int i = rowEnd; i >= rowStart; i--) {
                    res[i][colStart] = num++;
                    count++;
            }
                colStart++;
        }
        return res;
        
    }
}
```


##### optimal solution:

Note: Fixed rule along four side! Traverse through the 2d array with same and fixed rule. To fill the array by 4 sides. In each side, it starts at first element and ends at the element before the last element. So, it left the last element in the the four inner for loops.

```
class Solution {
    public int[][] generateMatrix(int n) {
        if(n == 1){
            int[][] res = new int[1][1];
            res[0][0] = 1;
            return res;
        }
        //result 2d array
        int[][] res = new int[n][n];
        //number elements filled
        int num = 1;
        //start position for each loop
        int startRow = 0;
        int startCol = 0;
        // offset control the length of each side 
        int offset = 1;
        // last element in odd matrix
        if(n % 2 == 1){
            res[n/2][n/2] = n * n;
        }
        int counter = 0;
        int j;
        int i;
        
        while (counter < n/2) {
            for (j = startCol; j < n - offset; j++) {
                res[startRow][j] = num++;
            }
            for (i = startRow; i < n - offset; i++) {
                res[i][j] = num++;
            }
            
            for (; j > startCol; j--) {
                    res[i][j] = num++;
            }
            for (; i > startRow; i--) {
                    res[i][startCol] = num++;
            }
            startRow++;
            startCol++;
            offset++;
            counter++;
        }
        return res;
    }
}
```
