---
layout: title
title: 剑指offer
date: 2021-09-16 20:36:47
tags: [算法题,开发岗]
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
## 剑指offer05 替换空格
```c++
class Solution {
public:
    string replaceSpace(string s) {
        int n = s.size();
        string res = "";
        for(int i = 0 , j = 0 ; i  < n ; i ++){
            if(s[i] == ' '){
                res.push_back('%');
                res.push_back('2');
                res.push_back('0');
            }else{
                 res.push_back(s[i]);
            }
        }
        return res;
    }
    
};
```
## 剑指offer58 左旋转字符串
```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string res = "";
        string a = "";
        string b = "";
        for(int i = 0 ; i < n ; i ++){
            a.push_back(s[i]);
        }
        for(int j = n ; j < s.size() ; j ++){
            b.push_back(s[j]);
        }
        res = b + a;
        return res;
    }
};
```
## 剑指offer03 数组中重复的数字
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        unordered_set<int> hash;
        for(int i = 0 ; i < nums.size();  i ++){
            if(hash.count(nums[i]) == 0){
                hash.insert(nums[i]);
            }
            else{
                return nums[i];
            }
        }
        return nums[nums.size() - 1];
    }
};
```
## 剑指offer53 在排序数组中查找数字I
思路：二分查找的思想，与原始的二分查找的模板有所不同，while中的条件带上=，然后循环体中l与r都含有加一减一的操作，这样就不会出现死循环的结果，构造函数找到target的最右边的坐标
1、如果mid值小于target，l = mid + 1
2、如果mid值大于target，r = mid - 1
3、如果mid等于target，如果我们要找到最右边的target，同样l = mid + 1 ，反之如果我们要找到最左边的target，r = mid -1 
最后找最右边的target，返回l ，找最左边的target，返回r。
注意：这里可以加上特判条件，当我们找最右边或者最左边的target的时候，一次二分之后，可以通过特判当前找到的数是否是target，如果不是说明整个区间都没有target，可以直接返回0
```c++
class Solution {
public:
    int findright(vector<int>& nums , int target){
        int l = 0 ,r = nums.size() - 1;
        while(l <= r){
            int mid = (l + r) >> 1;
            if(nums[mid] <= target) {
                l = mid + 1;
            }else{
                r = mid -1;
            }
        }
        return l ;
    }
    int search(vector<int>& nums, int target) {
        return findright(nums , target) - findright(nums , target-1);
    }
};
```
## 剑指offer53 0到n-1中缺失的数字
思路：二分法，左子数字：nums[i] == i，右子数组nums[i] != i ，循环结束的时候l指向第一个右子数组的第一个也就是返回值，r指向左子数组的最后一个
```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        // 首先是一个递增的区间，这里的二分判断依据是当前数字的值和它的下标之间的比较
        int  l = 0 ,  r = nums.size() -1;
        // 找到第一个大于的点
        while(l <= r){
            int mid = (l + r)>> 1;
            if(nums[mid] == mid) l = mid + 1;
            else{
                r = mid -1;
            }
        }
        return l;
    }
};
```
## 剑指offer04 二维数组的查找
思路：将整个二维数组进行翻转，整体就转换为一个类似树的形式，丛树根开始遍历，如果小于target，向右子树移动，如果大于target就向左子树移动
```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0) return false;
        int i = matrix.size() -1  , j = 0;
        while(j < matrix[0].size() && i >= 0){
            if(matrix[i][j] < target) j ++;
            else if(matrix[i][j] > target) {
                 i -- ;
            }
            else return true;
        }
        return false;
    }
};
```
## 剑指offer11 旋转数组的最小数字
思路：难点在nums[mid] == nums[r]的情况下，如何操作，这里直接r = r-1 ，因为相等的情况无法判断右子数组的最小值出现在哪一边。证明r=r-1的正确性：
1.当旋转点x < r, 执行r = r - 1 易知无影响
2.当旋转点x == r ，执行r = r - 1 丢失了旋转点，但是最终返回的值与旋转点的值大小相同，因为由于x ==r nums[x] == nums[r] == nums[mid] <= nums[l]， 同时mid一定在左数组中，有nums[l] <= nums[mid] ，所以nums[l] == nums[mid]，所以左数组的所有值都相同都等于nums[x]所以得证。
```c++
class Solution {
public:
// 二分法 分为左子数组 和右子数组 左子树组的所有数字都是大于右子数组的所有数字的
// 左右数组都是单调递增的 如果nums[mid] < nums[r]  r = mid  如果nums[mid] > nums[r] l = mid + 1
// 如果nums[mid] ==nums[r] 是无法判断在哪个区间的 所以直接r = r -1
    int minArray(vector<int>& nums) {
        int l = 0 , r = nums.size() - 1;
        while(l < r){
            int mid  = (l + r) >> 1;
            if(nums[mid] < nums[r]){
                r = mid;
            }else if (nums[mid] > nums[r]){
                l = mid + 1;
            }else if(nums[mid] == nums[r]){
                r = r - 1;
            }
        }
        return nums[r];
    }
};
```
## 剑指offer50 第一个只出现一次的字符
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char, bool > hash;
        for(int i = 0 ; i < s.size(); i ++){
            hash[s[i]] = hash.find(s[i]) == hash.end();
        }
        for(int i = 0 ; i < s.size() ; i++){
            if(hash[s[i]] ) return s[i];
        }
        return ' ';
    }
};
```
## 剑指offer32 从上到下打印二叉树I,II,III
```c++
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        int last = -1;
        queue<TreeNode*> q;
        if(!root)return res;
        q.push(root);
        while(!q.empty()){
            auto cur = q.front();
            q.pop();
            res.push_back(cur->val);
            auto left = cur->left;
            if(left) q.push(left);
            auto right = cur->right;
            if(right) q.push(right);
        }
        return res;
    }
};
//思路：使用q.size()来判断每一行有多少个数字
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode* >q ; 
        if(!root) return res;
        q.push(root);
        while(!q.empty()){
            vector<int> path;
            int n = q.size();
            for(int i = 0 ; i < n ; i ++){
                auto cur = q.front();
                q.pop();
                path.push_back(cur->val);
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right);
            }
            res.push_back(path);
        }
        return res;
    }
};
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if(!root) return res;
        q.push(root);
        while(!q.empty()){
            int n = q.size();
            vector<int> path;
            for(int i = 0 ; i < n; i ++){
                auto cur = q.front();
                q.pop();
                path.push_back(cur->val);
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right);
            }
            res.push_back(path);
        }
        for(int i = 0 ; i < res.size() ;i ++){
            if(i % 2 ==1) {
                reverse(res[i].begin() , res[i].end());       
            }
        }
        return res;
    }
};
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        deque<TreeNode*> q;
        if(!root) return res;
        q.push_back(root);
        int level = 0;
        while(!q.empty()){
            int n = q.size();
            vector<int> path;
            for(int i = 0 ;i < n ; i ++){
                if(level & 1){
                    auto cur = q.back();
                    q.pop_back();
                    path.push_back(cur->val);
                    if(cur->right) q.push_front(cur->right);
                    if(cur->left)  q.push_front(cur->left);
                }else{
                    auto cur = q.front();
                    q.pop_front();
                    path.push_back(cur->val);
                    if(cur->left) q.push_back(cur->left);
                    if(cur->right)  q.push_back(cur->right);
                }
            }
            res.push_back(path);
            level++;
        }
        return res;
    }
};
```

