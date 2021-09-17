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
 <!--more--> 
## 剑指offer30 包含最小值函数的栈
思路：设置两个栈a,b 栈a进行数据的添加删除等等操作，关键问题是解决在O(1)的时间找到栈的最小元素，我们在每次入栈的时候进行判断。
1.如果当前入栈值小于栈顶元素，那么当前入栈值应该同时应该添加到栈b中
2.如果当前栈a中是空栈，那么直接入栈a
3.出栈的时候，判断当前出栈值和当前b中的栈顶值的大小比较，如果相同的话，那么b的值也要弹出，如果a的出栈值比b的栈顶大，不要管
```c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> minstk; 
    stack<int> stk;
    MinStack() {
        
    }
    
    void push(int x) {
        stk.push(x);
        if(minstk.empty() || minstk.top() >= x){
            minstk.push(x);
        }
    }
    
    void pop() {
        if(minstk.top() == stk.top()) minstk.pop();
        stk.pop();

    }
    
    int top() {
        return stk.top();
    }
    
    int getMin() {
        return minstk.top();
    }
};
```
## 剑指offer06 从尾打印链表
```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        stack<ListNode*> stack;
        vector<int> res;
       for(auto p =  head;  p ; p = p ->next){
           stack.push(p);
       }
        while(!stack.empty()){
            res.push_back(stack.top()->val);
            stack.pop();
        }
        return res;
    }
};
```
## 剑指offer24 反转链表
思路：直接反转每次遍历，遍历完全链表，while(head) 而不是head->next;
```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(!head) return head;
        ListNode* dummy = new ListNode(-1);
        dummy->next =NULL;
        while(head){
            auto r = head->next;
            head->next = dummy->next;
            dummy->next = head;
            head = r;
        }
        return dummy->next;
    }
};
```
## 剑指offer35 复杂链表的复制
思路：给每个节点复制一个小弟节点，然后在小弟节点上操作
1.复制random节点的时候需要判断p->random 是否是空节点，非空才可以进行小弟节点random
2.分开两个链表的时候，对于要分开的链表我们设置一个dummy以及一个一开始指向dummy的cur去操作，这样不会出现双next的情况，最终返回dummy->next即可
```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return head;
        // 给每个节点创建一个小弟
        for(auto p = head ; p ; p = p ->next->next){
            Node* new_p = new Node(p->val);
            new_p->next = p->next;
            p->next = new_p;
        }
        for(auto p = head ; p ; p = p ->next->next){
            auto q = p->next;
            // 小弟的random指向的是大哥的random的下一个
            if(p->random){
                q->random = p->random ->next;
            }
        }
        // 最后将两个分开
        Node* dummy = new Node(-1);
        dummy->next = head->next;
        auto cur = dummy;
        for(auto p = head; p ; p = p ->next){
            auto q = p->next;
            cur->next = q;
            cur = q;
            p->next = q->next;
        }
        return dummy->next;
    }
};
```