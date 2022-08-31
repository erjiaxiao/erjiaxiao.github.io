---
title: Combination Sum II
date: 2022-02-11 09:00:00
categories: Algorithm
---

### Problem 

<https://leetcode.cn/problems/combination-sum-ii/>

### Gems 

- Backtracking
- Avoid repeated combinations by utilizing indices of the candidate array. It's a trick to remove repetition on the fly rather than using a set which is time-consuming and space-consuming.
- Define a function within a function, which relieves us of the burden that whenever we define or call a function in the class, we have to write the keyword ``` self ```. Also, it's reasonable to do this trick since ``` helper() ``` is an auxiliary function for ``` combinationSum2() ```. Finally, all of these help us clarify the code logic and focus on algorithm design by saving more brain working memory when thinking.
