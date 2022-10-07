---
title: Leetcode 732 - My Calendar III
date: 2022-10-07 17:58:23 +0800
categories: [Leetcode, Hard]
tags: [binary search, hash table]     # TAG names should always be lowercase
math: true
---

The hard [question](https://leetcode.com/problems/my-calendar-iii/solution/) is not that hard. There is only 400 calls stated in **Constraints** and hence even $$ O(n^2 log{n}) $$ would be an acceptable solution.

The key is to maintain a sorted array of object `(time, +1 or -1)` such that `+1 / -1` denote the start and end of interval respectively. We then loop through the array to count the number of overlappoing intervals at any time point. Returning the maximum value would be a piece of cake.

Note. Segment tree is likely to be another approach with better time complexity. However, the sweep line algorithm is very intuitive and still perform nicely.

```typescript
class MyCalendarThree {
  q: Array<[number, number]>
  
  constructor() {
    this.q = []
  }

  book(start: number, end: number): number {
    this.q.push([start, 1])
    this.q.push([end, -1]) 
    this.q.sort((a,b) => {
      if ( a[0] === b[0] ) return a[1] < b[1] ? -1 : 1
      return a[0] < b[0] ? -1 : 1
    })
    let ret = 0
    let count = 0
    for ( let i=0;i<this.q.length;++i ) {
      count += this.q[i][1]
      ret = Math.max(ret, count)
    }
    return ret
  }
}
```