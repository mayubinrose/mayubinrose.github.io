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
 <!--more--> 
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
## 剑指offer26 树的子结构
思路：先序遍历树A，每次从该节点开始去找是否包含子树B
定义函数dfs(a,b)表示从节点a开始去找是否包含节点b子树，当b为空表示b已经匹配完全，返回true，如果a为空，表示没法继续匹配了，返回false，如果两个值都不一样，返回false，如果两个值相同，那么去匹配a的左和b的左以及a的右和b的右
```c++
class Solution {
public:
// 分为两步，1.先序遍历每个树A中的每个节点 2.判断每个节点为根节点是否包含B树
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(!A || !B) return false;
        return (dfs(A , B) || isSubStructure(A->left , B) || isSubStructure(A->right , B)); 
    }
    // 判断a中是否包含b
    bool dfs(TreeNode* a , TreeNode* b){
        if(b == NULL) return true;
        if(a == NULL) return false;
        if(a->val != b->val) return false;
        return dfs(a->left , b->left) && dfs(a->right , b->right);
    }
};
```
## 剑指offer27 二叉树的镜像
思路：将整体进行递归，左子树放到右子树 右子树放到左子树 一开始要设置两个临时遍历right left 放置中途的改变
```c++
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if(!root) return NULL;
        auto right = mirrorTree(root->left);
        auto left = mirrorTree(root->right);
        root->left  = left;
        root->right = right;
        return root;
    }
};
```
## 剑指offer28 对称的二叉树
```c++
class Solution {
public:
// 判断一个树是对称的 也就是判断左右子树是否对称
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        return dfs(root->left , root->right);
    }
    // 判断a b两个树是否是对称的
    bool dfs(TreeNode* a , TreeNode* b){
        if(!b && a)return false;
        if(!a && b)return false;
        if(!a && !b)return true;
        if(a->val == b->val ) return dfs(a->left ,b->right) && dfs(a->right , b ->left);
        else return false;
    }
};
```
## 剑指offer10 斐波那契数列
```c++
class Solution {
public:
    int fib(int n) {
        if(n == 1) return 1;
        if(n == 0 )return 0;
        int a = 0 ,b = 1;
        int sum = 0 ;
        int MOL = 1e9 + 7;
        for(int i = 2; i <= n  ; i ++){
            sum = (a  + b) %MOL;
            a = b ; 
            b = sum ;   
        }
        return sum;
    }
};
```
## 剑指offer63 股票的最大利润
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(!prices.size()) return 0;
        int n = prices.size();
        int MIN_PRICE = prices[0];
        int res = 0 ;
        // 去找到哪一天卖出去
        for(int i = 0 ; i < n ; i ++){
            MIN_PRICE = min(MIN_PRICE , prices[i]);
            res = max(res , prices[i] - MIN_PRICE);
        }
        return res;
    }
};
```
## 剑指offer42 连续子数组的最大和
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n + 1 , 0 );
        // fi 表示前i个数字的连续子数组的最大值 返回值为
        f[1] = nums[0];
        int res = f[1];
        for(int i = 2 ; i <= n ; i ++){
            f[i] = max(f[i - 1] , 0 ) + nums[i - 1];
        }
        for(int i = 1 ; i <= n ; i ++){
            res  = max(res, f[i]);
        }
        return res;
    }
};
```
## 剑指offer47 礼物的最大价值
```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        // 设置f[i][j] 表示达到ij点的时候可以获取的礼物价值的最大值
        int m = grid.size();
        if(!m ) return 0;
        int n = grid[0].size();
        vector<vector<int>> f(m + 1 ,  vector<int> (n + 1 , 0 ));
        f[0][0] = grid[0][0];
        //状态转移：f[i][j] = max(f[i-1][j] ,f[i][j-1]) + grid[i][j]
        for(int i = 0; i < m ; i ++){
            for(int j = 0 ; j < n ; j ++){
                if(!i && !j) f[i][j] =grid[0][0];
                if(i == 0 && j ) f[0][j] = f[0][j - 1] + grid[0][j];
                if(j == 0 && i ) f[i][0] = f[i - 1][0] + grid[i][0];
                if(i && j ) f[i][j] = max(f[i-1][j] , f[i][j-1]) +grid[i][j];
            }
        }
        return f[m-1][n-1];
    }
};
```
## 剑指offer46 把数字翻译为字符串
注意：num转换为string to_string()，初始化的f[0]==f[1]==1，没有数字和一个数字转换为字符串的方案数都是1次
```c++
class Solution {
public:
    int translateNum(int num) {
        string str = to_string(num);
        int n = str.size();
        //f[i]表示num的前i个数字可以翻译成字符串的方案个数,返回值为f[n]
        vector<int> f(n + 1, 0);
        f[0] = 1;
        f[1] = 1;
        for(int i = 2 ; i <= n ; i ++){
            f[i] = f[i - 1];
            int cur = 10* (str[i - 2] - '0') + str[i - 1] - '0';
            if(cur <= 25 && cur >= 10){
                f[i] += f[i - 2];
            }
        }
        return f[n];
    }
    
};
```
## 剑指offer48 最长不含重复字符的子串
方法一：滑动窗口，我们判断的时候不是根据窗口的最前面进行判断，而是在窗口的末尾处进行判断，通过末尾的数量去移动起始窗口的位置
方法二：动态规划+哈希表，哈希表存储的是每个字符的最右边的位置，由于在C++中没有找到当前字符的时候返回的是0，所以我们需要将整体的字符的 s = ' '+s，这样可以区分找到第一个位置和没有找到
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        int i = 0 , j = 0 ; 
        int res = 0 ;
        unordered_map<char,int> map ; 
        while(j < n){
            map[s[j]] ++;
            // 所有的判断条件在s[j]处
            while(map[s[j]] > 1){
                map[s[i]] --;
                i ++ ;
            }
            res = max(res , j - i + 1);
            j ++;
        }
        return res;
    }
};
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        if(n == 0 )return 0 ;
        s = ' ' + s;
        vector<int> f(n + 1 , 0 );
        // f[i] 表示s的前i个字符长度的最长的不包含重复子串的最长子字符串的长度 ,返回值为f[n]中的最大值
        // 状态转移方程，当我们遍历到s[i]，我们向前找到最近的一个和s[i]相同的字符s[j]，
        // 1. 如果j< 0，说明前一段是找不到相同的f[i] = f[i-1] +1 
        // 2. 如果 i - j > f[i-1] ，说明找到的j的位置已经是越过去最长子串的长度，此时f[i] = f[i-1] + 1;
        // 3. 如果 i - j <= f[i-1] ，说明找的的j的位置是在最长子串之内，那么此时f[i] = i - j ;
        // 设置哈希表存储每个字符出现的最右边的位置
        f[1] = 1;
        f[0] = 0;
        int res = 0;
        unordered_map<char , int> map;
        for(int i = 1 ; i <= n ; i ++){
            // 一开始的是找不到的 返回的是位置0 ， 先去找到i位置左边的最近的一个相同字符的下标，然后再给当前字符赋值最右边位置
            int j = map[s[i]];
            map[s[i]] = i ;
            if(i - j > f[i - 1]) f[i] = f[i - 1] + 1;
            if(i - j <= f[i - 1]) f[i] = i - j ;
             res = max(res , f[i]);
        }
        return res;
    }
};
```
## 剑指offer18 删除链表的节点

```c++
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        auto p = head , q = dummy;
        while(p){
            if(p->val == val){
                q->next = p->next;
            }
            p = p ->next;
            q = q->next ;
        }
        return dummy->next;
    }   
};
```
## 剑指offer22 链表中倒数第k个节点
```c++
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        int n = 0 ; 
        ListNode* dummy = new ListNode(-1);
        dummy->next  = head;
        while(dummy->next){
            n ++;
            dummy = dummy->next;
        }
        int cnt = n - k  ;
        while(cnt --){
            head  =head->next;
        }
        return head;
    }sdsd
};
```
## 剑指offer25 合并两个排序的链表
```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = NULL;
        auto p = dummy;
        while(l1 && l2){
            auto r1 = l1->next;
            auto r2 = l2->next;
            if(l1->val < l2->val){
                l1->next  =NULL;
                p->next = l1;
                p = p->next;
                l1 = r1;
            }else{
                l2->next = NULL;
                p->next = l2;
                p = p->next;
                l2 = r2;
            }
        }
        if(l1) p->next = l1 ; 
        if(l2) p ->next = l2;
        return dummy->next;
    }
};
```
## 剑指offer52 两个链表的第一个公共节点
```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* pa = headA;
        ListNode* pb = headB;
        while(pa != pb){
            pa = pa != NULL ? pa->next: headB;
            pb = pb != NULL ? pb->next : headA;
        }
        return pa;
    }
};
```
## 剑指offer21 调整数组顺序使得奇数在前面
思路：双指针，快慢指针
```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int n  = nums.size();
        int left = 0 , right = n - 1;
        while(left < right){
            if(nums[left] % 2 == 1){
                left++;
                continue;
            }
            if(nums[right] % 2 == 0 ){
                right -- ;
                continue;
            }
            swap(nums[right --] , nums[left ++]);
        }
        return nums;
    }
};
```
## 剑指offer57 和为s的两个数
思路：双指针算法，利用数组递增的性质
```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0 , right = n - 1;
        while(left < right){
            int sum = nums[left] + nums[right];
            if(sum == target) return {nums[left] , nums[right]};
            else if(sum < target) {
                left ++;
            }else{
                right -- ;
            }
        }
        return {};
    }
};
```
## 剑指offer58 翻转单词顺序
思路：先将前后的空格去掉，然后将每个单词进行反转存储在容器中，最后反向从容器中取单词合并组成最终答案。这题关键是很多细节部分
1.当s为空，当s为纯空格需要进行特判
2.当我们把前后的空格去掉之后，为了方便找每个单词，给最后一个字符后再加上一个空格
```c++
class Solution {
public:
    string reverseWords(string s) {
        int n = s.size();
        if(n == 0)return "";
        //第一步将空格处理
        int i = 0 , j = n - 1;
        while(s[i] ==' ' && i < n) i ++ ;
        if(i == n )return "";
        while(s[j] == ' ' && j >= 0 ) j --;
        s = s.substr(i , j - i + 1);
        // 第二步开始翻转
        s = s + ' ';
        vector<string> res;
        i = 0 , j = 0 ;
        while(j < s.size() ){
            if(s[j] != ' ') {
                j ++;
            }
            if(s[j] == ' '){
                string cur = s.substr(i , j - i );
                res.push_back(cur);
                while(s[j]==' '){
                    j++;
                }
                i = j ;
            }
        }
        string ans = "";
        for(int i = res.size() - 1 ; i >= 0 ; i --){
            ans += res[i];
            ans += ' ';
        }
        ans.pop_back();
        return ans;
    }
};
```
## 剑指offer12 矩阵中的路径
关键处，在返回的时候，应该写if(dfs)return ，而不是直接return dfs 。这两者的区别为当dfs为false的时候，只是说明当前的起始节点找不到路径，而后者是如果一条路径找不到直接返回了。
```c++
class Solution {
public: 
    //  设置一个标记容器，当走过当前的字符的时候，标记为True
    vector<vector<bool>> st;
    int dx[4] = {0 , 1 , 0 , -1};
    int dy[4] = {1 , 0 , -1 , 0};
    int m , n;
    bool exist(vector<vector<char>>& board, string word) {
        m = board.size();
        if(!m) return false;
        n = board[0].size();
        st = vector<vector<bool>>(m  , vector<bool>(n , false));
        for(int i = 0 ; i < m ; i ++){
            for(int j = 0 ; j < n ; j ++){
                if(dfs(board , word , 0 , i , j ) ) return true;
            }
        }
        return false;
    }
    bool dfs(vector<vector<char>> & board , string word , int u ,int x, int y){
        // 首先判断当前的字符是否为board上xy处的点如果不是直接返回false
        if(word[u] != board[x][y]) return false;
        // 当前遍历的点正好是表格上的点，那么判断是否正好遍历到了最后一个  如果是的话直接返回true
        if(u == word.size()-1 ){
            return true;
        }
        // 当前字符没有遍历过，那么设置遍历好了，然后走到下一个字符
        if(!st[x][y]){
            st[x][y] = true;
            for(int i = 0 ; i < 4; i ++){
                int a = dx[i] + x;
                int b = dy[i] + y;
                if(a >= 0 && a < m && b>= 0 && b< n && !st[a][b]){
                    if(dfs(board , word , u+1 , a  ,b)) return true;
                }
            }
            st[x][y] = false;
        }
        return false;
     }
};
```
## 剑指offer13 机器人的运动范围
思路：广度优先遍历一般采用的是队列的形式存储已经遍历的节点，这里采用的是负对角线斜向下的广度优先遍历方法
```c++
class Solution {
public:
    int count(int x,int y){
        int res = 0 ;
        while(x){
            res += x%10;
            x /= 10;
        }
        while(y){
            res += y%10;
            y /= 10;
        }
        return res;
    }
    int movingCount(int m, int n, int k) {
        int dx[2] = {0 , 1};
        int dy[2] = {1 , 0};
        if(!k) return 1;
        queue<pair<int , int>> q;
        vector<vector<bool>> st(m , vector<bool>(n , false));
        q.push(make_pair(0 , 0 ));
        st[0][0] = true;
        int res = 1;
        while(!q.empty()){
            auto [a,b] = q.front();
            q.pop();
            for(int i = 0; i < 2 ; i ++){
                int x = a  + dx[i];
                int y = b + dy[i];
                if(x <0 || x >= m || y <0 || y >= n || st[x][y] || count(x , y) > k ){
                    continue;
                }
                q.push(make_pair(x,y));
                res ++;
                st[x][y] = true;
            }
        }
        return res;
    }
};
```










