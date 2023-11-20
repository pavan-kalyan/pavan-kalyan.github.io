---
layout: post
title:  "Tips on DP + Bitmasking for competitive programming"
date:   2023-07-02 00:00:00 +0530
excerpt: ""
author: Pavan Kalyan Damalapati
tags: [cp]
---

I came across a few "HARD" problems on Leetcode and codeforces that all seemed to have a similar pattern. 
They utilized Dynamic Programming and Bitmasking.
After failing to solve them I finally decided to learn it properly.

Here are some tips I have learned on recognizing and solving these types of problems.


#### Recognizing when DP + Bitmasking is applicable:
The following patterns are a possible indication of DP + Bitmasking:
- When the constraints state that n <= 20.
- When the problem can be solved by backtracking and has some common states while backtracking.
- When the problem involves using subsets in some way or the other.

#### Contrast with Backtracking:
Every DP + Bitmasking problem can be represented in a backtracking recursion graph.
Every backtracking approach can be represented using DP + Bitmasking **if and only if**, the recursion graph has common states. This indicates overlapping subproblems.

#### Main Advantage:
If we can solve every DP bitmasking problem with a backtracking + memoization approach then why even bother with DP + bitmasking?
The main advantage of using DP + bitmasking is to save space. A backtracking recursion tree could have 2<sup>n</sup> states (assuming a state for each possible subset). You would need to store a current\_subset for each state to track which subset is currently being considered. That's potentially O(n) space for each state.

If we use bitmasking, we instead can represent this as an integer for every state. Space optimization from O(n) to O(1) per state.



#### Expected Complexities:
The time complexity for these problems seem to be around O(n * 2<sup>n</sup>).
The space complexity would be around O(2<sup>n</sup>).

#### Code Template:


~~~ cpp

// Let dp[mask] represent the answer when
// considering only the subset elements in mask
int dp[(1 << n)];


for (int mask = 0; i< (1 <<n); i++) {
    
    // If this subset satisfies the condition
    if (cond(mask) == true) {
        
        // iterate over all bits to see which could lead to this state
        for (int bitNo=0;bitNo<n;bitNo++) {
            if (mask | bitNo != mask) {
                // bit is not set
                continue;
            }
            // else bit is set and could indicate a previous state
            // mask ^ (1 << bitNo) indicates subset without the element at position bitNo
            int prevState = mask ^ (1 << bitNo);

            dp[mask] = transform(dp[prevState]); // transform() is based on the dp recurrence
        }
    }
}
cout << dp[(1 << n) -1] << endl;
~~~



### References

- [To Learn about DP + bitmasking with some problems](https://usaco.guide/gold/dp-bitmasks?lang=cpp)
