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
### 1792. Maximum Average Pass Ratio
[Leetcode link](https://leetcode.com/problems/maximum-average-pass-ratio/?envType=daily-question&envId=2024-12-15)
<br>
There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array classes, where classes[i] = [passi, totali]. You know beforehand that in the ith class, there are totali total students, but only passi number of students will pass the exam. You are also given an integer extraStudents. There are another extraStudents brilliant students that are guaranteed to pass the exam of any class they are assigned to. You want to assign each of the extraStudents students to a class in a way that maximizes the average pass ratio across all the classes. The pass ratio of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The average pass ratio is the sum of pass ratios of all the classes divided by the number of the classes. Return the maximum possible average pass ratio after assigning the extraStudents students. Answers within 10-5 of the actual answer will be accepted.

Example 1:
Input: classes = [[1,2],[3,5],[2,2]], extraStudents = 2
Output: 0.78333
Explanation: You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.

Example 2:
Input: classes = [[2,4],[3,9],[4,5],[2,10]], extraStudents = 4
Output: 0.53485

Constraints:
1 <= classes.length <= 105
classes[i].length == 2
1 <= passi <= totali <= 105
1 <= extraStudents <= 105

```java
class Solution {
    public double find(double a,double b)
    {
        double d1 = (double)a/b;
        double d2 = (double)(a+1)/(b+1);
        return (d2-d1);
    }
    public double maxAverageRatio(int[][] classes, int extraStudents) {
        PriorityQueue<double[]> pq = new PriorityQueue<>((a,b)->Double.compare(b[2],a[2]));
        double ans = 0;
        for(int i=0;i<classes.length;i++)
        {
            pq.add(new double[]{classes[i][0],classes[i][1],find(classes[i][0],classes[i][1])});
        }
        while(extraStudents>0)
        {
            double[] arr = pq.poll();
            pq.add(new double[]{arr[0]+1.0,arr[1]+1.0,find(arr[0]+1.0,arr[1]+1.0)});
            extraStudents--;
        } 
        while(!pq.isEmpty())
        {
            double[] arr = pq.poll();
            ans+=(arr[0]/arr[1]);
        }
        return ans/classes.length;
    }
}
```
- The question say that add the extra students to any one of the class we get the maximum pass ratio.(Pass ration is defined above).
- So The idea behind is first we take all the maximum gain that is the old pass ratio and after adding one students the pass ratio find the difference between them give the gain.
- We always take the maximum gain so we increase our pass ratio.
- So in the queue push the first element second element and the gain as a array.
- Then take the top element beacause that give the maximum gain.
- Then push the changed value that is plus one value and find the gain for the plus one value and push to the queue.
- Do the same thing for the all extra students then find the pass ratio and add to the ans.
- Then return the ans by its length of the class array that is total number of class.
> [REference](https://www.youtube.com/watch?v=Oxry5tB0UQM)
### 3264. Final Array State After K Multiplication Operations I
[Leetcode link](https://leetcode.com/problems/final-array-state-after-k-multiplication-operations-i/?envType=daily-question&envId=2024-12-16)
<br>
You are given an integer array nums, an integer k, and an integer multiplier. You need to perform k operations on nums. In each operation: Find the minimum value x in nums. If there are multiple occurrences of the minimum value, select the one that appears first. Replace the selected minimum value x with x * multiplier. Return an integer array denoting the final state of nums after performing all k operations.

Example 1:
Input: nums = [2,1,3,5,6], k = 5, multiplier = 2
Output: [8,4,6,5,6]

Explanation:
Operation	Result
After operation 1	[2, 2, 3, 5, 6]
After operation 2	[4, 2, 3, 5, 6]
After operation 3	[4, 4, 3, 5, 6]
After operation 4	[4, 4, 6, 5, 6]
After operation 5	[8, 4, 6, 5, 6]

Example 2:
Input: nums = [1,2], k = 3, multiplier = 4
Output: [16,8]
Explanation:
Operation	Result
After operation 1	[4, 2]
After operation 2	[4, 8]
After operation 3	[16, 8]

Constraints:
1 <= nums.length <= 100
1 <= nums[i] <= 100
1 <= k <= 10
1 <= multiplier <= 5

```js
class Solution {
    public int[] getFinalState(int[] nums, int k, int multiplier) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b)->{
            if(a[0] == b[0]) return a[1] - b[1];
            else return a[0] - b[0];
        });
        for(int i=0;i<nums.length;i++)
        {
            pq.add(new int[]{nums[i],i});
        }
        while(k>0)
        {
            int arr[] = pq.poll();
            arr[0] = arr[0]*multiplier;
            pq.add(arr);
            k--;
        }
        while(!pq.isEmpty())
        {
            int[] arr = pq.poll();
            nums[arr[1]] = arr[0];
        }
        return nums;
    }
}
```
- In this code we use the priority queue to get the min element each time and multiply by the multiplier then add to the pq.
- Then we will put the pq's index with the corrosponding value.
