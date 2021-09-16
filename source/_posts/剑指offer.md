---
layout: title
title: 剑指offer
date: 2021-09-16 20:36:47
tags: 算法题
top: 99 
categories: 开发岗
---
## 剑指offer09 用两个栈实现队列
解题思路：
整体的思路是采用两个栈进行操作，第一个栈进行入栈操作，第二站进行删除操作，这里题目的输出值表示每次操作的输出值，当每次入栈的时候，直接将入栈值放入a中，当出队的时候，判断b是否为空，如果是空，那么判断a是否为空，都是空那么表示没有元素了，直接返回-1，反之将a中所有元素出栈然后入栈b，最后返回b的栈顶元素。
```c++
class CQueue {
    stack<int> a  , b ; 
public:
    CQueue() {
        while(!a.empty()){
            a.pop();
        }
        while(!b.empty()){
            b.pop();
        }
    }
    
    void appendTail(int value) {
        a.push(value);
    }
    
    int deleteHead() {
        if(b.empty()){
            if(a.empty()) return -1;
            else{
                while(!a.empty()){
                    int top = a.top();
                    a.pop();
                    b.push(top);
                }
            }
        } 
        if(b.empty()){
            return -1;
        }else{
            int res = b.top();
            b.pop();
            return res;
        }
    }
};
```

