---
tags: 
title: Improving Pipelining
date: 2024-12-13 15:07
type: permanent-note
---
---
Loading data from memory (even the L1 cache) into registers takes some CPU cycles. We have several loads in the update loop. Instead of waiting for all of them to complete we could try to do some useful work between the load calls.

This is another step to discover the brilliancy of the BLIS micro kernel!

## The Micro Kernel Algorithm
We make 6 modifications:

- Before we enter the update loop and before zero initializing the $\mathbb{AB}$  registers we already load
    
    - $(a_{0}, a_{1})$  into $\mathbb{tmp}_0$ ,
        
    - $(a_{2}, a_{3})$  into $\mathbb{tmp}_1$  and
        
    - $(b_{0}, b_{1})$  into $\mathbb{tmp}_2$.
        
- Reload $\mathbb{tmp}_2$ with $(b_{4l+4}, b_{4l+5})$ at about the middle of the loop.
    
- Reload $\mathbb{tmp}_0$ with $(a_{4l+4}, a_{4l+5})$ and $\mathbb{tmp}_1$ with $\mathbb{tmp}_2$ with $(a_{4l+6}, a_{4l+7})$ at about the end of the loop (with an SSE multiplication in between and previous to two SSE additions).