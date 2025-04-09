# 🔄 Two Pointers & Sliding Window: How They're Interrelated
Both patterns are often used together in problems involving arrays and strings. While two pointers generally refer to using two variables to traverse or compare elements, sliding window applies this idea in a more structured way—especially useful for subarray/substring problems.
🧠 Once you understand the two pointers technique, you’ll see how sliding window is simply a specialized use-case where the pointers move with specific window rules (size-fixed or condition-based).

# there are only four types of problems that you will see in this topic from all over leetcode.
1️⃣ Constant Window Size
Useful for problems where the window size is fixed.
📘 Example: Maximum Points You Can Obtain from Cards(https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/)

2️⃣ Maximum Subarray / Substring Type Questions
Dynamic window size that grows or shrinks based on a condition.
📘 Example: Longest Substring Without Repeating Characters(https://leetcode.com/problems/longest-substring-without-repeating-characters/)

3️⃣ Number of Subarrays That Meet a Condition
Count how many subarrays satisfy a given condition (sum, product, etc.)
📘 Example: Subarrays with Product Less than K(https://leetcode.com/problems/subarray-product-less-than-k/description/)

4️⃣ Minimum Substring That Meets a Condition
Shrink window until it just satisfies the condition (commonly used in string problems).
📘 Example: Minimum Window Substring(https://leetcode.com/problems/minimum-window-substring/description/)

📸 Attaching My Handwritten Notes
These notes explain these 4 templates and how to approach any question related to Sliding Window & Two Pointers effectively.

<div style="display: flex; flex-wrap: wrap; gap: 10px;"> <img src="https://github.com/user-attachments/assets/62dd5c5f-b7d8-4a17-a417-f14364067579" width="300"/> <img src="https://github.com/user-attachments/assets/22238782-57ff-45c3-922a-807757c7fee4" width="300"/> <img src="https://github.com/user-attachments/assets/1091faf7-0908-41c5-898c-a40ee6f18032" width="300"/> <img src="https://github.com/user-attachments/assets/52a95a95-9f81-43fc-9e6b-2b45c17e514d" width="300"/> <img src="https://github.com/user-attachments/assets/e33ae3cb-e06e-4a5e-a9dd-f1b97f697356" width="300"/> <img src="https://github.com/user-attachments/assets/eb754b68-3801-409e-8c74-a176f084a271" width="300"/> </div>

✅ With these 4 templates and visual notes, you'll be able to recognize the right pattern quickly and solve problems more efficiently—step by step from brute → better → optimal.

# PROBLEM 1 ->
https://leetcode.com/problems/max-consecutive-ones-iii/description/

THAUGHT PROCESS:
*✅ Problem Recap:*
"Max Consecutive Ones III"
Given a binary array nums, and an integer k, return the maximum number of consecutive 1s in the array if you can flip at most k 0s.

*🧠 Step-by-Step Thought Process*
1. Recognize the Pattern
This is a sliding window + two pointers problem:

*🧠 My Explanation of the Sliding Window Logic*{SEE THE CODE WITH EXPLAINATION}
When iterating through the array:

If nums[r] == 1:
There’s no issue — we don’t need to flip anything.
So, we just update the maxLen and move the right pointer r++.

If nums[r] == 0:
We have a choice to flip this 0 to 1.
So, we check:
If flip < k:
That means we’re still within our allowed number of flips.
We increment flip++, then move r++.

If flip == k:
Our flipping threshold is reached. We can’t flip more than k zeros.

When flip == k and we encounter another 0:
Now we need to shrink the window from the left until we make space for this new flip.

We move the left pointer l++
While shrinking, if the value we remove (nums[l]) was a flipped 0, we must decrease flip--
This helps adjust our flip count so we stay within the limit.
By maintaining this sliding window with at most k flipped zeros, we continuously update maxLen to track the longest valid window.

*Important tip*
Be careful with:
Flip only when needed
Flip only undone when you move past a 0
Don't blindly flip or shrink without checking what nums[l] is

*🧪 Let’s Solve One Example*
Input:
nums = [1, 1, 0, 1, 0, 1, 0]
k = 2

Step-by-Step Table:
![image](https://github.com/user-attachments/assets/e1eb2332-6d08-427c-b94f-08b864bfc8a2)

→ Now flip > k, start shrinking:
nums[0] == 1 → move l++ → no flip--
nums[1] == 1 → move l++ → no flip--
nums[2] == 0 → move l++ → flip-- → flip becomes 2 ✅
Now flip = 2, valid again → continue growing
Final maxLen = 6

MY SOLUTION:
### Solution for 1004. Max Consecutive Ones III

```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        int maxLen = 0, l = 0, r = 0, len = 0, flip = 0;
        while (r < n) {
            if (nums[r] == 1) {
                maxLen = Math.max(maxLen, r - l + 1);
                r++;
            } else if (nums[r] == 0 && flip < k) {
                flip++;
                r++;
            } else if (nums[r] == 0 && flip >= k) {
                if (nums[l] == 0) {
                    flip--;
                }
                l++;
            }
        }
        return maxLen;
    }
}
