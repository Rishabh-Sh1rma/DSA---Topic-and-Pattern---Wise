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

Actual correct code

class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        int maxLen =0, l =0,r =0,flip = 0;
        while(r<n){
           if(nums[r] != 1){
            flip++; 
           }
            while(flip > k){
            if(nums[l] != 1){
                flip--;
            }
             l++;
            }
        maxLen = Math.max(maxLen, r-l+1);
        r++; 
        }
        return maxLen;
    }
}

// agar nums of r 0 h to mai flip ko badhadunga frr mai dekhunga ki flip bada h kya k s , agar haan to r++ ho jyga, agar naa to hum nums of l check krnge agar vo 1 h to simple l++ agar zero h matlab while loop k hisab s flip k ko exceed bhi kr gya aur nums[l] 0 bhi h to hamne vapas k ki range m aane k liye flip ko -- krna padega 

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

// q m bola h ki ya to left side s patte utha lo ya right s utha lo, array h yh jiska index bata rha h konsa card h aur value points bata rhi h ki vo card s kitne point milnge.
apna simple start kro left side s, matlab sare cards left s utha lo unko add kro to yh ek maxsum aagya initially jisko hum initial start m use krnge ab aage frr compare krte jynge nye nye sums s jo bada mila maxsum vo answer.
1. ititial m ek window bn gyi [l.....k] ki kyuki hmne l s k tak ke cards utha liye.
2. ab bs condition folow kro new cards bch s nhi uthane mtlb ab hum left se shrink nhi kr skte kyuki left s kiya to hamari na to starting left s bachegi na right se
ex dekhte h:
[5,6,8,10,4,7], k =3 to abhi maxsum kya hua 19. ab agar m left s shrink krdu to new window kya hogi [6,8,10] jo ki bch k elements h array ke lkin humko to sirf left side s start hone wale ya right s start hone wale chahiye.

3. ab hum k s shrink krnge matlab ki k p jo element hua uspe hum array k end s ek element daal k dekhnge frr maxsum nikalange agar zda aaya to maxsum update kr dnge, vrna nhi krnge.
isko repeat krnge aur final answer max points ka mil jyga.


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
// q m bol rha h without repeating characters to sedhe sehde hashset use krne ka. check kro set m elemet h agar nhi h to add krdo hai aur agar hai to mtlb ab add krnge to duplicate ho jyga na vo to nhi krna apan ko, to apan piche s shrink krna start kr dnge aur jab tak shrink krnge jab tak duplicate na ht jye.
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

# ✅ PROBLEM 4
🔗 GeeksforGeeks - Longest K Unique Characters Substring
https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853

✅ Problem Recap:
Given a string s and an integer k, find the length of the longest substring that contains exactly k unique characters.

If there is no such substring, return -1.

🧠 Step-by-Step Thought Process:
🔍 Recognize the Pattern:
This is a Sliding Window + HashMap problem.

You're asked to find the longest valid window that contains exactly k distinct characters.

🧠 My Explanation of the Sliding Window Logic:
Goal:
Maintain a window [l, r] with exactly k unique characters and keep track of the maximum window size that satisfies this condition.

Key Steps:
Use a HashMap to store the frequency of characters inside the window.

Expand the window (move r):

Add s[r] to the map and update its count.

When the map has more than k unique characters:

Shrink the window from the left (l++) until the number of keys in the map becomes <= k.

While shrinking, decrease the count of s[l] in the map.

If its count becomes 0, remove it from the map completely.

When map size is exactly k:

Update maxLen = max(maxLen, r - l + 1)

Continue until r < s.length()

🧪 Let's Solve One Example:
Input:

```java
s = "aabacbebebe", k = 3
```
Walkthrough:

Build up to "cbebebe" → This has exactly 3 unique characters: c, b, e

Length = 7 → this is the max valid window

Final output: 7

⚠️ Important Edge Case:
If no substring with exactly k unique characters is found, return -1. That's why maxLen is initialized as -1.

```java
//seedhe seehde hashmap k andr "k" s zda elements nhi hone chahiye agar zda h k s to shrink krna start krdo piche s jb tak ek ht na jye
class Solution {
    public int longestkSubstr(String s, int k) {
        // code here
        HashMap <Character, Integer> map  = new HashMap<>();
        int l =0,r=0,maxLen = -1; // to check foe no valid substring
        while(r<s.length()){
            char right  = s.charAt(r);
            // if(!map.containsKey(right)){ //not used if condition because if map conatins not not we have to put the element anyways and increase the count;
                map.put(right, map.getOrDefault(right,0)+1);
            // }
            // else{
                // map.put(right, map.get(right+1));
            // }
            while(map.size()>k){
                char left = s.charAt(l);
                map.put(left , map.get(left) - 1);
                if(map.get(left) == 0){
                    map.remove(left);
                    
                }
                l++;
            }
            if(map.size() == k){
                maxLen = Math.max(maxLen,r-l+1);
            }
            r++;
            
        }
        return maxLen;
        
    }
}
```
# 🟪 Problem 5 → Fruit Into Baskets
🔗 Leetcode 904. Fruit Into Baskets

✅ Problem Recap:
You're given an array fruits[], where each element represents a type of fruit on a tree.
You have two baskets, and you can pick fruits from a continuous row of trees such that you can only pick at most two types of fruits.

Your goal is to find the maximum number of fruits you can collect in a row while following these constraints.

🧠 Step-by-Step Thought Process (Sliding Window + HashMap):
This is a classic variable-size sliding window problem where:

Each fruit type is tracked in a HashMap.

You’re allowed to pick at most 2 types of fruits.

We expand the window from the right (r++) and shrink it from the left (l++) whenever the number of fruit types exceeds 2.

🍉 Sliding Window Logic:
Start with two pointers: l = 0, r = 0, and a HashMap to count fruit types.

For each fruits[r]:

Add it to the map and update the count.

If the map contains more than 2 types of fruits:

Shrink the window from the left (l++) until only 2 types remain.

While shrinking, decrease the count of fruits[l], and if it becomes zero, remove it from the map.

After adjusting the window, update maxLen to hold the length of the current valid window.

🧪 Let’s Solve an Example:
Input: fruits = [1,2,1,2,3]

Start with empty map.

Traverse and fill the map until size > 2:

[1,2,1,2] → valid window → maxLen = 4

Next 3 comes in → map size becomes 3.

Shrink from left: remove 1, then 2 until only two types remain → new window is [2,3]

Continue this process and always track max window with 2 types.

Output: maxLen = 4
```java
//isme hashmap ko maintain krna h exactly two type of elements hi hone chahiye usme to uska size 2 s bada hua to piche s window shrink krni h jab tak ek element hat na jye//
class Solution {
    public int totalFruit(int[] fruits) {
        HashMap <Integer,Integer> map  = new HashMap<>();
        int l =0,r=0,maxLen = 0;
        while(r<fruits.length){
                map.put(fruits[r], map.getOrDefault(fruits[r], 0) + 1);
            while(map.size()>2){
                map.put(fruits[l],map.get(fruits[l]) -1);
                if(map.get(fruits[l]) == 0){
                    map.remove(fruits[l]);
                }
                l++;
            }
            maxLen = Math.max(maxLen, r-l+1);
            r++;
        }
        return maxLen;
    }
}
```
