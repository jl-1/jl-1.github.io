---
layout: post
title:  "Geometry Problem with Binary Search"
---

[Link to the problem](https://leetcode-cn.com/problems/zui-xiao-ju-xing-mian-ji/)

- The first impression was some convex hull problem but then realized that the edges of the enclosed rectangle are all parallel to x or y axes. 
- If the enclosed rectange is correct, the cross-points between lines and the four edges of the rectange are all monotonic! Therefore we can binary search for the result.
- The idea was simple, but the implementation has many pitfalls. 
    - ***For those kind of implementation heavy problems, make the code clean and concise. Don't dive into implementation before you have a very clean picture of what you are doing.*** 
    - ***It's generally a very bad idea to figure out details while implementing, especially for hard problems.*** 
    - ***At least have formulas/equations/sketches written down somewhere.***
- First sort all the lines according to k. Note that here k has limited range, so we don't need to worry about lines parallel to x or y.
- Before doing binary search, calculate the lo and hi values, and epsilon.
    - Avoid using extreme small/large values. It's convenient, but for tight time limit, this could lead to TLE.
    - In this problem, x and y have different min/max values.
- Be careful about the cases where ***Two lines with same k***! The order of the lines depend on b differently for $$ x_{min}, x_{max}, y_{min}, y_{max} $$
- Be careful about the cases where the binary search could not find answer (when crossing points have only one x or y axis value).
    - ***when doing binary serach, always keep in mind that you may not find an answer***
    - In this case, output 0 when $$ x_{min} > x_{max} $$ or $$ y_{min} > y_{max} $$
- For floating point binary search, to prevent TLE, ***we could manually set a limit for the number of iterations!***
