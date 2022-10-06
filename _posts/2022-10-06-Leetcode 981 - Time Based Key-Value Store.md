---
title: Leetcode 981 - Time Based Key-Value Store
date: 2022-10-06 09:22:19 +0800
categories: [Leetcode, Medium]
tags: [binary search, hash table]     # TAG names should always be lowercase
---

The titile of the [question](https://leetcode.com/problems/time-based-key-value-store/) is clearly show a dictionary is required. As stated in the **Constraints**, the `set` function will give `timestamp` strictly increasing, which implies that a sorted array could be easily drawn. 

Based on the intuitive data structure, the rest is applying [binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm).

```typescript
class TimeMap {
  dict: {[k: string]: Array<[string, number]>}
  
  constructor() {
    this.dict = {}
  }

  set(key: string, value: string, timestamp: number): void {
    if ( this.dict[key] === undefined ) this.dict[key] = []
    this.dict[key].push([value, timestamp])
  }

  get(key: string, timestamp: number): string {
    if ( this.dict[key] === undefined ) this.dict[key] = []
    let h = 0
    let t = this.dict[key].length
    let m
    while ( h < t ) {
      m = Math.floor((h+t)/2)
      if ( this.dict[key][m][1] <= timestamp ) {
        h = m + 1
      } else {
        t = m
      }
    }
    if ( h - 1 >= 0 ) {
      return this.dict[key][h-1][0]
    } else {
      return ""
    }
  }
}
```

