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
## 剑指offer34 二叉树中和为某一值的路径
思路：整体分为两步，一步是进行前序遍历，第二步是在遍历的过程中进行记录，每次遍历到一个节点我们将这个节点的值进行记录
1.当路径达到了空节点的时候，我们直接返回
2.当路径达到了叶子节点同时此时的路径之和正好等于目标值，我们添加到res中
```c++
class Solution {
public:
    vector<vector<int>> res;    
    vector<int> path;
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        if(!root) return res;
        dfs(root , target);
        return res;
    }
    void dfs(TreeNode* root , int sum){
        // 前序遍历 如果当前遍历到了空节点 那么直接返回
        if(!root) return ;
        sum -= root->val;
        path.push_back(root->val);
        if(sum == 0  && root->left == NULL && root->right == NULL)res.push_back(path);
        if(root->left) dfs(root->left, sum);
        if(root->right) dfs(root->right , sum);
        path.pop_back();
    }
};
```
## 剑指offer36 二叉搜索树与双向链表
```c++
class Solution {
public:
// 二叉搜索树的中序遍历就是一个递增的序列，我们中序遍历的过程中找到前一个节点以及后一个节点，然后将两个节点进行连接，如果前一个节点是空的话，说明当前节点就是最小的节点，最终遍历完全之后，head指向的是头，pre指向的是尾部，然后head->left= pre pre->right=head
// 初始化head节点和pre节点什么都没有指向，然后head的
    Node* head;
    Node* pre;
    Node* treeToDoublyList(Node* root) {
        if(!root)return NULL;
        dfs(root);
        // 最终结束的时候head指向最前一个 pre指向最后一个 cur指向了NULL 返回
        head->left = pre;
        pre->right = head;
        return head;
    }
    void dfs(Node* cur){
        // 当前遍历的节点是空说明已经走到最后了
        if(cur == NULL) return ;
        dfs(cur->left);
        // 如果当前节点的前一个是空表示当前节点没有前一个小于它的节点，那么它就是最小的，所以head指向它
        if(pre == NULL) head =cur;
        else{
            pre->right = cur;
            cur->left = pre;
        }
        pre = cur;
        dfs(cur->right);
    }
};
```
## 剑指offer53 二叉搜索树的第k大的数
```c++
class Solution {
public:
    int res = 0;
    int k = 0;
    int kthLargest(TreeNode* root, int _k) {
        if(!root) return 0;
        k = _k;
        dfs(root);
        return res;
    }
    void dfs(TreeNode* root) {
        if(!root) return ;
        dfs(root->right);
        if(--k == 0 )res = root->val;
        dfs(root->left);
    }
};
```
## 剑指offer45 把数组排成最小的数
将快排应用在字符串中间，一个数比另一个大的时候，那么它在前面形成的数字的大小是要更大的
```c++
class Solution {
public:
// c++中string类型可以直接进行比较操作
    string minNumber(vector<int>& nums) {
        string res = "";
        vector<string> strs;
        for(auto c:nums) strs.push_back(to_string(c));
        int  n =strs.size();
        QuickSort(strs , 0 , n - 1);
        for(auto c:strs){
            res.append(c);
        }
        return res;
    }
    int Patition(vector<string>& strs , int low , int high){
        string temp = strs[low];
        string pivot = strs[low];
        while(low < high) {
            while(strs[high] + pivot >= pivot + strs[high] && low < high) high--;
            strs[low] = strs[high];
            while(strs[low] + pivot <= pivot + strs[low] && low < high) low++;
            strs[high] = strs[low];
        }
        strs[low] = temp;
        return low;
    }
    void QuickSort(vector<string>& strs, int low , int high){
        if(low >= high) return ;
        int p = Patition(strs , low , high);
        QuickSort(strs, low, p - 1);
        QuickSort(strs, p + 1, high);
    }
};
```
## 剑指offer61 扑克牌中的顺子
满足顺子的条件有
1.最大值减去最小值的差值小于5（不算大小王）
2.没有重复的数字
```c++
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        int n = nums.size();
        unordered_set<int> Set;
        int ss = 14 , mm = 0;
        for(int i = 0 ; i < n ; i++){
            if(nums[i] == 0 )continue;
            if(Set.count(nums[i])) return false;
            mm = max(mm ,nums[i]);
            ss = min(ss , nums[i]);
            Set.insert(nums[i]);
        }
        return mm - ss < 5;
    }
};
```
## 剑指offer40 最小的k个数字
思路：大根堆，c++中的优先队列就是大根堆，每次存储最小的k个数字，如果当前的数字小于大根堆中的最小的数字，那么直接弹出堆顶元素，优先队列的pop()是堆顶元素，push是加入进去，top()也是堆顶元素
```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> res(k , 0);
        if(k == 0 )return res;
        // 大根堆
        priority_queue<int , vector<int> , less<int>> Q;
        // 小根堆
        // priority_queue<int , vector<int> , greater<int>> Q;
        for(int i = 0 ; i < k ; i ++){
            Q.push(arr[i]);
        }
        for(int i = k ; i < arr.size() ; i ++){
            if(Q.top() > arr[i]){
                Q.pop();
                Q.push(arr[i]);
            }
        }
        for(int i = 0 ; i < k ; i ++){
            res[i] = Q.top();
            Q.pop();
        }
        return res;
    }
};
```
## 剑指offer41 数据流中的中位数
```c++
class MedianFinder {
public:
    /** initialize your data structure here. */
    //设置一个大根堆一个小根堆
    priority_queue<int,vector<int> ,less<int>> Big;
    priority_queue<int,vector<int> ,greater<int>> Small;
    MedianFinder() {
        
    }
    void addNum(int num) {
        // 每次保证大根堆的数字总数是大于小根堆的总数，或者两个堆的数量相同，返回值就是大根堆的堆顶，或者两个堆的堆顶之和的平均值
        // 在大根堆中洗礼一下得到一个大根堆的最大值
        Big.push(num);
        int top = Big.top();
        Big.pop();
        Small.push(top);
        if(Small.size() > Big.size()){
            int top2 = Small.top();
            Small.pop();
            Big.push(top2);
        }
    }
    double findMedian() {
        if(Small.size() == Big.size()) return (double)(Big.top() + Small.top())/2;
        else return (double)Big.top();
    }
};
 ```
 ## 剑指offer55 二叉树的深度
 ```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        return max(left, right) + 1;
    }
};
```
## 剑指offer55 平衡二叉树
平衡二叉树不仅仅当前的左右子树的高度差不能超过1，同时左右子树的高度差也不能超过一
```c++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(!root) return true;
        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        if(left > right + 1 || right > left + 1)return false;
        return isBalanced(root->left) && isBalanced(root->right);
    }
    int maxDepth(TreeNode* cur){
        if(!cur) return 0;
        int left = maxDepth(cur->left);
        int right = maxDepth(cur->right);
        return max(left  , right) +1;
    }
};
```
## 剑指offer68 二叉搜索树的最近公共祖先节点
```c++
class Solution {
public:
// 分为三种情况，如果p的值小于root的值但是q的值大于root的值那么直接返回root
// 如果 p q 在同一边都在root的左边，那么直接递归到左子树去 反之递归到右子树
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // 始终保持p的节点值小于q的节点值
        if(p -> val > q->val) swap(p , q);
        if(root->val >= p->val && root->val <= q->val) return root;
        if(q->val < root->val) return lowestCommonAncestor(root->left , p , q);
        else return lowestCommonAncestor(root->right , p , q);
    }
};
```
## 剑指offer68 II 二叉树的最近公共祖先
方法一：直接找到到当前节点的路径，然后从根开始遍历路径中是否存在相同的祖先节点，如果存在则直接返回
方法二：递归
```c++
class Solution {
public:
// 得到一条从des到cur的路径
    bool dfs(TreeNode* cur , TreeNode* des , vector<TreeNode*>& path){
        if(cur == NULL) return false;
        if(cur == des) {
            path.push_back(cur);
            return true;
        }
        if(dfs(cur ->left , des , path ) || dfs(cur->right , des ,path)){
            cout<<path[0]->val;
            path.push_back(cur);
            return true;
        }
        return false;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        vector<TreeNode*> pathP ;
        vector<TreeNode*> pathQ;
        // 得到两条从p和q出发到root的路径
        dfs(root , p , pathP);
        dfs(root , q , pathQ);
        // 转变路径之后得到两条从root出发到q p的路径
        reverse(pathQ.begin(),pathQ.end());
        reverse(pathP.begin(),pathP.end());
        int len = min(pathP.size() , pathQ.size());
        for(int i = len-1 ; i >= 0  ; i --){
            if(pathP[i] == pathQ[i]){
                return pathP[i];
            }
        }
        return NULL;
    }
};
class Solution {
public:
// 递归方法如果当前的root等于p或者q那么直接返回q或者p
// 在左右子树中找p或者q的公共祖先，如果找到的left或者right都是空说明，左右子树都不存在p或者q，那么返回NULL
// 如果左子树为空，返回右反之返回左，左右都不是空，返回root
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL || root == q || root == p) return root;
        auto left = lowestCommonAncestor(root->left , p , q) ;
        auto right = lowestCommonAncestor(root->right , p , q);
        if(left==NULL) return right;
        if(right == NULL) return left;
        return root;
    }
};
```
## 剑指offer07 重建二叉树
使用一个map找到每个中序中的数字的位置，通过位置找到左右子树的开始与结束的节点位置
```c++
class Solution {
public:
// 递归构造方法，在前序遍历中是根左右  中序中是左根右
    unordered_map<int,int>  map;
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for(int i = 0 ;i < inorder.size() ; i ++) map[inorder[i]] = i ; 
        int len = preorder.size();
        TreeNode* root = dfs(preorder , inorder , 0 , len -1 , 0 , len - 1);
        return root;
    }
    TreeNode*  dfs(vector<int>& preorder , vector<int>& inorder , int pl , int pr , int il , int ir){
        if(pl > pr) return NULL;
        TreeNode* root = new TreeNode(preorder[pl]);
        int k = map[root->val];
        // k是根的位置 k-1-il是递归下去的长度
        root->left = dfs(preorder , inorder , pl + 1 ,pl + 1 + k - 1 - il ,il , il + k - 1);
        root->right = dfs(preorder , inorder , pl + 1 + k - 1 - il + 1 , pr , k + 1 , ir);
        return root;
    }
};
```
## 剑指offer16 值的整数次平方
快速幂的方法：注意边界值的处理
```c++
class Solution {
public:
// 快速幂的方法，任何一个数字n可以转换为一个二进制数，当这个二进制数字 x^n == x^1001 = x^2^3*1 * x^2^0*1
    double myPow(double x, int n) {
        // 如果n是int类型的最小值 -2147483648,2147483647 需要转换为-n 的时候会出错
        long b = n ;
        if(n < 0 ){
            x = 1  / x;
            b = -b;
        }
        double res = 1.0;
        while(b > 0 ){
            if(b & 1 == 1) res = res * x;
            x = x *x ;
            b >>= 1;
        }
        return res;
    }
};
```
## 剑指offer33 二叉搜素树的后序遍历序列
```c++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        int len = postorder.size();
        return dfs(postorder , 0 , len - 1);
    }
    bool dfs(vector<int>& postorder ,int  l , int r){
        // 如果开始节点的位置大于等于结尾的位置，说明这个后序节点没有问题了
        if(l >= r) return true;
        int k = l ;
        while(k < r && postorder[k] <  postorder[r]) k ++ ;
        // k == r表示没有右子树 全部是左子树
        if(k == r)  return dfs(postorder , l , r - 1);
        int temp = k ;
        while(k < r && postorder[k] > postorder[r]) k ++;
        // 一次遍历完全后，k应该为结尾节点 如果不是直接返回false
        if(k < r) return false; 
        return dfs(postorder, l , temp - 1 ) && dfs(postorder, temp , r - 1);
    }
};
```
## 剑指offer15 二进制中1的个数
```c++
class Solution {
public:
// 无符号整数的n 
    int hammingWeight(uint32_t n) {
        int res = 0 ;
        long long  m = n ;
        while(m){
            res += m & 1;
            m >>= 1;
        }
        return res;
    }
};
```
## 剑指offer65 不用加减乘除做加法
每一位的和转换为亦或操作，每一位的进位操作转换为求与然后右移一位操作，求和的方法就是循环使用亦或，然后将进位赋值给b
```c++
class Solution {
public:
// 计算机中int类型是以补码形式存在的
// a b 和  进位
// 0 0 0   0 
// 0 1 1   0 
// 1 0 1   0
// 1 1 0   1  和的计算的为非进位的亦或运算，进位的计算为a & b >> 1
    int add(int a, int b) {
        int res = 0 ; 
        while(b ){
            cout <<"a为"<< a << endl;
            cout <<"b为"<< b << endl;
            // 首先计算进位的数值  c++ 中负数不支持左移位 ，所以要转换为无符号
            int c = (unsigned int)(a & b ) << 1;
            cout <<"c为"<< c << endl;
            // 然后计算和，直接将和合并到a中去
            a ^= b;
            b = c ;
        }
        return a ; 
    }
};
```
## 剑指offer56 数组中数字出现的次数
```c++
class Solution {
public:
// 一共有两个数字是不一样的，所有的数字进行亦或处理之后，得到了就是这两个数字的亦或值
// 这个两个数字是不一样的，所以得到的这个异或值中为1的部分肯定是两个数不同的位，我们找到第一个这样的位，就可以将两个数字区分开来了
    vector<int> singleNumbers(vector<int>& nums) {
        int x = 0 , y = 0 , m = 1, temp = 0;
        for(auto num: nums){
            temp ^= num;
        }
        // 找到第一个为1的位
        while((temp & m) == 0 ){
            m <<= 1;
        }
        for(auto num :nums){
            if(num & m )  x^= num;
            else y^= num;
        }
        return {x,y};
    }
};
```
## 剑指offer39 数组中的众数
记作众数的票数为1 非众数的票数为-1 总体的票数之和是>0的，如果遍历过程中前一段的票数之和为0了，表示众数没有出现在前一段，后一段的票数之和仍然是大于0的，众数出现在后一段中
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int res = 0 ;
        int n = nums.size();
        int vote = 0;
        for(int i = 0 ; i < n ; i ++){
            if(vote == 0) res = nums[i];
            if(nums[i] == res) vote++;
            else vote -- ;
        }
        return res;
    }
};
```
## 剑指offer66 构建乘积数组
分成两段计算乘积，前一段的乘积再乘以后一段的乘积得到最终的结果
```c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int n = a.size();
        if(!n) return {};
        vector<int> b(n, 0);
        b[0]=1;
        // 最终得到的b[i]为从0到i-1这一段的数字之积
        for(int i = 1 ; i < n ; i ++){
            b[i] = b[i-1] * a[i - 1 ];
        }
        int temp = 1;
        // temp得到从i往后的一段的乘积
        for(int i = n - 2; i >= 0 ; i --){
            temp = temp * a[i + 1];
            b[i] = b[i] * temp;
        }
        return b;
    }
};
```
## 剑指offer14 剪绳子
```c++
class Solution {
public:
    int cuttingRope(int n) {
        // 2*2*2 小于3 * 3
        int res = 1 ;
        if(n <= 3){
            if(n == 3)return 2;
            if(n == 2)return 1;
        }
        while(n > 3){
            n-=3;
            res *= 3;
        }
        if(n == 3){
            res *=3;
        }
        if(n == 1 ) {
            res /=3;
            res *=4;
        }
        if(n == 2) res *= 2;
        return res;
    }
};
```
## 剑指offer57 和为s的连续正常序列
```c++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<int> path;
        vector<vector<int>> res;
        // 滑动窗口 左闭右开 
        int left = 1 , right = 1 ;
        int sum = 0 ;
        while(left <= target / 2){
            if(sum < target){
                sum += right;
                right ++ ;
            }
            else if (sum > target){
                sum -= left;
                left ++ ;
            }else{
                for(int i = left ; i < right ; i++){
                    path.push_back(i);
                }
                res.push_back(path);
                path.clear();
                sum -= left;
                left ++ ;
            }
        }
        return res;
    }
};
```
## 剑指offer62 圆圈中最后剩下的数字
方法一：数学方法，每次删除一个然后下一个数字顶上去，最终剩下的坐标肯定是下标0，每次加上长度然后取模得到下一轮的下标直到到最后的结果。
方法二：递归方法加上数学方法，get(n , m) ，如果n为1，返回的下标就是0，每次删除一个数字之后得到最终的
```c++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int res = 0 ;
        for(int i = 2 ; i <= n ; i++){
            res = (res + m ) % i ; 
        }
        return res;
    }
};
class Solution {
public:
// 递归的方法
    int get(int n , int m ){
        if(n == 1) return 0;
        int x = get(n - 1 , m);
        return (m%n  + x) % n ;
    }
    int lastRemaining(int n, int m) {
        return get(n , m);
    }
};
```
## 剑指offer29 顺时针打印矩阵
```c++
class Solution {
public:
    vector<vector<bool>> st ;
    int dx[4] = {0 , 1 , 0 , -1 };
    int dy[4] = {1 , 0 , -1 , 0 };
    vector<int> res;
    int d = 0 ;
    int m , n ;
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        m = matrix.size();
        if(!m) return res;
        n = matrix[0].size();
        st = vector<vector<bool>>(m , vector<bool>(n , false));
        for(int i = 0 , x = 0 , y = 0 ; i < m * n ; i ++){
            st[x][y] = true;
            res.push_back(matrix[x][y]);
            int a = x + dx[d];
            int b = y + dy[d];
            // 这里不能用while由于当所有的数字都遍历完全后，会一直进入死循环，因为周围全部都是true
            if(a >= m || a < 0 || b >= n ||  b < 0  || st[a][b] == true) {
                cout<<"循环内部:"<< a << ' ' << b ;
                d = (d + 1) % 4;
                a = x + dx[d];
                b = y + dy[d];
            }
            cout<< "这是："<<a <<" "<< b;
            x = a; 
            y = b;
        }
        return res;
    }
};
```
## 剑指offer31 栈的压入、弹出序列
```c++
class Solution {
public:
// 使用一个栈进行模拟操作，如果这个栈最终的模拟结果可以实现空栈，说明可以达到目标
// 如果最后这个栈不能为空说明不能，由于栈中的数字都是互不相同的，所有每次遇到一个可以出栈的数字就得马上出栈
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> stk ; 
        int count  = 0 ; 
        for(int i = 0 ; i < pushed.size(); i ++){
            stk.push(pushed[i]);
            while(!stk.empty() && stk.top() == popped[count]){
                stk.pop();
                count ++; 
            }
        }
        return stk.empty();
    }
};
```
## 剑指offer20 表示数值的字符串
分为两个函数一个是判断是否为无符号一个是判断为有符号的
```c++
class Solution {
public:
    bool scaninteger(string s , int& index){
        if(s[index] == '+' || s[index] == '-') {
            index ++;
        }
        return unsignedInterger(s , index);
    }
    bool unsignedInterger(string s , int& index){
        int begin = index;
        while(index < s.size()  && s[index] >= '0' && s[index] <= '9'){
            index++;
        }
        return begin < index;

    }
    bool isNumber(string s) {
        if(s.size() == 0)return false;
        int k = 0;
        while(s[k] == ' ') k ++;
        bool res = scaninteger(s , k);
        if(s[k] == '.'){
            k ++;
            res = unsignedInterger(s , k ) || res;
        }
        if(s[k] == 'e' || s[k] == 'E'){
            k ++;
            res = scaninteger(s, k) && res;
        }
        while(s[k] == ' ') k ++;
        return res && k == s.size();
    }
};
```
## 剑指offer67 字符串转换为整数
```c++
class Solution {
public:
    int strToInt(string s) {
        int n = s.size();
        int res = 0 ;
        int i = 0 ;
        int sign = 1;
        while(s[i] == ' ' && i < n){
            i ++ ;
        }
        if(s[i] == '+'){
            i ++;
        }else if(s[i] == '-'){
            i ++ ;
            sign = -1;
        }
        for( ; i < n ; i ++){
            if(s[i] < '0' || s[i] > '9') break;
            int x = s[i] - '0';
            if(sign == 1 && res > (INT_MAX - x)/10) return INT_MAX;
            if(sign == -1 && -res < (INT_MIN + x)/10 )return INT_MIN;
            // 因为负数的绝对值要大于正数的绝对值 ,此时res一直存储的是正数部分 ,所以这里要进行特判,如果当前的数字正好大小就是负数的最小值直接返回
            if(-res * 10 - x == INT_MIN) return INT_MIN;
            res = res * 10 + x;
        }
        return res * sign;
    }
};
```


