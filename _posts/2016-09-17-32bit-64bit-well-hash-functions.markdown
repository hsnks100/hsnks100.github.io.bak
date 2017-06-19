---
layout: post
title: "32bit / 64bit well hash functions"
author: 뻘짓마스터(hsnks100@gmail.com)
date: 2016-09-17 08:51 +0900
tags: hash
category: cpp
comments: true
---
* table of contents
{:toc}



간단히 hash 함수가 필요할 때 쓸 수 있다.

``` cpp
// Robert Jenkins' 32bit hash function
unsigned __int32 hash(unsigned __int32 key)
{
    key += (key<<12);
    key ^= (key>>22);
    key += (key<<4);
    key ^= (key>>9);
    key += (key<<10);
    key ^= (key>>2);
    key += (key<<7);
    key ^= (key>>12);
 
    return key;
}
 
// Thomas Wang's hash function
unsigned __int32 hash32(unsigned __int32 key)
{
    key = ~key + (key << 15); // key = (key << 15) - key - 1
    key ^= (key >> 12);
    key += (key << 2);
    key ^= (key >> 4);
    key += (key << 3) + (key << 11);    // key = key * 2057
    key ^= (key >> 16);
 
    return key;
}
 
unsigned __int64 hash64(unsigned __int64 key)
{
    key = ~key + (key << 21); // key = (key << 21) - key - 1
    key ^= (key >> 24);
    key += (key << 3) + (key << 8); // key = key * 265
    key ^= (key >> 14);
    key += (key << 2) + (key << 4); // key = key * 21
    key ^= (key >> 28);
    key += (key << 31);
 
    return key;
}
 
unsigned __int32 hash6432(unsigned __int64 key)
{
    key = ~key + (key << 18); // key = (key << 18) - key - 1
    key ^= (key >> 31);
    key += (key << 2) + (key << 4); // key = key * 21
    key ^= (key >> 11);
    key += (key << 6);
    key ^= (key >> 22);
 
    return (unsigned __int32)key;
}

```
