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
```


# ✅ PROBLEM 2
🔗 Leetcode 1423. Maximum Points You Can Obtain from Cards

*✅ Problem Recap:*
You are given an integer array cardPoints and an integer k. You can take k cards from either the beginning or the end of the array. Your goal is to maximize the total points obtained by picking exactly k cards.

*🧠 Step-by-Step Thought Process:*
*🧠 Recognize the Pattern:*
This is a variation of a prefix + suffix or sliding window problem in disguise.

The twist is that you can pick cards only from the two ends — either from the start (left) or the end (right) — and you must pick exactly k cards.

*🧠 My Explanation of the Approach:*
Initial Observation:

You can pick cards in any combination from both ends, as long as the total count is k.

So the valid combinations include:

All from left (first k cards)

1 from right + (k-1) from left

2 from right + (k-2) from left

...

All from right (last k cards)

Start with the sum of the first k cards from the left. This is your initial maxSum.

Iterate backward from the left and add cards from the right:

In each step, remove one card from the left (lsum -= cardPoints[i])

Add one card from the right (rsum += cardPoints[rindex])

Update the maxSum by comparing lsum + rsum.

Why this works:

It simulates all valid combinations of taking cards from left and right totaling k cards.

🧪 Let’s Solve One Example:
Input:

cardPoints = [1, 2, 3, 4, 5, 6, 1], k = 3

Start with sum of first k = 3 elements → lsum = 1 + 2 + 3 = 6

Try taking some cards from the right:

Take 1 from right (1), remove last from left (3) → new sum = 1 + 2 + 1 = 4

Take 2 from right (6 + 1), remove 2 from left (2 + 3) → sum = 1 + 6 + 1 = 8

Take 3 from right (5 + 6 + 1) → sum = 12

Track the maximum at each step → Final maxSum = 12

*⚠️ Key Tips to Remember:*
Always ensure total picked cards = k

Don't just add from both ends, but remove from one side to maintain k total

Try to reuse previous calculations (e.g., prefix sum) to save time

```java
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int lsum =0, rsum = 0, maxSum =0, n = cardPoints.length;
        for(int i  =0; i < k ; i++){
            lsum = lsum + cardPoints[i];
            maxSum = lsum;
        }
        int rindex = n - 1;
        for(int i = k-1 ; i >= 0; i--){
            lsum = lsum - cardPoints[i];
            rsum = rsum + cardPoints [rindex];
            rindex = rindex-1;
            maxSum = Math.max(maxSum, lsum+rsum);

            }
        return maxSum;
    }
}
```
# ✅ PROBLEM 3
🔗 Leetcode 3. Longest Substring Without Repeating Characters

*✅ Problem Recap:*
Given a string s, find the length of the longest substring without repeating characters.

*🧠 Step-by-Step Thought Process:*
🔍 Recognize the Pattern:
This is a Sliding Window + Two Pointers problem.

You're asked to find the longest substring with unique characters — meaning no repeats allowed in the window.

*🧠 My Explanation of the Sliding Window Logic:*
Goal:
Maintain a dynamic window [l, r] that always contains unique characters.

How to do that?
Use a HashSet to keep track of characters inside the current window.

Start with l = 0 and r = 0 (both at beginning), and an empty set.

Expand the window (move r) as long as characters are unique:

If s.charAt(r) is not in the set, add it and update maxLen.

When a duplicate character is found:

You must shrink the window from the left by moving l++ and removing s.charAt(l) from the set until the duplicate is removed.

Keep doing this while r < s.length().

🧪 Let’s Solve One Example:
Input:

s = "abcabcbb"

*Execution:*

Add 'a' → set = [a] → maxLen = 1

Add 'b' → set = [a,b] → maxLen = 2

Add 'c' → set = [a,b,c] → maxLen = 3

See duplicate 'a' again → remove 'a' from left → set = [b,c] → now add 'a' → set = [b,c,a]

Repeat until done

Final answer: maxLen = 3 (substring: "abc")

*⚠️ Important Tips to Remember:*
A Set is used to ensure uniqueness of characters.

Shrinking the window only happens when a duplicate is found.

Preserving the order is automatic since we use a window and don’t reorder characters..

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int k = s.length();
        int l =0, r=0, maxLen =0;
        while(r<k){
            if(!set.contains(s.charAt(r))){
                 set.add(s.charAt(r));
                maxLen = Math.max(maxLen, r-l+1);
                r++;
            } else{
                set.remove(s.charAt(l));
                l++;
            }
        }
        return maxLen;   
    }
}
```
