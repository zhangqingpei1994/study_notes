<!-- TOC -->

- [记录自己刷leetcode的点点滴滴](#leetcode)
    - [<font color=#DC143C>203. 删除链表中等于给定值 val 的所有节点</font>](#font-colordc143c203--val--font)
    - [<font color=#DC143C>389. Find the Difference（map实现） </font>](#font-colordc143c389-find-the-differencemap--font)
    - [<font color=#DC143C>739. Daily Temperatures(递减栈) </font>](#font-colordc143c739-daily-temperatures--font)
    - [<font color=#DC143C>258. Add Digits(找规律型的题）</font>](#font-colordc143c258-add-digits-font)
    - [<font color=#DC143C>242. 有效的字母异位词(3种方法) </font>](#font-colordc143c242-3--font)
    - [<font color=#DC143C>49. 字母异位词分组 </font>](#font-colordc143c49---font)
    - [<font color=#DC143C>114. Flatten Binary Tree to Linked List(递归调用)</font>](#font-colordc143c114-flatten-binary-tree-to-linked-list-font)
    - [<font color=red>541. Reverse String II (周期性局部反转字符串) </font>](#font-colorred541-reverse-string-ii---font)
    - [<font color=red>70. 爬楼梯 (经典递归) </font>](#font-colorred70----font)
    - [<font color=red>121. 买卖股票的最佳时机 (经典题)</font>](#font-colorred121---font)
    - [<font color=red>231. Power of Two( n & n-1等知识点较多 ) </font>](#font-colorred231-power-of-two-n-n-1--font)
    - [<font color=red>202. 快乐数(小有成就的题)</font>](#font-colorred202--font)

<!-- /TOC -->
# 记录自己刷leetcode的点点滴滴


## <font color=#DC143C>203. 删除链表中等于给定值 val 的所有节点</font>

示例:
    
    输入: 1->2->6->3->4->5->6, val = 6
    输出: 1->2->3->4->5

代码：
```C
/*Definition for singly-linked list.
struct ListNode 
{  
   int val;
   ListNode *next;
   ListNode(int x) : val(x), next(NULL) {}
};*/
 
class Solution 
{
    public:
    void remove(ListNode*  &head, int val)
    {
     //如果没有引用&的话，这个head就不是传进来的head，而是在另外一个地方新建的一个变量     
          if(head)      
          {
            if(head->val == val )
            { 
               head=head->next;
               if(head)
                 remove(head,val);
            }
            else
               remove(head->next,val);     
          }    
    }
    ListNode* removeElements(ListNode* head, int val)
    {
        remove(head,val);
        return head;
    }
};
```
总结：写这个题意在说明有时候即便函数中的参数是指针它也有可能不改变传进来的参数，而是在函数内重新建一个指针变量，可意尝试一些把remove函数中参数的 & 去掉看会有什么效果，具体详解可参考： https://www.cnblogs.com/li-peng/p/4116349.html 

## <font color=#DC143C>389. Find the Difference（map实现） </font>
给定两个字符串 s 和 t，它们只包含小写字母,字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母, 请找出在 t 中被添加的字母。

示例：
     
    输入：
    s = "abcd"       t = "abcde"
    输出：    e
    解释：   'e' 是那个被添加的字母
code：
```c
class Solution 
{
public:
    char findTheDifference(string s, string t) 
    {
       map<char,int > data;
       for(auto c:s)
           data[c]++;
        for(auto c:t)
            if(--data[c]<0)
                return c;      
    }
};
```
<font color=00FF>总结：</font>map的关键字是 不可以 重复的,这种解法的思路就是 把源字符串用map存起来，关键字存储字符，值是出现的个数，然后在目标字符串中遇到一个源字符串中的字符时就把map对应的关键字的value减1,最后map中值小于0的那个char就是我们要找的。

## <font color=#DC143C>739. Daily Temperatures(递减栈) </font>
根据每日 气温 列表，请重新生成一个列表，对应位置的输入是你需要再等待多久温度才会升高的天数。如果之后都不会升高，请输入 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的都是 [30, 100] 范围内的整数。

code：
```c
class Solution 
{
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) 
    {
     int n = temperatures.size();
     vector<int> res(n, 0);
     stack<int> st;
     for (int i = 0; i < temperatures.size(); ++i) 
     {
            while (!st.empty() && temperatures[i] > temperatures[st.top()]) 
            {
                auto t = st.top(); st.pop();
                res[t] = i - t;
            }
            st.push(i);
     }
     return res;   
    }
};
```
<font color=#00FF>总结：</font>这道题主要是用的 <font color=#DC143C>递减栈</font> 的思想，新定义一个栈 data ，存储元素索引，该栈是一个递减的栈，元素索引对应的值是递减的。当我们访问到元素 temperatures[i] 的时候，把栈中 索引在i 之前的 值比 temperatures[i] 小的元素的索引全部pop掉，并计算他们和i之间的距离，这样当访问下一个元素时，被pop掉的元素就不用考虑了，时间节省到了这里。

## <font color=#DC143C>258. Add Digits(找规律型的题）</font>

给一个非负整数 num，反复添加所有的数字，直到结果只有一个数字。

例如: 设定 num = 38，过程就像： 3 + 8 = 11, 1 + 1 = 2。 由于 2 只有1个数字，所以返回它。

要求：不用迭代递归和循环，在 O(1) 的时间内解决问题

code：
```c
class Solution 
{
public:
    int addDigits(int num) 
    {
      return num == 0 ? 0 : (num % 9 == 0 ? 9 : num % 9);  
    }
};
```

<font color=#00FF>总结：</font> 这个题是一个纯找规律的题，好像没什么技术含量，具体参考网址：https://leetcode.com/problems/add-digits/discuss/128841/C++-onliner


##  <font color=#DC143C>242. 有效的字母异位词(3种方法) </font>
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的一个字母异位词。例如:

s = "anagram"，t = "nagaram"，返回 true

s = "rat"，t = "car"，返回 false

code:
```c
class Solution 
{
public:
    bool isAnagram(string s, string t) 
    {
      //方法1
      /*if(s.size()!=t.size())
          return false;
      map<char,int>data;
      for(auto c:s)
          data[c]++;
      for(auto c:t)
          data[c]--;
      for(auto c:s)
         if(data[c]!=0) 
             return false;
      return true;*/

      //方法2
      /*if(s.size()!=t.size()) return false;
      int sum1=0, sum2=0, sum3=0;
      for(int i=0;i<s.size();++i)
      {
            sum1 += s[i]-t[i];
            sum2 += s[i]*s[i]-t[i]*t[i];
            sum3 += s[i]*s[i]*s[i]-t[i]*t[i]*t[i];
      }
      if(!sum1 && !sum2 && !sum3 ) return true;
        return false;*/

      //方法3
      sort(s.begin(),s.end());
      sort(t.begin(),t.end());
      if(s==t)
        return true;
      else
        return false;

        
    }
};
```

<font color=#00ff>总结：</font> 这道题采用了三种方法解题，第一种和第三种个人觉得原理差不多，方法1比方法3时间要短一些，个人理解map内部实现是采用的红黑树，而sort函数在C++中数据量稍微大一些的时候采用的是快排，平均时间复杂度是nlogn，map的时间复杂度是logn，所以差别不是太大，而第二种方法不是太能理解，感觉意思是在说如果两个集合每个元素的和 ，每个元素的平方的和，立方的和相等，就能说这两个集合是等价的意思，但是自己想不通，难过,sangxin。


## <font color=#DC143C>49. 字母异位词分组 </font>
给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例:

输入: ["eat", "tea", "tan", "ate", "nat", "bat"],

输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]

code:
```C
class Solution 
{
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) 
    {
        vector<vector<string>> ret;
        map<string,int> map_str;
        for(auto str:strs)
        {
            string str_temp=str;
            sort(str.begin(),str.end());
            if(map_str.count(str)==0)
            { //进入这个语句说明在返回数组中要开辟新的一行，所以用返回数组的大小作为map的值刚好。
                map_str.insert(pair<string,int>(str,ret.size()));
                vector<string> temp_vec;
                temp_vec.push_back(str_temp);
                ret.push_back(temp_vec);
            }
            else
                ret[map_str[str]].push_back(str_temp);
            
        }
        return ret;
    }
};
```

## <font color=#DC143C>114. Flatten Binary Tree to Linked List(递归调用)</font>
Given a binary tree, flatten it to a linked list in-place. For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
```
The flattened tree should look like:
```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```
code:
```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution 
{
public:
    void flatten(TreeNode* root)
    {
        if (!root) return;
        if (root->left) flatten(root->left);
        if (root->right) flatten(root->right);
        TreeNode *tmp = root->right;
        root->right = root->left;
        root->left = NULL;
        //用root记录叶子节点的位置
        while (root->right) root = root->right;
        //把右子树放到新的右树的叶子上
        root->right = tmp;
    }
}
```
<font color=#00ff>总结：</font>本题采用的是深度优先搜索策略，题目比较难，自己懒得写思路了，是一个递归的比较好的题。

## <font color=red>541. Reverse String II (周期性局部反转字符串) </font>
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original. 

Example:
Input: s = "abcdefg", k = 2

Output: "bacdfeg"

Restrictions:

The string consists of lower English letters only.

Length of the given string and k will in the range [1, 10000]

code
```c
class Solution 
{
public:
    string reverseStr(string s, int k)
    {
      int n=s.size(); 
      for(int i=0;i<n;i+=2*k)
          reverse(s.begin()+i,s.begin()+min(i+k,n));
      return s;
    }
}
```
<font color=green> **总结：**</font>写这个题的主要目的是想记录reverse函数的用法，自己一开始是采用遍历然后分情况讨论的，代码比较长，思路也不是太好理，上面这种方法思路就比较好，reverse函数包含在algorithm头文件中，反转范围是 [   ) 这种形式的，即包括第一个参数，但是不包括最后一个参数.

## <font color=red>70. 爬楼梯 (经典递归) </font>
假设你正在爬楼梯。需要 n 步你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？注意：给定 n 是一个正整数。

示例 1：

输入： 2

输出： 2

解释： 有两种方法可以爬到楼顶。
1.  1 步 + 1 步
2.  2 步

示例 2：

输入： 3

输出： 3

解释： 有三种方法可以爬到楼顶。
1.  1 步 + 1 步 + 1 步
2.  1 步 + 2 步
3.  2 步 + 1 步

code:
```C
class Solution {
public:
    int climbStairs(int n) 
    {
      /*if(n==3)
          return 3;
      else if(n==2)
           return 2;
       else if(n==1 || n==0)
           return 1;
       else
           return (climbStairs(n-3)+2*climbStairs(n-2));*/
      std::vector<int> ways(n);
      ways[0] = 1;
      ways[1] = 2;
      for(int i = 2; i < n; i++)
      {
            ways[i] = ways[i-1] + ways[i-2];
      }
      return ways[n-1];
    }
};
```
<font color=green>总结:  </font>自己写的方法数据量一大就会超时，别人的代码思路比较简单，时间也比较短，时间主要节省在用数组将之前算过的方法存了下来，而自己的方法会重复的去算之前算过的，个人觉得这道题 **堪称经典**。

## <font color=red>121. 买卖股票的最佳时机 (经典题)</font>
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。注意你不能在买入股票前卖出股票。

示例 1:

输入: [7,1,5,3,6,4]

输出: 5

解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

示例 2:

输入: [7,6,4,3,1]

输出: 0

解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

code：
```c
class Solution 
{
public:
    int maxProfit(vector<int>& prices) 
    {
       if ( prices.size() < 1) 
            return 0;
        int min = prices[0];
        int profit = 0;

        for (int i = 1; i < prices.size(); i++) 
        {
            if (min > prices[i]) 
            {
                min = prices[i];
            } 
            else 
            {
                if (profit < prices[i] - min) 
                {
                    profit = prices[i] - min;
                }
            }
        }
        return profit;
    }
};
```
## <font color=red>231. Power of Two( n & n-1等知识点较多 ) </font>
Given an integer, write a function to determine if it is a power of two.

    Example :
    Input: 1      Output: true
    Input: 16     Output: true
    Input: 218    Output: false
code:
```c
class Solution 
{
public:
    bool isPowerOfTwo(int n) 
    {
        //方法1
        /*if(n<=0 || (n & n-1)!=0)
            return false;
        else 
            return true;*/

        //方法2
        return n>0 && (1073741824 % n == 0);
    }
};
```
<font color=green>总结：</font> 在这里我只列出了两种方法，在leetcode的讨论区有四种方法，但是个人觉得列出来的这两种方法比较新颖。方法1是利用了一个小技巧，n=n & （n-1）会把 n 的(用二进制表示时)最右边的那个1变为0，如果一个数是2的次幂，则这个数用二进制表示时，在各个位上就只有一个1,所以当我们把最右边那个1变为0时，n应该也变成了0;第二种方法是利用了<font color=red>**数学知识**</font>：

Because the range of an integer = -2147483648 (-2^31) ~ 2147483647 (2^31-1), the max possible power of two = 2^30 = 1073741824.

(1) If n is the power of two, let n = 2^k, where k is an integer.

We have 2^30 = (2^k) * 2^(30-k), which means (2^30 % 2^k) == 0.

(2) If n is not the power of two, let n = j*(2^k), where k is an integer and j is an odd number.

We have (2^30 % (j*(2^k))) != 0.

<font color=green>**引申：**</font>与之类似的还有两个题目分别是是不是3的次幂和是不是4的次幂，是不是3的次幂没有巧妙的方法，只能判断该数除以3余数是不是0或者用上面的类似的数学知识;4，8,16...的次幂的判断还是有一定技巧的，直接上代码：
```c
class Solution {
public:
    bool isPowerOfFour(int num) 
    {
     return num > 0 && (num & (num - 1)) == 0 && (num & 0x55555555) != 0;  //0101 0101 0101 0101 0101 0101 0101 0101
    }
};
class Solution {
public:
    bool isPowerOfEight(int num) 
    {
     return num > 0 && (num & (num - 1)) == 0 && (num & 0x49249249) != 0;  // 0100 1001 0010 0100 1001 0010 0100 1001
    }
};
```
 int 型正数用二进制表示时最高位为0.

## <font color=red>202. 快乐数(小有成就的题)</font>
编写一个算法来判断一个数是不是“快乐数”。一个“快乐数”定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数。

code:
```c
class Solution {
public:
    bool isHappy(int n) 
    {
       set<int> nums;
        while(n!=1)
        {
          int temp=0;
          while(n)
          {
              temp+=(n%10)*(n%10);
              n=n/10;
          }
          n=temp;
          if(nums.count(n)==0)
              nums.insert(n);
          else if(n==1)
              return true;
          else
              return false;
              
        }
        return true;
    }
};
```


