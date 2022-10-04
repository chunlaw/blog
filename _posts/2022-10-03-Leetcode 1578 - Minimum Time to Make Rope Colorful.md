---
title: Leetcode 1578 - Minimum Time to Make Rope Colorful
date: 2022-10-03 22:15:38 +0800
categories: [Leetcode, Medium]
tags: [dynamic programming, typescript]     # TAG names should always be lowercase
---

The [question](https://leetcode.com/problems/minimum-time-to-make-rope-colorful/) is obviously a dynamic programming question. By defining an array of minimum time needed to make the ballon sequence end with a specific colour (i.e., the array index). The algorithm is straightforward by the data structure definition.

```typescript
function minCost(colors: string, neededTime: number[]): number {
  const cost = Array(27).fill(100000000)
  cost[0] = 0
  for ( let i=0;i<colors.length;++i ) {
    let c = colors.charCodeAt(i) - 96
    let min = 100000000
    for ( let j=0;j<27;++j ) {
      if ( c !== j ) {
        min = Math.min(min, cost[j])
        cost[j] += neededTime[i]
      }
    }
    cost[c] = Math.min(min, cost[c] + neededTime[i])
  }
  
  return cost.reduce((acc, cur) => Math.min(acc, cur), cost[0])
};
```

