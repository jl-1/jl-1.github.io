---
layout: post
title:  "A Digit Counting DP Problem"
---

[Codeforces Round #287 (Div. 2) D. The Maths Lecture](https://codeforces.com/problemset/problem/507/D)

<br/>

It's easy to tell that this is a counting DP problem, but difficult to figure out all details correctly.

<br/>

Count the number $$ x $$ that:
1. contains n digits (no leading zero, first digit can't be 0)
2. has a suffix $$ y $$ that $$ y \% k = 0 $$
3. y can't be 0

The requirement 3 is a pain for DP implementation.
<br/>

State transfer function can be written as:

$$ f(i, j) = \sum_{d=1}^{9} f(i - 1, (j - 10^j\ \%\ k)\ \%\ k) \  + \ ((j - 10^j\ \%\ k)\ \%\ k == 0) $$

And the result is

$$ ret=f(n, 0) + \sum_{i=1}^{n - 1}10^{n-i-1}\cdot9\cdot f(i, 0)\ \%\ mod  $$

And this is a pain to get right. Be very careful about a few things:
- $$ f(i, j) $$ means the count of distint numbers with i digits and a postfix with remainder j. It doesn't have any postfix with 0 remainder.
    - When next (the i - 1) remainder is 0, don't count since we don't want any 0 postfix
    - But do count one case where the rest are all 0
- If i is not the first digit, we count 0. If i is the first digit, we don't count 0.
