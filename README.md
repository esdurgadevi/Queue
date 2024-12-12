# Queue
### 2530. Maximal Score After Applying K Operations
[Leetcode link](https://leetcode.com/problems/maximal-score-after-applying-k-operations/description/?envType=daily-question&envId=2024-10-14)
<br>
You are given a 0-indexed integer array nums and an integer k. You have a starting score of 0. In one operation: choose an index i such that 0 <= i < nums.length, increase your score by nums[i], and replace nums[i] with ceil(nums[i] / 3).
Return the maximum possible score you can attain after applying exactly k operations. The ceiling function ceil(val) is the least integer greater than or equal to val.

Example 1:
Input: nums = [10,10,10,10,10], k = 5
Output: 50
Explanation: Apply the operation to each array element exactly once. The final score is 10 + 10 + 10 + 10 + 10 = 50.

Example 2:
Input: nums = [1,10,3,3,3], k = 3
Output: 17
Explanation: You can do the following operations:
Operation 1: Select i = 1, so nums becomes [1,4,3,3,3]. Your score increases by 10.
Operation 2: Select i = 1, so nums becomes [1,2,3,3,3]. Your score increases by 4.
Operation 3: Select i = 2, so nums becomes [1,1,1,3,3]. Your score increases by 3.
The final score is 10 + 4 + 3 = 17.

Constraints:
1 <= nums.length, k <= 105
1 <= nums[i] <= 109

```java
class Solution {
    public long maxKelements(int[] nums, int k) {
        PriorityQueue<Integer> q = new PriorityQueue<>(Collections.reverseOrder());
        for(int i = 0;i<nums.length;i++)
        {
            q.add(nums[i]);
        }
        long sum = 0;
        while(!q.isEmpty() && k>0)
        {
            sum+=q.peek();
            int x = (int)Math.ceil(q.poll()/3.0);
            q.add(x);
            k--;
        }
        return sum;
    }
}
```
- In this problem we add the maximum element from the queue and added to the sum.
- In every case the max will divide by three and added to the queue.
### 2558. Take Gifts From the Richest Pile
[Leetcode link](https://leetcode.com/problems/take-gifts-from-the-richest-pile/?envType=daily-question&envId=2024-12-12)
<br>
You are given an integer array gifts denoting the number of gifts in various piles. Every second, you do the following: Choose the pile with the maximum number of gifts. If there is more than one pile with the maximum number of gifts, choose any. Leave behind the floor of the square root of the number of gifts in the pile. Take the rest of the gifts. Return the number of gifts remaining after k seconds.

Example 1:
Input: gifts = [25,64,9,4,100], k = 4
Output: 29
Explanation: 
The gifts are taken in the following way:
- In the first second, the last pile is chosen and 10 gifts are left behind.
- Then the second pile is chosen and 8 gifts are left behind.
- After that the first pile is chosen and 5 gifts are left behind.
- Finally, the last pile is chosen again and 3 gifts are left behind.
The final remaining gifts are [5,8,9,4,3], so the total number of gifts remaining is 29.

Example 2:
Input: gifts = [1,1,1,1], k = 4
Output: 4
Explanation: 
In this case, regardless which pile you choose, you have to leave behind 1 gift in each pile. 
That is, you can't take any pile with you. 
So, the total gifts remaining are 4.

Constraints:
1 <= gifts.length <= 103
1 <= gifts[i] <= 109
1 <= k <= 103

```java
class Solution {
    public long pickGifts(int[] gifts, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        long sum = 0;
        for(int i=0;i<gifts.length;i++)
        {
            pq.add(gifts[i]);
            sum+=gifts[i];
        }
        while(k>0)
        {
            int element = pq.poll();
            sum-=element;
            int sqrt = (int)Math.sqrt(element);
            pq.add(sqrt);
            sum+=sqrt;
            k--;
        }
        return sum;
    }
}
```
- In this code for each second we take the maximum numbered gifts and left the sqrt of that gift. For finding the maximum we use priority queue for this and also find the sum.
- Then Before the k become zero get the maximum element from the queue and remove the element from the sum find the sqrt and put the sqrt in the queue as well as the sum finally return the sum.
