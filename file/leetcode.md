# leetcode刷题

## 2021-1-22

### [989. 数组形式的整数加法](https://leetcode-cn.com/problems/add-to-array-form-of-integer/)

对于非负整数 X 而言，X 的数组形式是每位数字按从左到右的顺序形成的数组。例如，如果 X = 1231，那么其数组形式为 [1,2,3,1]。

给定非负整数 X 的数组形式 A，返回整数 X+K 的数组形式。



#### 题解

```c
        //模拟题
        //考虑到A的长度可能为10000 所以其不能用一个long long来表示
        //只能通过模拟加法来实现
        //有点类似双链表合并
        int size=A.size();//获取A的长度
        //对K的每一位做加法
        int i=0;
        vector <int> temp,result;
        int carry = 0;//carry bit  
        while(K!=0||i<size){//K未结束或者数组未结束
            int number=0;
            if(size-i-1>=0)
            number = A[size-i-1];
            number+=K%10;//位加法
            K=K/10;
            number+=carry; //add carry bit
            carry = 0; //reset carry bit
            if(number>9){//处理进位
                number-=10;
                carry ++;
            }
            temp.push_back(number);//push number
            i++;//下一位
        }
        //直接不处理进位 留到后面一起处理
        //考虑到处理99999+1带来的进位
        //可能要多加一位
        if(carry==1){
            temp.push_back(carry);
        }
        //反转
        for(i=temp.size()-1;i>=0;--i){
            result.push_back(temp[i]);
        }
        return result;
```



## 2021-1-23

### [1319. 连通网络的操作次数](https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/)



用以太网线缆将 n 台计算机连接成一个网络，计算机的编号从 0 到 n-1。线缆用 connections 表示，其中 connections[i] = [a, b] 连接了计算机 a 和 b。

网络中的任何一台计算机都可以通过网络直接或者间接访问同一个网络中其他任意一台计算机。

给你这个计算机网络的初始布线 connections，你可以拔开任意两台直连计算机之间的线缆，并用它连接一对未直连的计算机。请你计算并返回使所有计算机都连通所需的最少操作次数。如果不可能，则返回 -1 。 

#### 思路

首先从图论角度来说，如果是一个连通图的话，那么边的个数一定大于等于点的个数减一，所以我们可以根据这个来判断是否能把这个计算机网络连接起来。

接下来我们考虑如何连接，我们将整个网络看成是一张图，已经连接起来的就是一个连通图，在某一个连通图内部已经满足了题目的要求，但是在不同的连通块之间，是不符合要求的。显然，我们首先要做的就是把两个连通块通过一根线连起来。考虑到前面说的，这里的线的个数总是满足题目的要求的，所以我们总能在某一个连通块里找到一根线，当我们去除它以后，该连通块的联通性不发生变化。基于该结论，我们总可以在某个连通块里找到一根多余的线，让他可以接到另一个联通块里，这样我们就可以连接两个连通块。同理，对于整个图来说，假设有n个连通块，其一定可以找到n-1个多余的线，而不论这n-1个多余的线在哪个连通块，我们总可以让他经过一次操作就连到需要连接的块上。简单的来说，如果把每个块都看成一个点，那么这些点的度是确定的，我们只需要根据这些度进行一些组合就可以了。所以我们可以得出，需要操作的次数就是连通块的个数-1。基于此结论，我们可以使用并查集来解决问题。

#### 题解

```c
class Solution {
public:
    int makeConnected(int n, vector<vector<int>>& connections) {
        //edge = node -1  否则无法实现 
        if(n-1>connections.size()) return -1;
        //计算次数
        //思路
        //假设有四个连通块 那么只需要三条就可以连起来
        //又因为前面已经保证了线是一定够得
        //所以我们只需要考虑有多少个连通块即可
        //result = 点连通块+块连通块-1
        //点连通块等于点个数减去在块连通块里的
        //简单的计算连通块 通过并查集实现
        int number[n]; 
        int setNumber = n;
        for(int i=0;i<n;++i){
            number[i]=i;//自己指向自己
        }
        int size = connections.size();
        for(int i=0;i<size;++i){
            if(find(number,connections[i][0])!=find(number,connections[i][1])){
                //需要合并 合并意味着两个变一个
                number[find(number,connections[i][0])]=find(number,connections[i][1]);
                setNumber--;
            }
        }
        return setNumber-1; 
    }
    int find(int fa[],int x) //查询函数
    {
    if(fa[x] == x)
        return x;
    else
        return find(fa,fa[x]);
    }
};
```



## 2021-1-24

### [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)



给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。



#### 思路

初看像dp，实际上是一道模拟的感觉，按照题意直接模拟即可。



#### 题解

```c
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        //特例
        if(nums.size()==0)  return 0;
        int length=1;
        int max=1;
        int size = nums.size();
        for(int i=0;i<size-1;++i){
            if(nums[i]<nums[i+1]){//是递增
                length++;
                if(length>max)
                    max=length;
            }
            else{
                length =1;
            }
        }
        return max;
    }
};
```





###  [1502. 判断能否形成等差数列](https://leetcode-cn.com/problems/can-make-arithmetic-progression-from-sequence/)



给你一个数字数组 arr 。

如果一个数列中，任意相邻两项的差总等于同一个常数，那么这个数列就称为 等差数列 。

如果可以重新排列数组形成等差数列，请返回 true ；否则，返回 false 。



#### 思路

先进行排序，再判断是否能成为等差数列。



#### 题解

```c
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        int size = arr.size();
        /*for(int i = 0;i<size;++i){
            for (int j=0;j<size-i-1;++j){
                if(arr[j]>arr[j+1]){
                    //swap
                    int temp = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = temp;
                }
            }
        }*/
        sort(arr.begin(),arr.end());
        int d = arr[1]-arr[0];
        for(int i=1;i<size-1;++i){
            if(arr[i+1]-arr[i]!=d)
                return false;
        }
        return true;
    }
};
```





### [LCP 06. 拿硬币](https://leetcode-cn.com/problems/na-ying-bi/)

桌上有 `n` 堆力扣币，每堆的数量保存在数组 `coins` 中。我们每次可以选择任意一堆，拿走其中的一枚或者两枚，求拿完所有力扣币的最少次数。



#### 思路

可以证明偶数拿完就是n/2,奇数拿完就是n/2+1,结合c除法向下取整特性,可以得到 ,n拿完次数为 (n+1)/2



#### 题解

```c
class Solution {
public:
    int minCount(vector<int>& coins) {
        //可以证明偶数拿完就是n/2 奇数拿完就是n/2+1
        //结合c除法向下取整特性 可以得到
        //n拿完次数为 (n+1)/2
        int sum=0;
        int size = coins.size();
        for(int i =0;i<size;++i){
            sum+=(coins[i]+1)/2;
        }
        return sum;
    }
};
```



### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)



输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。



#### 思路

考虑到链表无法获取长度，最小时间复杂度也要两次遍历。



#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector <int > result;
        while(head){
            result.push_back(head->val);
            head=head->next;
        }
        int size = result.size();
        for(int i=0;i<size/2;++i){
            int temp = result[i];
            result[i]=result[size-i-1];
            result[size-i-1]=temp;
        }
        return result;
    }
};
```



## 2021-1-25

### [18. 四数之和](https://leetcode-cn.com/problems/4sum/)



给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。



#### 思路

首先考虑O4的循环，然后通过set来判断是否有重复，失败。

接着考虑先对数据进行排序，然后进行O4循环，再通过set判断重复，失败（超时）。

之后考虑到双指针的方法，将a+b+c+d=target可以改写成c+d=target-a-b。联想到双指针方法，于是我们可以固定a和b的值，然后改变通过双指针方法来改变c+d寻找是否有目标值即可。

也即a循环下套b循环，b循环内是双指针，这样时间复杂度可以降低到$O(n^3)$



#### 题解

```c
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int size=nums.size();
        sort(nums.begin(),nums.end());
        set<vector<int>> result;
        vector<vector<int>> line;
        if(size<4)
            return line;
        int i=0,j=1,k=2,h=size-1;
        while(i!=size-3){
            //移动后面两个
            while(k!=h){
                if(nums[i]+nums[j]+nums[k]+nums[h]==target){
                vector<int> a={nums[i],nums[j],nums[k],nums[h]};
                if(result.find(a)==result.end()){
                        result.insert(a);
                        line.push_back(a);
                }
                k++;
                }
                else if(nums[i]+nums[j]+nums[k]+nums[h]<target){
                    k++;
                }
                else{
                    h--;
                }

            }
            //移动i
            if(j==size-3){//j到顶了
                i++;
                j=i+1;
            }
            //移动j
            else{ //正常到顶
                j++;
            }
            k=j+1;
            h=size-1;
        }
        return line;
    }
};
```





### [959. 由斜杠划分区域](https://leetcode-cn.com/problems/regions-cut-by-slashes/)

在由 1 x 1 方格组成的 N x N 网格 grid 中，每个 1 x 1 方块由 /、\ 或空格构成。这些字符会将方块划分为一些共边的区域。

（请注意，反斜杠字符是转义的，因此 \ 用 "\\" 表示。）。

返回区域的数目。



#### 思路

一开始并没有想到特别好的解法，直到看到有题解中说到将一个小方格根据对角线划分成四份，这时候我才想到解法。

划分以后，这个就变成了一个有0有1的图，我们要在这个图中找联通分量，因此我们可以选择bfs dfs或者并查集。

在这里，我们采取了并查集。

如果是空格，那么就将四块合并，如果是/，就两个两个合并，\也是两个两个合并。

之后，我们考虑块与块的关系，显然，一个块的右部分会和它的右边块的左部分无条件合并，一个块的下部分会和它的下边块的上部分无条件合并。这样我们就可以得到解。



#### 题解

```c
class Solution {
public:
    int regionsBySlashes(vector<string>& grid) {
        int size = grid.size();
        //考虑每个小格子划分为四块 
        //分别为0 1 2 3
        //使用并查集
        vector <int> Grid;
        for(int i =0;i<size*size*4;++i){
            Grid.push_back(i);
        }
        for(int i=0;i<size;++i){
            for(int j=0;j<size;++j){
                //处理块内的合并
                int position = (i*size+j)*4;
                if(grid[i][j]==' '){// 全部合并
                    Union(Grid,position+0,position+1);
                    Union(Grid,position+0,position+2);
                    Union(Grid,position+0,position+3);
                }
                else if(grid[i][j]=='\\'){
                    // 02   13
                    Union(Grid,position+0,position+2);
                    Union(Grid,position+1,position+3);
                }
                else {
                    Union(Grid,position+2,position+3);
                    Union(Grid,position+1,position+0);
                }
                //块外合并
                if(j<size-1){//不是最右边
                    Union(Grid,position+2,position+5);
                }
                if(i<size-1){//不是最下
                    Union(Grid,position+3,((i+1)*size+j)*4);
                }
            }
        }
        set<int > father;
        for(int i =0;i<size*size*4;++i){
            father.insert(find(Grid,i));
        }
        return father.size();
    }
    int find(vector<int>& grid,int x){
        while(grid[x]!=x){
            x=grid[x];
        }
        return x;
    }

    void Union(vector<int>& grid,int x,int y){
        grid[find(grid,x)]=find(grid,y);
    }
};
```



### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

给你一个 32 位的有符号整数 x ，返回 x 中每位上的数字反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）。



#### 思路

题目的难点在于如何判断溢出。

我们假定每次是这样的：
$$
sum=sum*10+x\%10;
$$
如果sum>INT_MAX，那么就有
$$
sum>=INT\_MAX
$$
如果$sum=INT\_MAX$，那我们重点关注x%10的值，当最大值时，其二进制为7FFF_FFFF，所以最后一位十进制是7，只要加的数大于7就会溢出。同样的，如果是负数的时候，其二进制最后一位是8，所以如果加的数小于-8，就会溢出。



#### 题解

```c
class Solution {
public:
    int reverse(int x) {
        int num=0;
        while(x!=0){
            if(num>INT_MAX/10||(num==INT_MAX/10&&x%10>7))
                return 0;
            else if(num<INT_MIN/10||(num==INT_MIN/10&&x%10<-8))
                return 0;
            num=num*10+x%10;
            x=x/10;
        }
        return num;
    }
};
```



### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)



判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。



#### 思路

首先考虑到数字反转，如果该数是回文数的话，那么其反转就是不能溢出的，因为其本身和反转是一样的。所以可以反转，然后来判断是否一样，当然要考虑反转后是否会溢出。

此外，我们可以考虑对每一位进行字符串的比对，也即用vector存储每一位，然后正反比较。

当然，我们也可以考虑只存储一半的数据进行比较。



#### 题解1 (较慢速度与较大空间)

```c
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0)  
            return false;
        vector<int> left(32),right(32);
        int i=0;
        while(x!=0){
            left[i]=x%10;
            right[31-i]=x%10;
            i++;
            x=x/10;
        }
        i--;
        int j=i;
        for(;i>=0;--i){
            if(left[i]!=right[31+i-j])
                return false;
        }
        return true;
    }
};
```



#### 题解2

```c
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0)  
            return false;
        vector<int> num;
        int i=0;
        while(x!=0){
            num.push_back(x%10);
            i++;
            x=x/10;
        }
        int size=num.size();
        for(i=0;i<size/2;++i){
            if(num[i]!=num[size-1-i])
                return false;
        }
        return true;
    }
};
```



#### 题解3

```c
class Solution {
public:
    bool isPalindrome(int x) {
        int X=x;
        if(x<0)  
            return false;
        int num=0;
        while(x!=0){
            if(num>INT_MAX/10||(num==INT_MAX/10&&x%10>7))
                return false;
            else if(num<INT_MIN/10||(num==INT_MIN/10&&x%10<-8))
                return false;
            num=num*10+x%10;
            x=x/10;
        }
        if(num==X)
            return true;
        return false;
    }
};
```





### [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。



#### 思路

类似于自动机，进行遍历判断就可以。

进阶：构造哈希表，来对比。



#### 题解

```c
class Solution {
public:
    int romanToInt(string s) {
        int sum=0;
        int size=s.length();
        for(int i=0;i<size;++i){
            if(s[i]=='I'){
                if(i<size-1&&s[i+1]=='V'){
                    sum+=4;
                    i++;
                }
                else if(i<size-1&&s[i+1]=='X'){
                    sum+=9;
                    i++;
                }
                else 
                    sum+=1;
            }
            else if(s[i]=='X'){
                if(i<size-1&&s[i+1]=='L'){
                    sum+=40;
                    i++;
                }
                else if(i<size-1&&s[i+1]=='C'){
                    sum+=90;
                    i++;
                }
                else
                    sum+=10;
            }
            else if(s[i]=='C'){
                if(i<size-1&&s[i+1]=='D'){
                    sum+=400;
                    i++;
                }
                else if(i<size-1&&s[i+1]=='M'){
                    sum+=900;
                    i++;
                }
                else 
                    sum+=100;
            }
            else if(s[i]=='V')
                sum+=5;
            else if(s[i]=='L')
                sum+=50;
            else if(s[i]=='D')
                sum+=500;
            else 
                sum+=1000;
        }
        return sum;
    }
};
```



### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)



编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 `""`。



#### 思路

我们遍历每个串，看他的第n位是否都一致。那么（0，n-1）就是公共前缀。



#### 题解

```c
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string a="";
        int size=strs.size();
        if(size==0) return a;
        else if(size==1)   return strs[0];
        //直接暴力寻找
        int min=200;
        for(int i=0;i<size;++i){
            if(strs[i].length()<min)
                min = strs[i].length();
        }
        int i=0;
        for(;i<min;++i){
            int flag=0;
            for(int j=0;j<size-1;++j){
                if(strs[j][i]!=strs[j+1][i])
                    flag=1;
            }
            if(flag==1)
                break;
        }
        if(i!=0){
            a.assign(strs[0],0,i);
        }
        return a;
    }
};
```



###  [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。



#### 思路

利用栈进行匹配即可。



#### 题解

```c
class Solution {
public:
    bool isValid(string s) {
        stack <char> stack;
        int size = s.length();
        int top=0;
        for(int i=0;i<size;++i){
            if(s[i]=='('||s[i]=='['||s[i]=='{'){
                //push
                top++;
                stack.push(s[i]);
            }
            else if(top!=0&&((s[i]==')'&&stack.top()=='(')
            ||(s[i]==']'&&stack.top()=='[')
            ||(s[i]=='}'&&stack.top()=='{'))){
                //pop
                top--;
                stack.pop();
            }
            else{
                return false;
            }
        }
        if(top==0)
            return true;
        return false;
    }
};
```





## 2021-1-26

### [等价多米诺骨牌对的数量](https://leetcode-cn.com/problems/number-of-equivalent-domino-pairs/)



给你一个由一些多米诺骨牌组成的列表 dominoes。

如果其中某一张多米诺骨牌可以通过旋转 0 度或 180 度得到另一张多米诺骨牌，我们就认为这两张牌是等价的。

形式上，dominoes[i] = [a, b] 和 dominoes[j] = [c, d] 等价的前提是 a==c 且 b==d，或是 a==d 且 b==c。

在 0 <= i < j < dominoes.length 的前提下，找出满足 dominoes[i] 和 dominoes[j] 等价的骨牌对 (i, j) 的数量。



#### 思路

首先考虑暴力法，可以但没必要。

接着发现牌的数字都在1-9之间，考虑二位数的映射，这样就能得到一个解法。

于是考虑用9*9数组保存数据，然后进行$C_n^2$的运算即可。



#### 题解

```c
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        int num[9][9];
        //初始化
        for(int i=0;i<9;++i){
            for(int j=0;j<9;++j){
                num[i][j]=0;
            }
        }
        //遍历扫描得到出现次数
        int size=dominoes.size();
        for(int i=0;i<size;++i){
            num[dominoes[i][0]-1][dominoes[i][1]-1]++;
        }
        int sum=0;
        //遍历二维数组计算出现次数
        for(int i=0;i<9;++i){
            for(int j=i;j<9;++j){
                //C_n2=(n)(n-1)/2
                if(j==i) 
                sum+=(num[i][j])*(num[i][j]-1)/2;
                else
                sum+=(num[i][j]+num[j][i])*(num[i][j]+num[j][i]-1)/2;
            }
        }
        return sum;
    }
};
```



### [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)



将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

#### 思路

直接根据大小合并，需要判断一个遍历完一个没完的情况。



#### 题解

````c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //特例
        if(l1==nullptr&&l2==nullptr) return nullptr;
        else if(l1==nullptr) return l2;
        else if(l2==nullptr) return l1;
        //normal
        ListNode *head,*p;
        head=(l1->val > l2->val)?l2:l1;
        (l1->val > l2->val)?l2=l2->next:l1=l1->next;
        p=head;
        while(l1!=nullptr&&l2!=nullptr){
            //合并
            if(l1->val > l2->val){
                head->next=l2;
                head=head->next;
                l2=l2->next;
            }
            else{
                head->next=l1;
                head=head->next;
                l1=l1->next;
            }
        }
        if(l1!=nullptr)
            head->next=l1;
        else if(l2!=nullptr)
            head->next=l2;
        return p;
    }
};
````





### [两数相加](https://leetcode-cn.com/problems/add-two-numbers/)



给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。



#### 思路

直接模拟，注意考虑进位以及如果是9的话可能会需要自己创建一个节点。

注意计算时候的循环赋值问题而出现的错误。

如

```c
			l1->val=(l1->val+carry)%10;
            carry=(l1->val+carry)>=10?1:0;
```





#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head =l1;
        //考虑进位
        int carry=0;//1:carry
        while(l1!=nullptr&&l2!=nullptr){
            int sum=l1->val+l2->val+carry;
            l1->val=(l1->val+l2->val+carry)%10;
            carry=sum>=10?1:0;
            //break
            if(l1->next==nullptr||l2->next==nullptr)
                break;
            //next
            l1=l1->next;
            l2=l2->next;
        }
        //l2未结束l1结束 直接接上
        if(l2->next!=nullptr){
            l1->next=l2->next;
            l2->next=nullptr;
        }
        //这样总是实现l1是长于l2的
        while(l1->next!=nullptr){
            l1=l1->next;
            int sum=l1->val+carry;
            l1->val=sum%10;
            carry=sum>=10?1:0;
        }
        if(carry==1){
            l1->next=new ListNode(1);
        }
        return head;
    }
};
```



### [寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。

进阶：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？





#### 思路

将两个数组合并，取中位数。

进阶：直接只合并到一半的情况即可。

再进阶：从中间开始查找，二分法。



#### 题解1

```c
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector <int> num;
        int size1=nums1.size();
        int size2=nums2.size();
        int i=0,j=0;
        while(i<size1&&j<size2){
            if(nums1[i]<=nums2[j]){
                num.push_back(nums1[i]);
                i++;
            }
            else {
                num.push_back(nums2[j]);
                j++;
            }
        }
        while(i<size1){
            num.push_back(nums1[i]);
            i++;
        }
        while(j<size2){
            num.push_back(nums2[j]);
            j++;
        }
        int size=num.size();
        if(size%2==0){
            return (double(num[size/2-1])+double(num[size/2]))/2;
        }
        else 
            return double(num[size/2]);
    }
};
```



#### 题解2

```c
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector <int> num;
        int size1=nums1.size();
        int size2=nums2.size();
        int Size=size1+size2;
        int i=0,j=0;
        while(i<size1&&j<size2){
            if(nums1[i]<=nums2[j]){
                num.push_back(nums1[i]);
                i++;
                if(i+j>Size/2){
                    break;
                }
            }
            else {
                num.push_back(nums2[j]);
                j++;
                if(i+j>Size/2){
                    break;
                }
            }
        }
        while(i<size1){
            num.push_back(nums1[i]);
            i++;
            if(i+j>Size/2){
                    break;
                }
        }
        while(j<size2){
            num.push_back(nums2[j]);
            j++;
            if(i+j>Size/2){
                break;
            }
        }
        if(Size%2==0){
            return double(num[Size/2-1]+num[Size/2])/2;
        }
        else 
            return double(num[Size/2]);
    }
};
```



### [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。





#### 思路

一开始想的是直接删除即可，注意好数组的变化情况就行。

查看题解后意识到用erase实在是太慢了一点，其实只需要用双指针把不一样的往前放就可以了。



#### 题解

```c
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        for(int i=1;i<nums.size();++i){
            if(nums[i]==nums[i-1]){
                nums.erase(nums.begin()+i,nums.begin()+1+i);
                i--;
            }
        }
        return nums.size();
    }
};
```





## 2021-1-31



###  [实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)



实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。



#### 思路

先看第一个字符是否一样，一样的话就尝试匹配，否则往后。



#### 题解

```c
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle=="") return 0;
        int size=haystack.length();
        int length=needle.length();
        for(int i=0;i<size;++i){
            if(haystack[i]==needle[0]){
                //比对
                int j=1;
                for(;j<length;++j){
                    if(haystack[i+j]!=needle[j])
                        break;
                }
                if(j==length) return i;
            }
        }
        return -1;
    }
};
```





### [移除元素](https://leetcode-cn.com/problems/remove-element/)

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。





#### 思路

因为是就地删除，等于就是把后面不等于val的元素往前移就行，那么我们只要记录下往前移的位置即可。



#### 题解

```c
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size=nums.size();
        int j=0;
        for(int i=0;i<size;++i){
            if(nums[i]!=val){
                //不相等
                nums[j]=nums[i];
                j++;
            }
                //相等 则i动 j不动
        }
        return j;
    }
};
```



### [搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。



#### 思路

直接遍历，第一个大于等于其的就是，如果没有则返回数组长度。



#### 题解

```c
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int size=nums.size();
        for (int i=0;i<size;++i){
            if(nums[i]==target)
                return i;
            else if(nums[i]>target){
                return i;
            }
        }
        return size;
    }
};
```





###  [外观数列](https://leetcode-cn.com/problems/count-and-say/)



给定一个正整数 n ，输出外观数列的第 n 项。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。

你可以将其视作是由递归公式定义的数字字符串序列：

countAndSay(1) = "1"
countAndSay(n) 是对 countAndSay(n-1) 的描述，然后转换成另一个数字字符串。
前五项如下：

1.     1
2.     11
3.     21
4.     1211
5.     111221
第一项是数字 1 
描述前一项，这个数是 1 即 “ 一 个 1 ”，记作 "11"
描述前一项，这个数是 11 即 “ 二 个 1 ” ，记作 "21"
描述前一项，这个数是 21 即 “ 一 个 2 + 一 个 1 ” ，记作 "1211"
描述前一项，这个数是 1211 即 “ 一 个 1 + 一 个 2 + 二 个 1 ” ，记作 "111221"
要 描述 一个数字字符串，首先要将字符串分割为 最小 数量的组，每个组都由连续的最多 相同字符 组成。然后对于每个组，先描述字符的数量，然后描述字符，形成一个描述组。要将描述转换为数字字符串，先将每组中的字符数量用数字替换，再将所有描述组连接起来。



#### 思路

直接模拟+递归



#### 题解

```c
class Solution {
public:
    string countAndSay(int n) {
        string x="1";
        for(int i=0;i<n-1;++i){
            int size=x.length();
            int length=1;
            string a="";
            for(int i=0;i<size;++i){
                if(x[i+1]!=x[i]||i==size-1){
                    //加入新字符
                    a=a+to_string(length)+x[i];
                    length=1;
                }
                else{
                    //下一个相等
                    length++;
                }
            }
            x=a;
        }
        return x;
    }
//递归函数
    string say(string x){
        int size=x.length();
        int length=1;
        string a="";
        for(int i=0;i<size;++i){
            if(x[i+1]!=x[i]||i==size-1){
                //加入新字符
                a=a+to_string(length)+x[i];
                length=1;
            }
            else{
                //下一个相等
                length++;
            }
        }
        return a;
    }
};
```





### [加一](https://leetcode-cn.com/problems/plus-one/)



给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。



#### 思路

从后往前，按位加，如果逢十要进一。



#### 题解

```c
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int size=digits.size();
        int carry=0;
        digits[size-1]++;
        //如果是两位数
        if(digits[size-1]==10){
            carry=1;
            digits[size-1]=0;//进位
        }
        else
            return digits;
        
        int i=size-2;
        //往前进位
        while(carry==1&&i>=0){
            digits[i]++;
            carry=0;
            if(digits[i]==10){
                carry=1;
                digits[i]=0;
                i--;
            }
        }
        if(carry==1){
            digits.insert(digits.begin(),1);
        }
        return digits;
    }
};
```





### [二进制求和](https://leetcode-cn.com/problems/add-binary/)



给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`。



#### 思路

把长的作为被加数，短的作加数，进行相加。



#### 题解

```c
class Solution {
public:
    string addBinary(string a, string b) {
        return (a.size()>b.size())?add(a,b):add(b,a);
    }
    
    string add(string a,string b){
        //a长b短
        int sizea=a.size();
        int sizeb=b.size();
        int carry=0;
        int i=0;
        for(;i<sizeb;++i){
            int sum=a[sizea-1-i]+b[sizeb-1-i]-'0'+carry;//加
            carry=0;
            a[sizea-1-i]=sum;
            if(sum>='2'){
                carry=1;
                a[sizea-1-i]=sum-2;
            }
        }
        //b已经加完 考虑进位情况
        while(carry==1&&i<sizea){
            int sum=a[sizea-1-i]+carry;//加
            carry=0;
            a[sizea-1-i]=sum;
            if(sum=='2'){
                carry=1;
                a[sizea-1-i]='0';
            }
            i++;
        }
        if(carry==1) //进位
            return "1"+a;
        else
            return a;
    }
};
```





### [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)



假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。



#### 思路

dp。往前考虑-1和-2的情况创建状态方程。



#### 题解

````c
class Solution {
public:
    int climbStairs(int n) {
        //dp
        //f[a]表示爬到a方法数
        //那么f[a]=f[a-1]+f[a-2]
        //不需要考虑a-2是1 1的情况 因为那样就变成了a-1 +1
        //也即我们只需要倒着看就可以
        if(n==1) return 1;
        else if(n==2) return 2;
        vector <int> a(n+1);
        a[1]=1;
        a[2]=2;
        for(int i=3;i<n+1;++i){
            a[i]=a[i-1]+a[i-2];
        }
        return a[n];
    }

};
````





### [ 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。



#### 思路

set去重，适用于全部的情况



#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head==nullptr) return head;
        set <int> a;
        ListNode *p=head->next,*q=head;
        a.insert(head->val);//插入当前
        while(p!=nullptr){
            //遍历下个
            if(a.find(p->val)==a.end()){
                //未出现过
                a.insert(p->val);
                q=q->next;
            }
            else{
                //出现过
                q->next=p->next;
            }
            p=p->next;
        }
        return head;
    }
};
```



### [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)



给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。



#### 思路

合并，考虑合并后会多元素，要注意pop掉



#### 题解

```c
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int j=0;
        int i=0;
        while(i<n){
            if(nums2[i]>nums1[j]&&j<m+i){
                //2》1
                j++;
            }
            else {
                //1<=2
                //要插入
                nums1.insert(nums1.begin()+j,nums2[i]);
                j++;
                i++;
            }
        }
        for(int i=0;i<n;i++){
            nums1.pop_back();
        }
    }
};
```







## 2021-2-1

### [公平的糖果棒交换](https://leetcode-cn.com/problems/fair-candy-swap/)



爱丽丝和鲍勃有不同大小的糖果棒：A[i] 是爱丽丝拥有的第 i 根糖果棒的大小，B[j] 是鲍勃拥有的第 j 根糖果棒的大小。

因为他们是朋友，所以他们想交换一根糖果棒，这样交换后，他们都有相同的糖果总量。（一个人拥有的糖果总量是他们拥有的糖果棒大小的总和。）

返回一个整数数组 ans，其中 ans[0] 是爱丽丝必须交换的糖果棒的大小，ans[1] 是 Bob 必须交换的糖果棒的大小。

如果有多个答案，你可以返回其中任何一个。保证答案存在。



#### 思路

先计算两者的和，然后可以算出两者的差值为sum，可以知道交换的数应该满足A=B+sum/2，所以根据这个，把B的数据存在set中，遍历A查找set。



#### 题解

```c
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& A, vector<int>& B) {
        set <int> Bob;//map存储
        int sizea=A.size();
        int sizeb=B.size();
        int sum=0;//用来统计差值
        for(int i=0;i<sizea;++i){
            sum+=A[i];
        }
        for(int i=0;i<sizeb;++i){
            sum-=B[i];
            Bob.insert(B[i]);//存储b的值
        }
        //此时sum存储的就是差值
        vector<int> ans;
        for(int i=0;i<sizea;++i){
            if((Bob.find(A[i]-sum/2))!=Bob.end()){
                ans.push_back(A[i]);
                ans.push_back(A[i]-sum/2);
                break;
            }
        }
        return ans;
    }
};
```



### [Excel表列序号](https://leetcode-cn.com/problems/excel-sheet-column-number/)

给定一个Excel表格中的列名称，返回其相应的列序号。

例如，

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...



#### 思路

本质是一个26进制转10进制。



#### 题解

```c
class Solution {
public:
    int titleToNumber(string s) {
        //26进制
        int sum=0;
        int size=s.length();
        for(int i=0;i<size;++i){
            int add=s[i]-'A'+1;
            //如果不先算add 可能会在执行的过程中出现溢出的情况
            sum=26*sum+add;
        }
        return sum;
    }
};
```





### [用户分组](https://leetcode-cn.com/problems/group-the-people-given-the-group-size-they-belong-to/)



有 n 位用户参加活动，他们的 ID 从 0 到 n - 1，每位用户都 恰好 属于某一用户组。给你一个长度为 n 的数组 groupSizes，其中包含每位用户所处的用户组的大小，请你返回用户分组情况（存在的用户组以及每个组中用户的 ID）。

你可以任何顺序返回解决方案，ID 的顺序也不受限制。此外，题目给出的数据保证至少存在一种解决方案。

 

#### 思路

先进行粗分组，根据所在用户组进行分类，然后在进行细分组。



#### 题解

```c
class Solution {
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        map<int,int> mp;//指向对应的vector
        vector<vector<int>> type;//分类
        vector<vector<int>> result;//ans
        int size=groupSizes.size();
        map<int,int>::iterator it;
        for(int i=0;i<size;++i){
            it=mp.find(groupSizes[i]);
            if(it!=mp.end()){
                //能找到
                type[it->second].push_back(i);//加入
            }
            else{
                //找不到
                vector<int> a;//新建vector
                a.push_back(i);//插入数据
                type.push_back(a);//插入a
                mp.insert(make_pair(groupSizes[i],type.size()-1));//插入map
            }
        } 
        //接下来遍历type分拆数据
        size=type.size();
        for(int i=0;i<size;++i){
            int length=groupSizes[type[i][0]];//获取长度
            int time=type[i].size()/length;
            for(int j=0;j<time;++j){
                vector <int> a;//新建vector
                for(int k=0;k<length;++k){
                    a.push_back(type[i][j*length+k]);
                }
                result.push_back(a);
            }
        }
        return result;
    }
};
```





### [棒球比赛](https://leetcode-cn.com/problems/baseball-game/)



你现在是一场采用特殊赛制棒球比赛的记录员。这场比赛由若干回合组成，过去几回合的得分可能会影响以后几回合的得分。

比赛开始时，记录是空白的。你会得到一个记录操作的字符串列表 ops，其中 ops[i] 是你需要记录的第 i 项操作，ops 遵循下述规则：

整数 x - 表示本回合新获得分数 x
"+" - 表示本回合新获得的得分是前两次得分的总和。题目数据保证记录此操作时前面总是存在两个有效的分数。
"D" - 表示本回合新获得的得分是前一次得分的两倍。题目数据保证记录此操作时前面总是存在一个有效的分数。
"C" - 表示前一次得分无效，将其从记录中移除。题目数据保证记录此操作时前面总是存在一个有效的分数。
请你返回记录中所有得分的总和。



#### 思路

利用栈执行



#### 题解

```c
class Solution {
public:
    int calPoints(vector<string>& ops) {
        stack <int> s;
        int top=-1;
        int size=ops.size();
        int sum=0;
        for(int i=0;i<size;++i){
            if(ops[i].compare("C")==0){
                //pop
                sum-=s.top();
                s.pop();
            }
            else if(ops[i].compare("D")==0){
                //*
                sum+=s.top()*2;
                s.push(s.top()*2);
            }
            else if(ops[i].compare("+")==0){
                //+
                int number1=s.top();
                s.pop();
                int number2=s.top()+number1;
                s.push(number1);
                sum+=number2;
                s.push(number2);
            }
            else{
                //number
                s.push(stoi(ops[i]));
                sum+=s.top();
            }
        }
        return sum;

    }
};
```





## 2021-2-2

### [替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

注意：字符串长度 和 k 不会超过 $10^4$。



#### 思路

法一：根据数学推导的结果，暴力遍历。

法二：滑动窗口



#### 题解一

```c
class Solution {
public:
    int characterReplacement(string s, int k) {
        //从数学角度推导
        //如果第一个字符不是我们替换的，那么换到下一个字符也是可以的
        //例如k=2 ABCDD 第一个为A 我们可以替换BC 这样我们移到B
        //替换成ABBBD也是可以的 所以我们可以认为第一个就是替换的字符
        //也即我们寻找从A字符出发 如果替换K个不为A字符的字符的最大长度
        int size=s.length();
        int i=0,j=0;
        int length=1;
        int sum=1;
        while(i<size-k){
            for(j=0;j<k+length&&i+j+1<size;++j){
                //dynamic change length
                if(s[i+1+j]==s[i]){
                    length++;
                    if(length>sum) 
                        sum=length;
                }
            }
            length=1;
            i++;
        }
        return (sum+k>size)?size:sum+k;
    }
};
```



#### 题解二

```c
//滑动窗口，也即我们考虑一个长度为k+max的窗口
//通过right和left画出一个长度的窗口 我们找出其中的最大出现次数
//考虑是否符合就行
//求最大值只需要在每次右移的时候考虑 因为左移是减少
class Solution {
public:
    int characterReplacement(string s, int k) {
        int size=s.length();
        vector<int> num(26);
        int left=0,right=0;
        int max=0;
        while(right<size){
            //没滑到最右边
            //计算max
            num[s[right]-'A']++;
            max=(max>num[s[right]-'A'])?max:num[s[right]-'A'];
            if(right-left+1-max>k){//无法用k个字符覆盖不一样的
                num[s[left]-'A']--;
                left++;
            }
            right++;
        }
        return right-left;
    }
};
```





## 2021-2-3

### [相同的树](https://leetcode-cn.com/problems/same-tree/)



给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。



#### 思路

遍历判断即可



#### 题解

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==nullptr&&q==nullptr) return true;
        else if(p==nullptr) return false;
        else if(q==nullptr) return false;
        else{
            //考虑到熔断性 我们要先判断当前值在进行下面的遍历
            return (p->val==q->val)&&isSameTree(p->left,q->left)&&isSameTree(p->right,q->right);
        }
    }
};
```





## 2021-2-6

### [可获得的最大点数](https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards/)



几张卡牌 排成一行，每张卡牌都有一个对应的点数。点数由整数数组 cardPoints 给出。

每次行动，你可以从行的开头或者末尾拿一张卡牌，最终你必须正好拿 k 张卡牌。

你的点数就是你拿到手中的所有卡牌的点数之和。

给你一个整数数组 cardPoints 和整数 k，请你返回可以获得的最大点数。



#### 思路

滑动窗口，维护中间length-k的最小值



#### 题解

```c
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        //转换思路
        //里面的length-k个最小 外面就是最大
        //滑动窗口
        int left=0,right=0;
        int max=0;//统计最小值
        int sum=0;
        int all=0;
        int size=cardPoints.size();
        for(;right<size-k;++right){
            sum+=cardPoints[right];
        }
        max=sum;
        all=sum;
        while(right<size){
            all+=cardPoints[right];
            sum+=cardPoints[right];
            sum-=cardPoints[left];
            right++;
            left++;
            if(sum<max) max=sum;
        }
        return all-max;
    }
};
```





### [删除排序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)



给定一个增序排列数组 nums ，你需要在 原地 删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。



#### 思路

1、原来没看到排序数组，直接用map模拟了

2、增序的话 看前面一个和自己是否一样



#### 题解1

```c
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        map<int,int> mp;//记录次数
        int left=0;
        int i=0;
        int size=nums.size();
        map<int,int>::iterator it;
        while(i<size){
            it=mp.find(nums[i]);
            if(it!=mp.end()){
                //能找到
                it->second++;
                if(it->second<=2){
                    nums[left]=nums[i];//符合要求 移进去
                    left++;
                }
            }
            else{
                //不能找到
                mp.insert(pair(nums[i],1));//插入次数为1
                nums[left]=nums[i];//符合要求 移进去
                left++;
            }
            i++;
        }
        return left;
    }
};
```





#### 题解2

```c
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int size = nums.size();
        int count=1;
        int left=1;
        int right=1;//总是从第二个元素开始
        for(;right<size;++right){
            if(nums[right]==nums[right-1]){
                //一样
                count++;
                if(count==2){
                    //没问题
                    nums[left]=nums[right];
                    left++;
                }
            }
            else{
                //不一样
                nums[left]=nums[right];
                left++;
                count=1;
            }
        }
        return left;
    }
};
```



## 2021-2-13

### [找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。



#### 思路

如果一个数出现过，我们就标记一下该数对应的位置其出现过，所以只需要考虑怎么标记的问题。如果可以使用额外空间就会很简单，既然不能使用额外空间的话，我们考虑给对应位置的数+n，这样只要这个位置的数超过n，那么该位置对应的数就出现过。



#### 题解

```c
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int size=nums.size();
        for(int i=0;i<size;++i){
                //相应位置+n 表示出现过
            if(nums[(nums[i]-1)%size]<=size)
                nums[(nums[i]-1)%size]+=size;
        }
        vector<int> result;
        for(int i=0;i<size;++i){
            if(nums[i]<=size){
                result.push_back(i+1);
            }
        }
        return result;
    }
};
```



### [无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。



#### 思路

双指针遍历，通过set判断是否有重复，如果无重复则加入set，并且移动右指针，如果重复则一直移动左指针，知道左右相等。



#### 题解

**可以将find改成count方法，但是该题解运行速度仅超过75，空间超过45，可优化。**

```c
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        //滑动窗口 每次滑完判断下
        int size=s.length();
        int left=0;
        int right=0;
        int max=0;
        int length=0;
        set <char> se;
        while(right<size){
            if(se.find(s[right])==se.end()){
                //未找到
                se.insert(s[right]);
                length++;
                if(length>max) max=length;
            }
            else{
                //找到了一样的
                //先移到相同的地方
                while(s[left]!=s[right]){
                    se.erase(s[left]);
                    left++;
                    length--;
                }
                //到达相同 往下一个
                left++;
            }
            right++;
    }
    return max;
    }

};
```



### [盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。



#### 思路

和找一个最大的长方形面积是一样的，如果要找一种不是暴力的解法，我们就要考虑一种合适的调整策略，合适的调整长方形的两边的位置。

我们考虑从最外面往内收缩，当然我们也可以从内向外，但是找内部的中间比从外向内难，当向内收缩的时候，考虑到x轴的变化是一样的，所以如果我们缩小的是高的那边，那么新的高一定是小于等于低的那边，如果缩小低的那边，则是小于等于个高的那边，所以我们考虑缩小低的那边（类似贪心），直到两者相遇就结束。



#### 题解

```c
class Solution {
public:
    int maxArea(vector<int>& height) {
        int i=0,j=height.size()-1;
        int max=0;
        int num;
        while(i<j){
            //从两边往中间
            int min=(height[i]>height[j])?height[j]:height[i];
            num=min*(j-i);
            if(num>max) max=num;
            if(min==height[i]){
                i++;
            }
            else
                j--;
        }
        return max;
    }
};
```





## 2021-2-14

### [二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

#### 思路

直接遍历找深度就可以。



#### 题解一

```c
class Solution {
public:
    int maxDepth(TreeNode* root) {
        //递归的dfs
        if(root==nullptr) return 0;
        int left=maxDepth(root->left)+1;
        int right=maxDepth(root->right)+1;
        return max(left,right);
    }
};
```



#### 题解2

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
// 层次遍历
class Solution {
public:
    int maxDepth(TreeNode* root) {
       if(root==nullptr) return 0;
       //normal
       int p=0;
       int q=0;
       int length=0;
       int flag=0;
       vector<TreeNode*> queue;
       queue.push_back(root);
       while(p<=q){
           if(queue[p]->left!=nullptr){
               queue.push_back(queue[p]->left);
               q++;
           }
           if(queue[p]->right!=nullptr){
               queue.push_back(queue[p]->right);
               q++;
           }
           if(p==flag){
               //到达一层的结束 记录新的结束并且增加层数
               flag=q;
               length++;
           }
           p++;
       }
       return length;
    }
};
```



### [生成交替二进制字符串的最少操作数](https://leetcode-cn.com/problems/minimum-changes-to-make-alternating-binary-string/)

给你一个仅由字符 '0' 和 '1' 组成的字符串 s 。一步操作中，你可以将任一 '0' 变成 '1' ，或者将 '1' 变成 '0' 。

交替字符串 定义为：如果字符串中不存在相邻两个字符相等的情况，那么该字符串就是交替字符串。例如，字符串 "010" 是交替字符串，而字符串 "0100" 不是。

返回使 s 变成 交替字符串 所需的 最少 操作数。



#### 思路

要么是01串要么是10串，所以只要统计变成10简单还是变成01简单，也即我们只需要遍历的时候统计下满足10的有多少个，满足01的有多少个，然后用总长度减掉即可。



#### 题解

```c
class Solution {
public:
    int minOperations(string s) {
        //要么改变要么不动
        //所以有可能是10串 也可能是01串
        //找出换成10和01的情况 然后比较即可
        int size=s.length();
        int a01=0; //01串符合个数
        int a10=0; //10串符合个数
        for(int i=0;i<size;++i){
            if(i%2==0){
                //偶数情况
                if(s[i]=='0'){
                    //01串
                    a01++;
                }
                else{
                    a10++;
                }
            }
            else{
                //奇数情况
                if(s[i]=='1'){
                    //01
                    a01++;
                }
                else
                    a10++;
            }
        }
        //由于统计的是符合要求的 所以我们直接用大的
        return (a01>a10)?size-a01:size-a10;
    }
};
```





## 2021-2-15

### [最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)



给定一个二进制数组， 计算其中最大连续1的个数。



#### 思路

如果遇到1就计数，遇到0就清零。



#### 题解

```c
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int size=nums.size();
        int length=0;
        int max=0;
        for(int i=0;i<size;++i){
            if(nums[i]==1){
                length++;
                if(length>max) max=length;
            }
            else{
                length=0;
            }
        }
        return max;
    }
};
```





## 2021-2-16

###  [数组拆分 I](https://leetcode-cn.com/problems/array-partition-i/)

给定长度为 2n 的整数数组 nums ，你的任务是将这些数分成 n 对, 例如 (a1, b1), (a2, b2), ..., (an, bn) ，使得从 1 到 n 的 min(ai, bi) 总和最大。

返回该 最大总和 。



#### 思路

先通过数学推导，考虑什么时候会是最大的情况



#### 题解

```c
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        //数学推导一下
        //如果大的数确定 那么小数就是最贴近他那个
        //例如abcd递增 那么ab cd的组合会得到 ac
        //而ac bd的组合会得到ab 
        //所以我们排序一下然后进行两两划分
        int sum=0;
        int size=nums.size();
        sort(nums.begin(),nums.end());
        for(int i=0;i<size;i=i+2){
            sum+=nums[i];
        }
        return sum;
    }
};
```



## 2021-2-18

### [只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？



#### 思路

（查看题解后）使用异或，如果出现两次，异或后就消失了，所以全部异或剩下的就是答案。



#### 题解

```c
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result=0;
        int size=nums.size();
        for(int i=0;i<size;++i){
            result=result^nums[i];
        }
        return result;
    }
};
```



### [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。



#### 思路

就像在操场上跑步一样，跑得快的那个总会超过跑得慢的一圈以上，他们必定会在某处相遇，所以我们可以在这里设置快慢指针，如果他们相遇了就说明有环。



#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast=head;
        ListNode *slow=head;
        while(fast!=nullptr){
            if(fast->next&&fast->next->next){
                fast=fast->next->next;
            }
            else return false;
            slow=slow->next;
            if(slow==fast) return true;
        }
        return false;
    }
};
```



## 2021-2-19



### [1004. 最大连续1的个数 III](https://leetcode-cn.com/problems/max-consecutive-ones-iii/)

给定一个由若干 `0` 和 `1` 组成的数组 `A`，我们最多可以将 `K` 个值从 0 变成 1 。

返回仅包含 1 的最长（连续）子数组的长度。



#### 思路

滑动窗口，每次向右滑动，然后判断当前窗口内是否符合改变K个数后都是1的要求，如果可以的话就更新`length`以及`max`，不行的话就更新把左边向右移动。



#### 题解

```c
class Solution {
public:
    int longestOnes(vector<int>& A, int K) {
        int size=A.size();
        int left=0;
        int right=0;
        int max=0;
        int length=0;
        while(right<size){
            //滑动窗口
            if(A[right]==0){
                length++;
            }
            //判断是否符合要求 不符合滑动左边
            if(length>K){
                while(A[left]!=0) {left++;}
                left++;
                length--;
            }
            else{
                if(right-left+1>max) max=right-left+1;
            }
            right++;
        }
        return max;
    }
};
```



## 2021-2-20

### [697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

给定一个非空且只包含非负数的整数数组 nums，数组的度的定义是指数组里任一元素出现频数的最大值。

你的任务是在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。



#### 思路

通过map来记录每个数出现的次数、起始位置和最后出现位置，这样我们就可以只需要遍历一遍原数据然后再遍历一遍map即可。



#### 题解

```c
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        map <int,vector<int>> mp;
        map <int,vector<int>>::iterator it;
        int size=nums.size();
        for(int i=0;i<size;++i){
            it=mp.find(nums[i]);
            if(it==mp.end()){
                //新出现的数
                vector<int> q(3);
                q[0]=1;
                q[1]=i;
                q[2]=i;
                mp.insert(pair<int,vector<int>>(nums[i],q));
            }
            else{
                //更新出现情况
                it->second[0]++;
                it->second[2]=i;
            }
        }
        int max=0;
        int length=0;
        for(it=mp.begin();it!=mp.end();it++){
            if(it->second[0]>max){
                //出现了新的最大
                max=it->second[0];
                length=it->second[2]-it->second[1]+1;
            }
            else if(it->second[0]==max){
                //出现次数一样 比较子数组长短
                if(it->second[2]-it->second[1]+1<length){
                    length=it->second[2]-it->second[1]+1;
                }
            }
        }
        return length;
    }
};
```



### [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

编写一个程序，找到两个单链表相交的起始节点。



#### 思路

如果暴力寻找的话，会浪费大量的时间。考虑到如果相交的时候，这两个链表有一段是一样长的。假定链表分别是A和B，那么我们把链表组合成`A+B `和`B+A`的形式，同步去遍历他们，当第二次遍历到相同的部分的时候，他们一定是一样的。也即如果A可以看成`a+c`，B可以看成`b+c`，则`A+B `和`B+A`分别为`a+c+b+c`和`b+c+a+c`，当我们遍历到第二个`c`的时候，他们一定是同时到达的，这样就能找到相交节点了。



#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
       ListNode *q=headA,*p=headB;
       if(headA==nullptr||headB==nullptr) return 0;
       while(q!=p){
           p=(p==nullptr)?headA:p->next;
           q=(q==nullptr)?headB:q->next;
       }
       return q;
    }
};
```



## 2021-2-21

### [1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

给你一个整数数组 nums ，和一个表示限制的整数 limit，请你返回最长连续子数组的长度，该子数组中的任意两个元素之间的绝对差必须小于或者等于 limit 。

如果不存在满足条件的子数组，则返回 0 。



#### 思路

首先这是一个滑动窗口的模板，接着我们要考虑如何保证`任意两个元素之间的绝对差小于等于limit`，这句话换个角度来看就是说保证这个子数组中的最大最小值的差值小于等于limit，所以我们考虑用一种合适的数据结构来保存最大最小即可。



#### 题解

```c
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        multiset<int> st;
        int left = 0, right = 0;
        int size=nums.size();
        int answer = 0;
        while (right < size) {
            st.insert(nums[right]);
            while (*st.rbegin() - *st.begin() > limit) {
                st.erase(st.find(nums[left]));
                left ++;
            }
            answer = max(answer, right - left + 1);
            right ++;
        }
        return answer;
    }
};
```





### [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

注意：
0 ≤ x, y < 231.



#### 思路

首先，对两个数据进行异或操作，之后统计异或的结果有多少个1即可。

在统计1的个数的时候，我们可以选择朴素的二进制计算法，也可以选择**布赖恩·克尼根算法**



#### 题解

```c
class Solution {
public:
    int hammingDistance(int x, int y) {
        //先异或 然后看异或以后的结果的1的数目
        int c=x^y;
        int result=0;
        while(c!=0){
            result+=c%2;
            c=c/2;
        }
        return result;
    }
};
```



#### 布赖恩·克尼根算法

很朴素的一个思路，我们假定有一个数是`100100`，那么人只会去找1的个数，所以我们可以尝试让这个数减一，在减一以后，与原来的数进行与运算，这样的话，就会将最右边的1变成0，重复这样的操作，我们就可以找到1的个数。





### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。



#### 思路

只要能够对树进行遍历即可，遍历的时候保证两棵树一起遍历，即使是`null`也要遍历到然后合并就行。采用`DFS`比较简单。



#### 题解

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        //考虑有一部分残缺的情况 如果都残缺的话返回null也没问题
        if(root1==nullptr){
            return root2;
        }
        if(root2==nullptr){
            return root1;
        }
        //都不残缺的情况 就地更改root1即可 当然也可新建
        root1->val=root1->val+root2->val;
        root1->left=mergeTrees(root1->left,root2->left);
        root1->right=mergeTrees(root1->right,root2->right);
        return root1;
    }
};
```





### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

翻转一棵二叉树。



#### 思路

首先考虑一个问题，翻转子树优先和翻转当前节点是否冲突。画图后我们可以发现，二叉树翻转某个节点的两个子树和他自己并不影响，也即，每次翻转只会改变当前层级的两个节点的情况，所以我们直接翻转就可以。



#### 题解

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root==nullptr) return root;
        TreeNode *temp;
        temp=root->left;
        root->left=root->right;
        root->right=temp;
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```



### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

反转一个单链表。



#### 思路

假如链表为`a->b->c->d->null`，当我们遍历到`b`的时候，我们将`b->next`设置成`b`前面的，然后继续遍历，把c放到b前面，如此类推即可，值得注意的是，要记得把`a->next`变成`null`防止出现环。



#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(!head) return nullptr;
        ListNode * p;
        ListNode * q=head;
        ListNode * t=head;
        while(head!=nullptr){
            //记录当前节点
            p=head;
            //head跳到下一个节点
            head=head->next;
            //把反转链表接到当前节点上
            p->next=q;
            //反转链往前移一个
            q=p;
        }
        t->next=nullptr;
        return p;
    }
};
```





### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。



#### 思路

法一：分治法，可以证明如果一个数是众数，那么他在两个一半的数组中也是一个众数，我们可以将两个子数组的众数找出来在选择正确的那个

法二：摩尔投票法，一个经典的方法。我们需要维护一个`candidate`变量和一个`count`变量，前者记录众数的候选者，后者记录这个候选者的数目。当`count`等于0时，我们将`candidate`设置成当前的数组值。当遍历时，如果`candidate`等于当前值，则`count++`，反之则减一。最后遍历完剩下的就是众数。

很直观的一个解释就是，众数会超过整个数组的一半，所以当`candidate`是众数时，因为众数超过了一半，所以其不会减到0，即使减到了0，也会在后面遇到更多的该数使得`candidate`又变成这个数，而如果其不是众数，因为其会大量出现，所以它会让当前的`candidate`下台。



#### 题解（摩尔投票法）

```c
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count=0;
        int candidate;
        int size=nums.size();
        for(int i=0;i<size;++i){
            if(count==0) 
                candidate = nums[i];
            if(nums[i]==candidate)
                    count++;
                else    
                    count--;
            }
        return candidate;
        }
};
```



### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。



#### 思路

0移到末尾也就是把非0数放到前面，也即每次我们将非0数往前放，然后整个数组放完后对后面进行补0即可。所以我们需要双指针，一个遍历数组一个用来记录往左移动后非0数的最后一个的位置。



#### 题解

```c
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        //换个思路看题就是
        //将所有非0数移到前面即可然后将后面全部补0
        int size=nums.size();
        int q=0;//记录当前存储值的位置
        for(int i=0;i<size;++i){
            if(nums[i]!=0){
                nums[q]=nums[i];
                q++;
            }
        }
        for(;q<size;++q){
            nums[q]=0;
        }
    }
};
```



## 2021-2-23

### [1052. 爱生气的书店老板](https://leetcode-cn.com/problems/grumpy-bookstore-owner/)

今天，书店老板有一家店打算试营业 customers.length 分钟。每分钟都有一些顾客（customers[i]）会进入书店，所有这些顾客都会在那一分钟结束后离开。

在某些时候，书店老板会生气。 如果书店老板在第 i 分钟生气，那么 grumpy[i] = 1，否则 grumpy[i] = 0。 当书店老板生气时，那一分钟的顾客就会不满意，不生气则他们是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 X 分钟不生气，但却只能使用一次。

请你返回这一天营业下来，最多有多少客户能够感到满意的数量。



#### 思路

显然，这是滑动窗口的题。题目中的窗口大小为`X`，是固定不动的，我们要计算这个窗口中如果不生气的顾客满意的增量，而如果原来就不生气的顾客我们直接加上去即可。所以我们需要寻找一个顾客满意增量最大的定长窗口。



#### 题解

```c
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int X) {
        int size=customers.size();
        int left=0,right=0;
        int sum=0;
        int s=0;
        for(;right<X;++right){
            //总和
            if(grumpy[right]==0)
                sum+=customers[right];//总和
            //增量
            s+=grumpy[right]*customers[right];
        }
        int maxD=s;
        for(;right<size;++right){
            //计算总和
            if(grumpy[right]==0)
                sum+=customers[right];//总和
            //计算不生气后的增量
            s=s-grumpy[left]*customers[left]+grumpy[right]*customers[right];
            if(s>maxD) maxD=s;
            left++;
          
        }
        return maxD+sum;
    }
};
```



### [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

请判断一个链表是否为回文链表。



#### 思路

- 数组保存指针，然后遍历数组即可
- 快慢指针，可以找到这个链表的中间，然后翻转后半链表与前半部分比较。



#### 题解

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        //数组
        vector<ListNode*> num;
        while(head!=nullptr){
            num.push_back(head);
            head=head->next;
        }
        int size=num.size();
        for(int i=0;i<size/2;++i){
            if(num[i]->val!=num[size-i-1]->val)
                return false;
        }
        return true;
    }
};
```



### [78. 子集](https://leetcode-cn.com/problems/subsets/)

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。



#### 思路

这个数在数组里面与不在里面可以用01来表示，那么整个数组就是一个01串，也就是一个二进制数。这样我们进行遍历二进制数，如果某位是1，那么就把对应的数放进去即可。



#### 题解

```c
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int size=nums.size();
        vector<int> temp;
        vector<vector<int>> result;
        int limit=1<<size;
        for(int i=0;i<limit;++i){
            temp.clear();
            for(int t=0;t<size;++t){
                if(i&(1<<t)){
                    //是1
                    temp.push_back(nums[t]);
                }
            }
            result.push_back(temp);
        }
        return result;

    }
};
```



### [766. 托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)

给你一个 m x n 的矩阵 matrix 。如果这个矩阵是托普利茨矩阵，返回 true ；否则，返回 false 。

如果矩阵上每一条由左上到右下的对角线上的元素都相同，那么这个矩阵是 托普利茨矩阵 。



#### 思路

模拟，遍历数组，每次将他和左上角进行比较即可。



#### 题解

```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        int x = matrix.length;
        int y = matrix[0].length;
        for(int i=1;i<x;++i){
            for(int j=1;j<y;++j){
                if(matrix[i][j]!=matrix[i-1][j-1])
                    return false;
            }
        }
        return true;
    }
}
```



### [832. 翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

给定一个二进制矩阵 A，我们想先水平翻转图像，然后反转图像并返回结果。

水平翻转图片就是将图片的每一行都进行翻转，即逆序。例如，水平翻转 [1, 1, 0] 的结果是 [0, 1, 1]。

反转图片的意思是图片中的 0 全部被 1 替换， 1 全部被 0 替换。例如，反转 [0, 1, 1] 的结果是 [1, 0, 0]。



#### 思路

按照题目意思来做即可，注意可以把01置换和交换步骤放到一次循环里来做。



#### 题解

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int x=A.length;
        int y=A[0].length;
        for(int i=0;i<x;i++){
            for(int j=0;j<(y+1)/2;++j){
                //01 change
                A[i][j]^=1;
                if(y-1!=2*j)
                    A[i][y-1-j]^=1;
                //swap
                int temp=A[i][y-1-j];
                A[i][y-1-j]=A[i][j];
                A[i][j]=temp;
            }
        }
        return A;
    }
}
```



## 2021-2-25

### [867. 转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

给你一个二维整数数组 `matrix`， 返回 `matrix` 的 **转置矩阵** 。

矩阵的 **转置** 是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。



#### 思路

模拟即可。



#### 题解

```java
class Solution {
    public int[][] transpose(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;
        int[][] num = new int [m][n];
        for(int i=0;i<n;++i){
            for(int j=0;j<m;++j){
                num[j][i]=matrix[i][j];
            }
        } 
        return num;
    }
}
```



### [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

给定一个非负整数 **num**。对于 **0 ≤ i ≤ num** 范围中的每个数字 **i** ，计算其二进制数中的 1 的数目并将它们作为数组返回。



#### 思路

考虑到前面的布赖恩·克尼根算法，我们可以将其作为子函数，然后每次去调用他即可。

但是这样我们每次都在重新算一遍，而作为二进制来说，相邻的数应该是有某种联系的，所以可以考虑一些dp解法。（见题解2）



#### 题解1

```java
class Solution {
    public int[] countBits(int num) {
        int[] result = new int[num+1];
        for(int i=0;i<=num;++i){
            result[i]=get1(i);
        }
        return result;
    }

    int get1(int x){
        int time=0;
        while(x!=0){
            x=x&(x-1);
            time++;
        }
        return time;
    }
}
```



#### 题解2 基于1的dp

```java
class Solution {
    public int[] countBits(int num) {
        //我们知道x&(x-1)比x少了一个1 所以基于这个特性我们就可以知道递推式
        //f(x)=f(x&(x-1))+1 ,x>0
        int[] result = new int[num+1];
        result[0]=0;//init
        for(int i=1;i<=num;++i){
            result[i]=result[i&(i-1)]+1;
        }
        return result;
    }

}
```



## 2021-2-28

### [896. 单调数列](https://leetcode-cn.com/problems/monotonic-array/)

如果数组是单调递增或单调递减的，那么它是单调的。

如果对于所有 i <= j，A[i] <= A[j]，那么数组 A 是单调递增的。 如果对于所有 i <= j，A[i]> = A[j]，那么数组 A 是单调递减的。

当给定的数组 A 是单调数组时返回 true，否则返回 false。



#### 思路

用一个`flag`记录前面的差值，如果两个差值异号则不单调，同号则更新flag，如果是0则保持前面的`flag`。



#### 题解

```java
class Solution {
    public boolean isMonotonic(int[] A) {
        int size = A.length;
        if(size<=2) return true;
        int flag=0;
        for(int i=1;i<size;++i){
            int d=A[i]-A[i-1];
            if(flag*d<0) return false;
            flag = (d==0)?flag:d;
        }
        return true;
    }
}
```



## 2021-3-1

### [303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

给定一个整数数组  nums，求出数组从索引 i 到 j（i ≤ j）范围内元素的总和，包含 i、j 两点。

实现 NumArray 类：

NumArray(int[] nums) 使用数组 nums 初始化对象
int sumRange(int i, int j) 返回数组 nums 从索引 i 到 j（i ≤ j）范围内元素的总和，包含 i、j 两点（也就是 sum(nums[i], nums[i + 1], ... , nums[j])）



#### 思路

前缀和。我们每次不记录数据是多少，而是记录前n个数的总和是多少，这样每个数据可以通过n与n-1的差值计算，而计算题目中的和的时候，只需要将两个前缀和相减就能得到某一串数据的和。



#### 题解

```java
class NumArray {
    int [] num;

    public NumArray(int[] nums) {
        int size = nums.length;
        num = new int[size+1];
        num[0]=0;
        for(int i=0;i<size;++i){
            num[i+1]=num[i]+nums[i];
        }
    }
    
    public int sumRange(int i, int j) {
        return num[j+1]-num[i];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```



## 2021-3-2

### [304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2) 。



#### 思路

考虑前缀和。在此题中，要求的是一个矩阵，所以我们会考虑在这个矩阵中，如何使用前缀和，即每个空格的数字代表着什么。考虑到容斥原理，我们可以将`num[i][j]`表示为i\*j大小的一个矩阵。那么我们如何表示要求的矩阵的面积呢？根据容斥原理，我们可以得到

```java
num[i][j]=num[i-1][j]+num[i][j-1]-num[i-1][j-1]+matrcx[i][j]
num[i-a][j-b]=num[i][j]-num[i-a-1][j]-num[i][j-b-1]+num[i-a-1][numj-b-1]
```

这样我们就可以考虑用代码实现了。在实现的过程中，我们发现，如果保持原来的数组大小的话，对于`i==0`和`j==0`这样的边界条件我们很难处理，所以我们可以考虑将数组设置成\[row+1][rol+1]的大小，这样就不用考虑其边界，只要做好对应运算即可。



#### 题解

```java
class NumMatrix {
    int [][] num;

    public NumMatrix(int[][] matrix) {
        int x = matrix.length;
        if(x==0) return  
        int y = matrix[0].length;
        num = new int [x+1][y+1];
        for(int i=1;i<=x;++i){
            for(int j=1;j<=y;++j){
                //容斥原理
                num[i][j] = num[i][j-1]+num[i-1][j]-num[i-1][j-1]+matrix[i-1][j-1];
                }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return num[row2+1][col2+1]-num[row2+1][col1]-num[row1][col2+1]+num[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```



## 2021-3-6

### [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

给你两个 没有重复元素 的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。

请你找出 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 -1 。



#### 思路

单调栈。我们维护一个单调栈来找比这个数大的下一个数，可以直接把整个数组的所有的都算出来，然后按照nums1填到答案数组里去即可。



#### 题解

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Stack<Integer> stack = new Stack <>();
        HashMap < Integer, Integer > map = new HashMap < > ();
        int[] ans = new int [nums1.length];
         for (int i = 0; i < nums2.length; i++) {
            while (!stack.empty() && nums2[i] > stack.peek())
                map.put(stack.pop(), nums2[i]);
            stack.push(nums2[i]);
        }
        while (!stack.empty())
            map.put(stack.pop(), -1);
        for (int i = 0; i < nums1.length; i++) {
            ans[i] = map.get(nums1[i]);
        }
        return ans;

    }
}
```



### [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。



#### 思路

单调栈。该题是一个循环数组，所以我们可以考虑把这个数组变成两倍长，这样的话，在数组末的元素也可以和数组头的元素进行比较。



#### 题解

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        Stack<Integer> stack = new Stack <>();
        int[] ans = new int [nums.length];
        for (int i = 0; i < nums.length; i++) {
                while (!stack.empty() && nums[i] > nums[stack.peek()])
                    ans[stack.pop()] = nums[i];
                stack.push(i);
            }
        //处理尾部元素和头部元素的比较
        for (int i = 0; i < nums.length; i++) {
                while (!stack.empty() && nums[i] > nums[stack.peek()])
                    ans[stack.pop()] = nums[i];
            }
        while (!stack.empty())
            ans[stack.pop()] = -1;
        return ans;
    }
}
```



## 2021-3-7

### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。



#### 思路

DP。假设dp数组为A\[i][j]表示字符串从i到j的子串是否是回文串的话，那么A\[i][j]是的前提是A\[i+1][j-1]是回文串且i和j处的字符相等，所以我们可以有解法一求解。





#### 题解一

```java
class Solution {
    public int countSubstrings(String s) {
        int size = s.length();
        boolean [][] f = new boolean[size][size];
        //置初值
        for(int i =0;i<size;++i){
            Arrays.fill(f[i],true);
        }
        //dp
        for(int i=size -1;i>=0;i--){
            for(int j=i+1;j<size;++j){
                f[i][j] = f[i+1][j-1]&&(s.charAt(i)==s.charAt(j));
            }
        }
        //遍历dp数组 计算个数
        int result = 0 ;
        for(int i=0;i<size;++i){
            for(int j=i;j<size;++j){
                if(f[i][j])
                    result++;
            }
        }
        return result;
    }
}
```







### [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。



#### 思路

dp+dfs回溯法。首先我们可以想到要用搜索，因为这相当于是在一个长度为n的数组中，找出所有可能的排列组合；但是我们不能够在每次搜索的时候才判断是不是回文串，所以我们可以考虑先用dp对数据做一个预处理，这样在搜索的时候直接用即可。dp预处理字符串可以参见上一题。



#### 题解

```java
class Solution {
     boolean [][] f;
    List <String> ans = new ArrayList<String> ();
    List <List<String>> result = new ArrayList<List<String>>();
    int size;

    public List<List<String>> partition(String s) {
        //dp+回溯法
        //首先通过dp算出回文串的情况
        //H(i-1,j+1) = H(i,j)&&num[i-1]==num[j+1]
        //接着通过dfs进行搜索 然后回溯
        size = s.length();
        f = new boolean[size][size];//dp存储
        for(int i = 0;i < size ;++i){
            Arrays.fill(f[i],true);//置初值为true
        }
        
        //dp预处理
        for(int i= size - 1; i>=0;--i){
            for(int j=i+1;j<size;++j){
                f[i][j] = (f[i+1][j-1])&&(s.charAt(i)==s.charAt(j));
            }
        }
        //现在已经有了一个f数组 f[i][j]表示字符串i到j的子串是否是回文串
        //这样我们进行搜索，每次遇到一个回文串就加进来
        //然后把右边界往右边放 继续递归搜索
        //当我们搜索到最后的时候 就找到了一种回文法 然后加入 回溯
        //回溯的时候要注意把本次加入的内容删除掉
        //这样我们就能找到所有的解
        dfs(s,0); 
        return result;
    }

    void dfs(String s, int n){
        //已经到了搜索的尽头
        if(n==size){
            result.add(new ArrayList(ans));
            return ; 
        }
        //还没到尽头
        for(int j = n;j<size;++j){
            if(f[n][j]){
                //从当前到j是回文串
                //那么我们加入该串到当前解法的ans中
                ans.add(s.substring(n,j+1));
                //继续dfs 但是要从j+1开始
                dfs(s,j+1);
                //这时候已经搜索完了，我们就要开始回溯
                //删除我们在这次调用中加入到ans的解
                //值得注意的是，在递归里加入的内容已经通过递归被删除了
                //所以每次照顾好自己就行了
                ans.remove(ans.size()-1);
            }
        }
    }
}
```



## 2021-3-8

### [132. 分割回文串 II](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是回文。

返回符合要求的 **最少分割次数** 。



#### 思路

首先根据昨天的题目我们可以知道如何求出当前字符串中有多少个回文串。那么问题就变成了在这个字符串的多种拆分中哪一种可以有更少的回文串数量。

首先考虑DFS，但是超时。

接着考虑DP。我们可以得到一个这样的递推关系：j位置的最少拆分等于j前面所有能和j组成回文串的前一个的最少字符串数+1。例如j位置处的最小拆分，我们看j前面的，如果i处可以到j形成回文串，那么j-1的最小拆分加上1（即i到j）就是j处的一种拆分法，然后我们寻找所有拆分法里最小的那个，递推即可。




#### 题解

```java
class Solution {
    boolean [][] f;
    int size;
    public int minCut(String s) {
        size = s.length();
        f = new boolean[size][size];
        for(int i=0;i<size;++i){
            Arrays.fill(f[i],true);
        }

        ///dp
        for(int i=size-1;i>=0;i--){
            for(int j=i+1;j<size;++j){
                f[i][j] = f[i+1][j-1]&&(s.charAt(i)==s.charAt(j));
            }
        }

        //接下来找到一种切割方式 能将字符串分割成最少的几段
        //dfs搜索太慢 考虑dp
        //假定dp[i]表示到字符串i位置前面的最少次数
        //则dp[i] = i前面j位置能形成ji回文且最小的情况
        //有result个回文串 需要result-1次切割
        int [] dp = new int [size];
        Arrays.fill(dp,1);
        for(int i=1;i<size;++i){
            int min=dp[i-1]+1;//显然最差情况就是i-1加上自己独自回文
            for(int j=0;j<i;++j){
                if(f[j][i]){
                    //j可以到i
                    if(j==0) 
                        min=1;
                    else if(min>dp[j-1]+1)
                        min=dp[j-1]+1;//相当于j-1后面的连一起了
                }
            }
            dp[i]=min;
        }
        return dp[size-1]-1;
    }

}
```



## 2021-3-9

### [1047. 删除字符串中的所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/)

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。



#### 思路

利用栈进行匹配抵消即可。注意使用`StringBuffer`类



#### 题解

```java
class Solution {
    public String removeDuplicates(String S) {
        int size = S.length();
        StringBuffer stack = new StringBuffer();
        int top=-1;
        for(int i=0;i<size;++i){
            char x = S.charAt(i);//取字符
            if(top>=0 && stack.charAt(top)==x){
                //相等
                stack.deleteCharAt(top);
                top--;
            }
            else{
                stack.append(x);
                top++;
            }
        }
        return stack.toString();
    }
}
```



## 2021-3-10

### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。



#### 思路

DFS搜索。在搜索的过程中，我们需要考虑如何标记某个数字是否已经加入了这个结果中。我们可以考虑使用标记数组，为了减少空间使用，我们可以考虑使用哨兵的方式。

简单的说，例如现在有一个序列`[10,8,9,1,2]`，如果在递归中，我们已经有两个数加入到了搜索中，那我们就把这两个数放到左边，通过数组的长度来确定这个哨兵的位置。当我们加入第三个数的时候，我们可以把这个数放到第三个位置，搜索完回溯的时候把这个数换回来即可。



#### 题解

```java
class Solution {
    List <List<Integer>> result;
    List <Integer> ans;
    int size;

    public List<List<Integer>> permute(int[] nums) {
        //dfs搜索
        result = new ArrayList<List<Integer>> ();
        ans = new ArrayList<Integer>();
        for (int num : nums) {
            //初始化
            ans.add(num);
        }
        size = nums.length;
        //
        dfs(ans,0);
        return result;
    }
    //ans输入数组 
    //length表示当前已经有多少个加入了全排列
    void dfs(List<Integer> ans,int length){
        if(length == size) {
            //已经满了 加入到解中
            result.add(new ArrayList<Integer>(ans));
        }
        else{
            //没有满 进行遍历
            for(int i=length;i<size;++i){
                //先把没加入的数加入
                Collections.swap(ans, length, i);
                //进行dfs
                dfs(ans,length+1);
                //调换回顺序
                Collections.swap(ans,length,i);
            }
        }
    }
}
```



### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。



#### 思路

常规思路：生成然后判断是不是。

在这里我们考虑DP的做法。我们考虑这样的问题，当一对括号插入时，左括号肯定在右括号左边，基于这个思路，我们可以认为对于前一个的结果，我们可以把两个括号对上一个的结果进行逐个插入。例如`()`我们可以先把左括号放最左边，然后从右边选个位置放入右括号。但是这样我们会遇到一个问题，如`()`，当我们把右括号放在最右边，我们的左括号放在最开始还是第一个左括号后生成的串是一样的，也即会出现重复的情况。当然我们也可以考虑用map来去重。但是我们不妨这样考虑，既然会有重复，那么我们限定好左括号的位置是不是就可以了呢？这样我们就只需要考虑后面的情况即可。那右括号放在哪呢？我们可以这样考虑，假设前一个有n-1个括号，那么我们拆成两部分，左部分在dp数组中显然是没有重复的，而右部分也没有重复，把右括号放在这两者之间，我们就可以认为其是没有重复的。基于这样的思想，我们得到递推式：dp[n]="(" + dp[i] + ")" + dp[n-1-i]。接下来注意好边界条件即可。



#### 题解

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<List<String>> dp = new ArrayList<List<String>>();//dp数组
        //初始化 0和1的情况
        List <String> result0 = new ArrayList<String> ();
        result0.add("");
        dp.add(result0);
        List <String> result1 = new ArrayList<String> (); //存储结果
        result1.add("()");
        dp.add(result1);
        //考虑01特例
        if(n<2) return dp.get(n);
        for(int i=2;i<=n;++i){
            //n次dp
            //首先考虑到左括号在右边
            //其次考虑到如果左括号插入在里面 那么等价于插入在最左边
            //因为我们总可以把最左边那个和这个等价替换
            //这样新的就是 (+q+)+p的组合 q和p的长度加起来是上一个长度
            List<String> ans = new ArrayList<String>();
            for(int j=0;j<i;++j){
                //对temp进行dp
                List <String> left = dp.get(j);
                List <String> right = dp.get(i-1-j);
                for (String s1 : left) {
					for (String s2 : right) {
						String el = "(" + s1 + ")" + s2;
						ans.add(el);
					}
				}
            }
            dp.add(ans);
        }
        return dp.get(n);
    }
}
```



### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。



#### 思路

用栈实现中序遍历。注意把握什么时候访问元素。



#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List <Integer> result = new ArrayList<Integer>();
        if(root == null){
            return result;
        }
        Stack <TreeNode> stack = new  Stack<TreeNode>();
        //利用栈进行遍历
        //左中右
        while(root!=null||stack.size()!=0){
            //左
            while(root!=null){
                stack.push(root);
                root=root.left;
            }
            //中
            root = stack.peek();
            stack.pop();
            //只在出栈的时候访问元素
            result.add(root.val);
            //右
            root = root.right;
        }
        return result;
    }
}
```



### [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。



#### 思路

两种思路，从高到低考虑和从低到高考虑。

从高到低考虑：由于我们从高到低的进行插入，那么当我们插入到第i个时候，前面i-1个都会对i产生影响，那么这样我们就只要放到person\[i][1]个人之后就可以。

从低到高考虑：由于我们从低到高进行插入，那么当插入到第i个的时候，前面的插入对其一点影响都没有，因为全都比他矮或者相等，所以我们只要选择第person\[i][1]个空位把他放进去即可。

值得注意的是，我们需要考虑一下当身高一样时候的顺序问题。从高到低的时候，肯定是先排身高一样前面比他高的少的那个，这样才能实现他的数据比较少。反过来的时候则需要先排高的那个，这样其前面才能留出足够的空位。



#### 题解：从低到高

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people,new Comparator<int[]>(){
            public int compare(int[] person1, int[] person2) {
                //两个数组
                if (person1[0] != person2[0]) {
                    //如果身高不一样 那就按照身高排
                    return person1[0] - person2[0];
                } else {
                    //如果身高一样那就把位置靠后的往后放
                    return person2[1] - person1[1];
                }
            }
        });
        //这时候我们得到了升序的一个数列 同身高的时候降序
        //[[4,4],[5,2],[5,0],[6,1],[7,1],[7,0]]
        int size = people.length;
        int [][] result = new int [size][];//构建返回数组
        for(int [] person:people){
            //遍历数组重建队列
            //每个人前面都有person[2]个比它高的人
            //换个角度说 比他矮的人其实不影响他 
            //我们只需要从低到高进行插入 每次选择第person[2]个空位置即可
            int spaces = person[1] + 1;
            for (int i = 0; i < size; ++i) {
                if (result[i] == null) {
                    --spaces;
                    if (spaces == 0) {
                        result[i] = person;
                        break;
                    }
                }
            }
        }
        return result;
    }

}
```



#### 题解：从高到低

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people,new Comparator<int[]>(){
            public int compare(int[] person1, int[] person2) {
                //两个数组
                if (person1[0] != person2[0]) {
                    //如果身高不一样 那就按照身高排
                    return person2[0] - person1[0];
                } else {
                    //如果身高一样那就把位置靠后的往后放
                    return person1[1] - person2[1];
                }
            }
        });
        //这时候我们得到了降序的一个数列 同身高的时候升序
        int size = people.length;
        List<int []> ans = new ArrayList<int []>();
        for(int [] person:people){
            //遍历数组重建队列
            //因为先插入的都比其高 所以只要在第person[1]位置插入即可
            ans.add(person[1],person);
        }
        return ans.toArray(new int[ans.size()][]);
    }

}
```



### [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。



#### 思路

考虑一个矩阵，我们会发现，旋转这个矩阵等于一个四个数字的循环，我们需要把这个数字的循环往前循环一次。例如原来为`1,2,3,4`，那么我们可以将其旋转为`2,3,4,1`。那么这样我们就需要解决两个问题：

- 如何找到这四个数字
- 如何合理的进行遍历

针对第一个问题，我们可以尝试画一个数组，写出他们的数字表达式，来进行尝试。而面对第二个问题，则需要我们合理的选择循环区域。简单的来说，对于一组四个数字，我们只要能遍历其中的一个数字即可，相当于我们要找到整个矩阵的四分之一。考虑一个偶数矩阵，这显然是容易找到的，而对于一个奇数矩阵，则需要我们尝试一些其他的切割方法，如下所示：

![image-20210310215528882](https://i.loli.net/2021/03/10/46KZVuiElGozqbP.png)

这样，我们就解决了这个问题。



#### 题解

```java
class Solution {
    public void rotate(int[][] matrix) {
        //等于就是全部按照顺指针旋转
        //考虑n*n 
        //取组数据
        //n-1-b,a->a,b->b,n-a-1->n-a-1,n-b-1 然后顺时针换位
        //2,0->0,1->1,3->3,2
        //考虑如何遍历全部
        //如果是偶数矩阵 那么直接四等分 遍历一个小块就可以
        //如果是奇数矩阵 则需要采取一种新的分割方法来遍历
        int size = matrix.length;
        int temp;//临时变量
        for(int i=0;i<size/2;++i){
            for(int j=0;j<(size+1)/2;++j){
                temp = matrix[i][j];
                matrix[i][j] = matrix[size-1-j][i];
                matrix[size-1-j][i] = matrix[size-1-i][size-1-j];
                matrix[size-1-i][size-1-j]=matrix[j][size-1-i];
                matrix[j][size-1-i]=temp;
            }
        }
    }
}
```





### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。



#### 思路

首先，我们可以对`candidates`进行排序，将比`target`大的数据全部丢弃不管。

接下来我们很容易想到这是一个搜索题，我们采取`DFS`。因为数据可以被无限次重复选用，所以我们先采取以下方法降低一些复杂度：

- 每次从当前选择数据的最大数开始选，也即假如我们已经选择了`1,2,3`，那么接下来加入的数一定比3大。（`1,2,3,1`和`1,1,2,3`是一样的）
- 大于target的函数直接返回。

这就是题解一。

但是发现提交后，速度不尽如人意，重新考虑剪枝的问题。如上所言，我们是在递归进入函数时发现`sum>target`时返回，那么不难想像，该函数的上一层函数的下面的递归，都是会大于target的。举例来说，`target`=5，当前`sum`=3，现在剩下可以选择的数有`3,4,5`，当我们选择3加入进入递归后，我们会结束3然后进入4，如此下去。但是因为我们的数据已经排了序了，所以后面的情况肯定都不会成功。所以我们可以在进入递归前就结束掉这样的情况。



#### 题解一

```java
class Solution {
    List<List<Integer>> result;
    int size ;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);//先排序
        int number = candidates.length;
        size = number;
        for(int i=0;i<number;++i){
            if(candidates[i]>target)
                size = i;
        }
        result = new ArrayList<List<Integer>>();
        List<Integer> ans = new ArrayList<Integer>();
        //dfs
        dfs(candidates,target,ans,0,0);
        return result;
    }
    //数据表 目标值 解答列表 列表和 当前选择的元素序号
    void dfs(int[] candidates,int target,List<Integer> ans,int sum,int limit){
        //找到了一种解法
        if(sum>target) return;
        else if(sum==target){
            result.add(new ArrayList<Integer>(ans));
            return ;
        }
        for(int i=limit;i<size;++i){
            ans.add(candidates[i]);
            dfs(candidates,target,ans,sum+candidates[i],i);
            ans.remove(ans.size()-1);
        }
    }
}
```



#### 题解二

```java
class Solution {
    List<List<Integer>> result;
    int size ;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);//先排序
        int number = candidates.length;
        size = number;
        for(int i=0;i<number;++i){
            if(candidates[i]>target)
                size = i;
        }
        result = new ArrayList<List<Integer>>();
        List<Integer> ans = new ArrayList<Integer>();
        //dfs
        dfs(candidates,target,ans,0,0);
        return result;
    }
    //数据表 目标值 解答列表 列表和 当前选择的元素序号
    void dfs(int[] candidates,int target,List<Integer> ans,int sum,int limit){
        //找到了一种解法
        if(sum==target){
            result.add(new ArrayList<Integer>(ans));
            return ;
        }
        for(int i=limit;i<size;++i){
            if(sum+candidates[i]>target) return ;
            ans.add(candidates[i]);
            dfs(candidates,target,ans,sum+candidates[i],i);
            ans.remove(ans.size()-1);
        }
    }
}
```





### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

给定一个包含 n + 1 个整数的数组 nums ，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。

假设 nums 只有 一个重复的整数 ，找出 这个重复的数 。



#### 思路

因为数字的值总在数组范围内，所以我们做一个映射，每次遇到一个值的时候，把对应位置的数组的数值加size，这样出现两次的地方的值就会加两次size，我们找这个即可。



#### 题解

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int size = nums.length;
        for(int i=0;i<size;++i){
            nums[nums[i]%size]+=size;
        }
        int i;
        for(i=0;i<size;++i){
            if(nums[i]>2*size)
                break;
        }
        return i;
    }
}
```



## 2021-3-11

### [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

给你二叉树的根结点 root ，请你将它展开为一个单链表：

展开后的单链表应该同样使用 TreeNode ，其中 right 子指针指向链表中下一个结点，而左子指针始终为 null 。
展开后的单链表应该与二叉树 先序遍历 顺序相同。



#### 思路

前序遍历，然后把链表连接起来即可。



#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public void flatten(TreeNode root) {
        if(root == null) return;
        Stack <TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        TreeNode head = new TreeNode();
        TreeNode q = head;
        while(!stack.empty()){
            TreeNode p = stack.peek();
            stack.pop();
            if(p.right!=null){
                stack.push(p.right);
            }
            if(p.left!=null){
                stack.push(p.left);
            }
            p.left = null;
            head.right = p;
            head = head.right;
        }
    }
}
```



#### [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。



#### 思路

首先我们肯定需要拿一个数组来记录结果。接着我们考虑如何进行计算，显然，我们是可以进行暴力计算的。但是仔细思考我们会发现，a[i]的左边的乘积和a[i-1]的左边乘积是有关系的，他们只差了一个数字，我们可以这样理解，从左边乘过来，每次乘我们其实都会保存当前乘的数。例如数组为`1,2,3,4,5`，那么3的左侧*3本身就是得到4的左侧值。同样的道理，我们可以再开一个数组来记录右边的乘积情况，那么a[i]的自身意外的乘积就是左边的乘积乘右边的乘积。在考虑优化，我们会发现，我们每次都只会用到一个数据，例如a[n]的右边，我们只看n+1的情况，而不会用到n+2等，所以我们可以考虑用一个变量保存下这个乘积。这样我们就又进行了一层优化。



#### 题解

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int size = nums.length;
        int [] result = new int [size];
        result[0]=1; //这样就不需要初始化1了
        for(int i=1;i<size;++i){
            result[i] = result[i-1] * nums[i-1];
        }
        int R=1; //记录右边的乘积 
        //不记录的话 模仿左边会导致有些数重新乘了
        for(int i=size-2;i>=0;--i){
            R = R * nums[i+1];
            result[i] = result[i] * R;
        }
        return result;
    }
}
```





### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

根据一棵树的前序遍历与中序遍历构造二叉树。



#### 思路

前序遍历：中，左子串，右子串

中序遍历：左子串，中，右子串。

所以我们可以根据前序的第一个字符把中旬进行切割，然后用递归切割中序的左子串和前序的左子串，同理切割右子串即可。

注意好对应的区间开闭：假设串为`[0,n-1]`，中序的中间字符出现在`in`位置，那么前序序列被切割为`0`，`[1,in]`，`[in+1,n-1]`，中序切割为`[0,in]`，`[in]`，`[in+1,n-1]`。

在这里有一个可以优化的地方就是找出前序的第一个符号在中序中出现的位置，我采用的是遍历，但是其实可以使用`map`来存储。

#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int size;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        TreeNode root = new TreeNode();
        int in = 0;
        size = preorder.length;
        for(;in<size;++in){
            if(inorder[in] == preorder[0])
                break;
        }
        root.val = preorder[0];
        root.left = Tree(preorder,inorder,1,in,0,in-1);
        root.right = Tree(preorder,inorder,in+1,size-1,in+1,size-1); 
        return root;
    }

    TreeNode Tree(int [] preorder,int[ ] inorder,int preL,int preR,int inL,int inR){
        if(preR<preL||inR<inL) return null;
        TreeNode root = new TreeNode(preorder[preL],null,null);
        if(inR==inL){
            return root;
        }
        int in = 0;
        for(;in<=inR-inL;++in){//找中间节点
            if(inorder[inL+in] == preorder[preL])
                break;
        }
        root.left = Tree(preorder,inorder,preL+1,preL+in,inL,inL+in-1);
        root.right = Tree(preorder,inorder,preL+1+in,preR,inL+in+1,inR);
        return root;
    }
}
```



### [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？



#### 思路

显然，暴力去做是不合适的，我们可以考虑搜索树的特征：比根小的在左边，比根大的在右边。那我们思考一下，如果我们取k作为根，那么左边就有k-1个数，右边有n-k个数，而k-1和n-k的构成的搜索树的可能的数目也正好递归的成为了一个子问题，即：

- k-1为节点的二叉搜索树的数目
- n-k为节点的二叉搜索树的数目

这样就很明显的成为了一个dp问题，那么我们考虑n个数时的递推公式即可。当有n个数时，每个数都可以做root，所以假设dp[n]是n个数的数目，那么递推公式为：
$$
dp[n] = \sum_{i=0}^{n-1}dp[i]*dp[n-1-i]
$$
根据该递推公式，我们就可以简单的求出解了。



#### 题解

````java
class Solution {
    public int numTrees(int n) {
        //对于一个升序序列 选定一个数作为root以后
        //其左子树就是左边k个数的可能的情况
        //其右子树也是其右边k个数可能的情况
        int [] dp = new int [n+1];
        dp[1] = 1;
        dp[0] = 1;
        for(int i=2;i<=n;++i){
            //dp[n] = dp[i]*dp[n-i-1]  0<=i<n
            dp[i] = 0;
            for(int j=0;j<i;++j){
                dp[i] += dp[j]*dp[i-1-j];
            }
        }
        return dp[n];
    }
}
````





### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

给定一个包含非负整数的 `m x n` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。



#### 思路

经典DP问题。要么往下走要么往右走，所以如果我们要找最小的话，只要看左边和上面哪个小即可。

考虑优化，建立一个`(m+1)*(n*1)`的数组，设置为较大值，这样就能减少掉对边界的检查。但是在复杂度上差距不大。



#### 题解

```java
class Solution {
    public int minPathSum(int[][] grid) {
        //dp
        //显然只能往下或者往右
        //那么到x,y的最小值要么来自于上方 要么来自于左边
        int x = grid.length;
        int y = grid[0].length;
        int [][] dp = new int [x][y];
        dp[0][0] = grid[0][0];
        for(int i=0;i<x;++i){
            for(int j=0;j<y;++j){
                if(j!=0&&i!=0){
                    //正常
                    dp[i][j] = (dp[i-1][j]>dp[i][j-1])?dp[i][j-1]+grid[i][j]:dp[i-1][j]+grid[i][j];
                }
                else if(i==0&&j!=0){
                    //第一行
                    dp[i][j] = dp[i][j-1]+grid[i][j];
                }
                else if(i!=0&&j==0){
                    //第一列
                    dp[i][j] = dp[i-1][j]+grid[i][j];
                }
            }
        }
        return dp[x-1][y-1];
    }
}
```



## 2021-3-12

### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”



#### 思路

递归搜索，从下往上。如果一个root的两边都能找到节点，说明root就是最近的那个，直接把root往上传即可。如果只有一边的话，那也保持那个节点往上传，直到两者相遇。值得注意的是，如果p本身就是q的祖先，那么这样会出现我们把p的指针传到最上方的情况，这样也是符合要求的。



#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        return search(root,p,q);
    }

    TreeNode search (TreeNode root, TreeNode p, TreeNode q){
        if(root == null) return null;
        TreeNode left = search(root.left,p,q);
        TreeNode right = search(root.right,p,q);
        //先看左右子树有没有
        if(left!=null&&right!=null) return root; //如果都有 那就说明这就是最近的
        else if(root== p || root == q){ //接下来考虑自己是否的情况
            //如果自己是那么就返回自己的指针 
            return root;
        }
        else  if(left!=null) return left;
        else  if(right!=null) return right;
        return null;
    }
    
}
```



### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。



#### 思路

维护单调栈，类似于找下一个更大元素，遇到大的就弹栈，遇到小的压栈即可。



#### 题解

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        //等价于找下一个比他大的数字的位置
        //维护一个单调栈即可
        int size = T.length;
        Stack <Integer> stack = new Stack<Integer>();
        int [] result = new int [size];
        stack.push(0);
        for(int i=1;i<size;++i){
            while(!stack.empty()&&T[i]>T[stack.peek()]){
                //当前数比栈里面大
                int num = stack.pop();
                result[num] = i-num;
            }
            //此时小 压栈
            stack.push(i);
        }
        while(!stack.empty()){
            //里面的都没有更小的了
            result[stack.pop()] = 0;
        }
        return result;
    }
}
```



### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

给定一个二叉树，检查它是否是镜像对称的。



#### 思路

如果是对称的，那么左右的节点应该可以互换，那么分两只遍历即可，一个往左一个往右。



#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root,root);
    }

    boolean check(TreeNode p,TreeNode q){
        if(q==null && p == null){
            return true;
        }
        else if(q==null || p==null){
            return false;
        }
        //两者都非空
        return p.val==q.val&&check(p.left,q.right)&&check(p.right,q.left);
    }
}
```



### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。





#### 思路

BST的特性是左边小右边大，显然我们可以按照中序遍历，这样会得到一个递增序列，然后从后往前加即可。

优化：既然要从后往前，不如直接从右边开始遍历。



#### 题解：从左往右

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode convertBST(TreeNode root) {
        //bst满足左小右大 我们可以做中序遍历
        //这样得到一个递增序列 然后从后往前做累加和
        Stack <TreeNode> stack = new Stack<TreeNode>();
        List<TreeNode> list = new ArrayList<TreeNode>();
        TreeNode p = root;
        while(root!=null||!stack.empty()){
            //左
            while(root!=null){
                stack.push(root);
                root=root.left;
            }
            //中
            root = stack.pop();
            //加入到list
            list.add(root);
            //右
            root = root.right;

        }
        //每个数都是该数+他后面的所有数
        //所以叠加即可
        for(int i=list.size()-2;i>=0;i--){
            //从后往前
            list.get(i).val+=list.get(i+1).val;
        }
        return p;
    }
}
```



#### 题解：从右往左

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode convertBST(TreeNode root) {
        //bst满足左小右大 我们可以做中序遍历
        //这样得到一个递增序列 然后从后往前做累加和
        Stack <TreeNode> stack = new Stack<TreeNode>();
        List<TreeNode> list = new ArrayList<TreeNode>();
        TreeNode p = root;
        int sum = 0;
        while(root!=null||!stack.empty()){
            //右
            while(root!=null){
                stack.push(root);
                root=root.right;
            }
            //中
            root = stack.pop();
            root.val += sum;
            sum = root.val;
            //加入到list
            list.add(root);
            //左
            root = root.left;
        }
        return p;
    }
}
```



### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。



#### 思路

排序，然后找到相关元素即可。



#### 题解

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        //考虑排序然后直接返回相关元素即可
        //快排
        Arrays.sort(nums);
        return nums[nums.length-k];
    }
}
```





### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。



#### 题解

层次遍历，上一层遍历完就更新下一层即可。



#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if(root==null) return result;
        List<Integer> ans = new ArrayList<Integer>();
        List<TreeNode> tree = new ArrayList<TreeNode>(); 
        tree.add(root);
        int head=0,rail=0;
        int flag=0;//层数
        while(head<=rail){
            TreeNode p = tree.get(head);//获取头
            ans.add(p.val); //加到数组里
            if(p.left!=null){
                rail++;
                tree.add(p.left);
            }
            if(p.right!=null){
                rail++;
                tree.add(p.right);
            }
            if(head == flag ){//一层遍历结束
                flag = rail;
                //加入数据
                result.add(new ArrayList<Integer>(ans));
                ans.clear();
            }
            head ++;
        }
        return result;
    }
}
```





## 2021-3-13

### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

给你一个整数 n ，返回和为 n 的完全平方数的 最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。



#### 思路

DP。假设dp[n]是数字n的最小组合，那么dp[n]=min{dp[j]+dp[n-j]，其中1<j<n}。

考虑优化，我们优先考虑最大的那个组合数，例如24，我们先考虑16+8的情况。



#### 题解一

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int [n+1];
        for(int i=1;i<=Math.sqrt(n);++i){
            //把是完全平方数的都变成1
            dp[i*i] = 1;
        }
        for(int i=2;i<=n;++i){
            if(dp[i]==1) continue;
            int min =9999; ///寻找最小
            for(int j=i/2;j<i;++j){
                //dp[n] = min{dp[j]+dp[n-j]} 1<j<n
                int sum = dp[j]+dp[i-j];//记录加和
                if(sum<min) min = sum;
            }
            dp[i]=min;
        }
        return dp[n];
    }
}
```



#### 题解二

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int [n+1];
        for(int i=1;i<=Math.sqrt(n);++i){
            //把是完全平方数的都变成1
            dp[i*i] = 1;
        }
        for(int i=2;i<=n;++i){
            if(dp[i]==1) continue;
            int min =9999; ///寻找最小
            for(int j=1;j<Math.sqrt(i);++j){
                //dp[n] = min{dp[j]+dp[n-j]} 1<j<n
                int sum = dp[j*j]+dp[i-j*j];//记录加和
                if(sum<min) min = sum;
            }
            dp[i]=min;
        }
        return dp[n];
    }
}
```



## 2021-3-14

### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。



#### 思路

自顶向上归并排序，我们需要先统计整个的长度，然后通过多个指针来进行链表切割，然后调用合并有序链表函数进行合并。



#### 题解

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        //归并排序
        //通过合并有序链表来作为合并
        //我们只需要负责切割就可以了
        if(head == null) return head;
        int length = 0; //统计长度
        ListNode p = head;
        while(p!=null){
            length++;
            p=p.next;
        }
        //这样我们得到了length
        //开始自底向上归并
        ListNode node1 = new ListNode(0,head);//加一个空头来接上
        //从1开始做归并 1.2.4
        for(int sub = 1;sub<length;sub=sub<<1){
            ListNode pre = node1;//记录这一段的上一个指针
            ListNode now = node1.next; //用来遍历的指针
            while(now!=null){
                ListNode h1 = now; //记录第一个链表起始
                for(int i=1;i<sub && now.next!=null ;++i){
                    now = now.next;
                }
                ListNode h2 = now.next; //记录二链表起始
                now.next = null;//让链表一那一段成为一个独立链表
                now = h2;
                //第二段链表
                 for (int i=1; i < sub && now != null && now.next != null; i++) {
                    now = now.next;
                }
                ListNode next = null;
                if(now!=null ){ //后面还有
                    next =now.next;//下一个开始指针
                    now.next=null;//当前链表独立
                }
                ListNode merge = merge(h1,h2);//合并两段指针
                pre.next = merge;//接上
                //找到这段链表的最后 给下次接上
                while(pre.next!=null){
                    pre=pre.next;
                }
                now = next;//得到下一段开始
            }
        }
        return node1.next;
    }

     ListNode merge(ListNode l1, ListNode l2) {
        //特例
        if(l1==null&&l2==null) return null;
        else if(l1==null) return l2;
        else if(l2==null) return l1;
        //normal
        ListNode head,p;
        head=(l1.val > l2.val)?l2:l1;
        if(l1.val > l2.val) 
            l2=l2.next;
        else l1=l1.next;
        p=head;
        while(l1!=null&&l2!=null){
            //合并
            if(l1.val > l2.val){
                head.next=l2;
                head=head.next;
                l2=l2.next;
            }
            else{
                head.next=l1;
                head=head.next;
                l1=l1.next;
            }
        }
        if(l1!=null)
            head.next=l1;
        else if(l2!=null)
            head.next=l2;
        return p;
    }
}
```



### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。



#### 思路

重点在于如何判断两个字符串是字母异位词。考虑两种方法：

- 排序
- 26进制

在这里采取了排序法。

在讨论区看到用质数替代26进制的，觉得很酷。



#### 题解

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String,List<String>> mp = new HashMap<String, List<String>>();
        for(String str:strs){
            char [] array = str.toCharArray();//转换为数组
            Arrays.sort(array);
            String key = new String(array);
            //如果有就返回那个list 否则重新建立
            List<String> list = mp.getOrDefault(key, new ArrayList<String>());
            //加入这个string
            list.add(str);
            mp.put(key, list);
        }
        return new ArrayList<List<String>>(mp.values());
    }
}
```



### [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色



#### 思路

要原地排序，而数据又只是`1,2,0`，那我们可以考虑把0放最前面，2放最后面，这样1就自然在中间了。考虑以下的情况：

- 遍历到1
  - 直接跳过
- 遍历到0，往前交换
  - 交换的是1，不影响
  - 交换的是2：考虑到如果有2,2已经全部被放到后面去了，所以不存在这种可能
- 遍历到2，和后面的交换
  - 交换的是0：这样的话，我们要将其往前交换，为了维护代码，我们可以将遍历的指针回退到这个数
  - 交换的是1：不影响
  - 交换的是2：同样要往后交换，所以回退指针



#### 题解

```java
class Solution {
    public void sortColors(int[] nums) {
        //原地排序
        //考虑一前一后交换
        //一个指针表示0的最后一个
        //一个指针表示2的第一个
        int size = nums.length;
        int pos0=0,pos2=size-1;
        //当i遇到pos2的时候 说明后面全是2
        for(int i=0;i<=pos2;++i){
            if(0 == nums[i]){
                int temp = nums[pos0];//记录0
                nums[pos0] = nums[i];
                nums[i] = temp;
                pos0++;
            }
            else if(2 == nums[i]) {
                //2的情况 交换
                int temp = nums[pos2];//记录0
                nums[pos2] = nums[i];
                nums[i] = temp;
                pos2--;
                //注意回退
                i--;
            }
            //遇到1跳过即可
        }
    }
}
```



## 2021-3-15

### [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。



#### 思路

直接模拟，注意方向和已经遍历过的就不能在遍历即可。



#### 题解

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<Integer>();
        int x = matrix.length; 
        if(x==0) return result;
        int y = matrix[0].length;
        if(y==0) return result;
        int i=0,j=0;
        int num=0;
        int dict = 0;//0右 1下 2左 3上
        int [][]direction = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};;
        while(num<x*y){
            result.add(matrix[i][j]);
            matrix[i][j] = 101;//已走过的标记下
            int newx = i + direction[dict][0];
            int newy = j + direction[dict][1];
            if(newx==x || newx<0 ||newy==y||newy<0 || matrix[newx][newy]==101){
                //换方向
                dict = (dict+1)%4;
                //新的坐标
                newx = i + direction[dict][0];
                newy = j + direction[dict][1];
            }
            i = newx;
            j = newy;
            num++;
        }
        return result;
    }
}
```





### [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。



#### 思路

通过一个栈来实现，但是栈不止存储数据，还存储当前位置对应的最小元素。



#### 题解

```java
class MinStack {
    Stack <List<Integer>> stack;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<List<Integer>> ();
    }
    
    public void push(int x) {
        List<Integer> add = new ArrayList<Integer>();
        if(stack.empty() || x<stack.peek().get(1)){
            //new min or empty stack
            add.add(x);
            add.add(x);
        }
        else{
            add.add(x);
            add.add(stack.peek().get(1));
        }
        stack.push(add);
    }
    
    public void pop() {
        stack.pop();
    }
    
    public int top() {
        return stack.peek().get(0);
    }
    
    public int getMin() {
        return stack.peek().get(1);
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```





### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。



#### 思路

我们可以明显的感受到，一条最大路径一定是一个倒V字的路径，那么我们就可以考虑通过遍历二叉树计算每个节点的深度，我们假定root.left的深度为n，right的深度为m，那么其路径长度就会是`m+n+2`，我们只需要在遍历的过程中拿一个数据进行数据保存即可。



#### 题解

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int maxLength = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        //可能穿过根节点 也可能不穿过
        //但是我们可以证明其总是一个倒V字 也即
        //一个root节点两边子树深度的加和
        //设置全局变量存储max 即可
        //遍历求深度
        getDepth(root);
        return maxLength;
    }

    int getDepth(TreeNode root){
        //处理null
        if(null == root) 
            return -1;
        //后序遍历
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        //计算当前节点的最大的直径
        //注意要算上当前root节点加入的2
        if(left+right+2 > maxLength) {
            maxLength = left + right +2;
        }
        //向上返回depth
        return (left>=right)?left+1:right+1;
    }
}
```



## 2021-3-16

### [59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix`。



#### 思路

模拟。通过一个方向数组控制方向，对一个数组进行赋值，注意不要走回到已经赋值过的位置即可。



#### 题解

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int [][] result = new int [n][n];
        int flag = 0;
        int i = 0,j = 0;
        int [][] direction = {{0,1},{1,0},{0,-1},{-1,0}};
        int num = 1;
        int size = n*n;
        while(num <= size){
            result [i][j] = num;
            int newi = i+direction[flag][0];
            int newj = j+direction[flag][1];
            //判断是否符合
            if(newi<0||newi==n||newj<0||newj==n||result[newi][newj]!=0){
                flag = (flag+1)%4;
                newi = i+direction[flag][0]; 
                newj = j+direction[flag][1];
            }
            i = newi;
            j = newj;
            num ++;
        }
        return result;
    }
}
```





### [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。



#### 思路

构建一棵树，通过书上的节点表示是不是一个字母并且保存他的下级。

![image-20210316105410921](https://i.loli.net/2021/03/16/lVBiCOkapId9JAr.png)



#### 题解

```java
class Trie {
    class TireNode {
        private boolean is_string;
        TireNode[] next;

        public TireNode() {
            is_string = false;
            next = new TireNode[26];
        }
    }

    private TireNode root;
    
    /** Initialize your data structure here. */
    public Trie() {
        root = new TireNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        int i;
        int size = word.length();
        TireNode node = root;
        for(i=0; i<size; ++i){
            if(node.next[word.charAt(i)-'a']==null){
                //new
                node.next[word.charAt(i)-'a'] = new TireNode();
            }
            node = node.next[word.charAt(i)-'a'];
            
        }
        node.is_string = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        int i;
        int size = word.length();
        TireNode node =root;
        for(i=0; i<size; ++i){
            if(node.next[word.charAt(i)-'a']==null){
                //new
                return false;
            }
            node = node.next[word.charAt(i)-'a'];
        }
        return node.is_string;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        int i;
        int size = prefix.length();
        TireNode node = root;
        for(i=0; i<size; ++i){
            if(node.next[prefix.charAt(i)-'a']==null){
                return false;
            }
            else{
                node = node.next[prefix.charAt(i)-'a'];
            }
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```





## 2021-3-18

### [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 





#### 思路

设置一个计数器，来记录我们遍历到了第几个节点。为了防止第一个节点就是left，所以我们要给链表头加一个，这样遇到第一个节点是left的情况，就可以进行翻转了。

在翻转的时候，我们先对整个链表拆分成`A,B,C`三段，只有B段要做翻转，所以我们要把B插入到A的后面，然后把A接上去，基于这种思路，我们可以用两个指针保存下left和right的节点，因为他们必然是一头一尾，然后我们翻转B，接上去即可。



#### 题解

````java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if(left == right)  return head;//无须翻转
        //先找到要反转的位置
        ListNode p = new ListNode(0,head);//dummy
        head = p;
        int length = 1;
        while(length != left){
            p=p.next;
            length++;
        }
        //此时p.next就是要反转的位置
        ListNode pre = p;
        //因为left!=right 所以必存在p.next.next
        ListNode leftL = p.next; //记录左边节点
        p = leftL.next;
        //p从left的下一个开始 
        while(length != right){
            //假定A->b->c->d 要成为 A->c->b->d
            ListNode temp = pre.next;
            //A->c
            pre.next = p;
            //往下移动 
            p = p.next;
            //c->b
            pre.next.next = temp;
            length++;
        }
        //已经翻转结束
        //连接翻转和未到达的节点
        //连接B C
        leftL.next = p;
        return head.next;
    }
}
````





## 2021-3-21

### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。



#### 思路

考虑单调队列。先考虑一个窗口，如果我们维护一个单调栈的话，那么显然的，我们可以求得最小/最大值，这个数肯定会在这个单调栈的最底部。那么我们将其变成一个双端队列，当这个滑动窗口滑动的时候，如果最左边的数不在窗口里，我们就可以去掉，然后继续维护这个单调栈即可，这样我们就可以继续在底部得到最大/最小值。



#### 题解

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        //单调队列
        //所有的都会经历入队与出队
        //维护一个单调递减的队列 这样队头就会是最大的那个
        //注意窗口移动的时候要删除窗口外的内容
        int size = nums.size();
        deque <int> queue;//双端队列
        for(int i=0;i<k;++i){
            //初始化队列
            while(!queue.empty() && nums[i]>nums[queue.back()]){
                queue.pop_back();//弹出
            }
            //当前要加入的是最小的
            queue.push_back(i);
        }
        //设置答案
        vector<int> result={nums[queue.front()]};//加入当前窗口的最大值
        int window_size = k;
        while(k<size){
            //下一个窗口
            //移出不在窗口元素和加入元素可以交换位置 因为取最大最后才做
            while(!queue.empty() && nums[k]>nums[queue.back()]){
                queue.pop_back();//弹出
            }
            queue.push_back(k);
            //显然 这个队列的index应该是递增的 所以左边的是最小的
            if(window_size<=k-queue.front()){
                //0 k k
                queue.pop_front();
            }
            result.push_back(nums[queue.front()]);
            k++;
        }
        return result;
    }
};
```





### [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。



#### 思路

经典dp。我们求n处的子序列长度，我们可以考虑n前面比n处小的元素的最长子序列，将该序列加1就成为了n处的可能长度，从这些可能长度中我们取出最大的那个即可。

值得注意的是，并不是最后一个元素一定是最大的，所以需要维护一个最大值变量。



#### 题解

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        //dp 假定dp[n]表示包含n的最长子序列的长度
        //那么dp[n] = dp[k]+1 k是比n小的数字的序号
        int size = nums.size();
        int dp[size+1];
        int max = 0;
        dp[1] = 0;
        for(int i = 2;i<=size;++i){
            dp[i] = 0;
            for(int j = 1;j<i;++j){
                if(nums[j-1]<nums[i-1]&&dp[j]+1>dp[i]){
                    //比他小
                    dp[i] = dp[j]+1;
                }
            }
            //更新最大值
            if(dp[i]>max) max = dp[i];
        }
        return max+1;
    }
};
```





### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

给你一个字符串 `s`，找到 `s` 中最长的回文子串。



#### 思路

dp。如果aBa回文，那么B必定回文。基于这个就可以寻找递推关系。

此外，可以考虑从最中间的某个节点往两边拓展，直到两边不一样为止。



#### 题解

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        //dp
        //如果一个字符串aBa是回文串 那么B也是回文串
        int size = s.length();
        if(size == 1) return s;
        int dp [size][size]; //
        int left = 0;
        int right = 0;
        //单个字符总是真的
        for(int i=0;i<size;++i){
            for(int j=0;j<size;++j){
                dp[i][j] = 1;
            }
        }
        //dp
        for(int i=size-1;i>=0;--i){
            for(int j=i+1;j<size;++j){
                if(dp[i+1][j-1]==1 && s[i] == s[j]){
                    dp[i][j] = 1;
                    if(j-i > right-left) {
                        right = j;
                        left = i;
                    }
                }
                else{
                    dp[i][j] = 0;
                }
            }
        }
        return s.substr(left,right-left+1);
    }
};
```





### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。





#### 思路

三个数，简单的思路就是全部搜索，然后去重。

考虑如下一个问题，如果我们确定一个数的值，那么问题就变成了是否存在两个数，使得除了确定数的加和为某个值，这样就很符合我们遇到过的两数之和的问题了。

值得注意的一个问题是，在找数的时候，可能切换到下一个时和现在的是一样的，所以我们要注意略去一样的情况。



#### 题解

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int size = nums.size();
        if(size<3) return result;
        //先排序
        sort(nums.begin(),nums.end());
        //固定一个数
        for(int i = 0;i<size-1;++i){
            //和上次一样就不考虑
            //这里不能使用和下一个不一样 因为可能会有 -1 -1 2 这样的解答
            if(i>0 && nums[i-1]==nums[i]){
                continue;
            }
            //双指针
            int j = i+1,k = size -1;
            for(;j<k;++j){
                //和上次一样
                if(j>i+1 && nums[j-1]==nums[j]){
                    continue;
                }
                //调整右指针
                while(j<k&&nums[i] + nums[j] + nums[k] >0){
                    k--;
                }
                //跳出
                if(j>=k){
                    break;
                }
                if(nums[i] + nums[j] + nums[k] == 0){
                    //加入解
                    vector <int> ans = {nums[i] , nums[j] , nums[k]};
                    result.push_back(ans);
                }
            }
        }
        return result;
    }
};
```





### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

给定一个二叉树，判断其是否是一个有效的二叉搜索树。



#### 思路

对BST做中序遍历，可以得到一个升序的列表。基于这个特征，进行中序遍历并判断。



#### 题解

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if(root == nullptr) return false;
        //中序遍历 
        //验证是否上升
        stack <TreeNode*> s;
        TreeNode* lastOne = nullptr; //记录上一个数 
        while(root!=nullptr || !s.empty()){
            //向左
            while(root != nullptr){
                s.push(root);
                root = root->left;
            }
            //中间
            root = s.top();
            s.pop();
            //不是上升
            if(lastOne != nullptr && lastOne->val>=root->val){
                return false;
            }
            else{
                lastOne = root;//初始化
            }
            //右边
            root = root->right;
        }
        return true;
    }
};
```



## 2021-3-22

### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的索引，否则返回 -1 。



#### 思路

整个列表虽然变成了两段升序，但是总体上我们仍然可以认为其是有序的，可以进行二分搜索。通过多个条件来进行分类，从而修改left和right的值即可。



#### 题解

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
            //原来升序 变化以后是 
            // a->b->c->d 其中ab升序 cd升序 且d小于a
            //我们可以考虑一个循环队列 从里面找整个数据 二分法
            int size = nums.size();
            if(size == 0) return -1;
            else if(size == 1) return (target==nums[0])?0:-1;
            int left = 0,right = size - 1;
            //注意跳出条件
            while(left <= right){
                //mid
                int mid = (left+right)/2;
                if(target == nums[mid]){
                    return mid;
                }
                if(nums[mid]>=nums[0]){
                    //注意控制新的left/right
                    //target比mid大 要么是左边 要么就往右 我们可以通过nums[0]来确定
                    //比较下确定如何切割
                    if(target>=nums[0] && target<nums[mid]){
                        right = mid - 1;
                    }
                    else {
                        left = mid + 1;
                    }
                }
                else{
                    if(target>nums[mid] && target<=nums[size-1]){
                        left = mid + 1;
                    }
                    else{
                        right = mid -1;
                    }
                }
            }
            return -1;
    }
};
```







## 2021-3-23

### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。



#### 思路

我们从一个柱子的角度来看，如果这个柱子就是他所在的矩形里最小的那个，那么属于听他的最大面积就是他所能拓展的最长的长度*他的高度。所以我们要找上一个比他小的和下一个比他小的，这样我们就能找到这个柱子的最远边界从而来进行比较。

而找上一个小的/下一个小的，比较合适的数据结构就是单调栈。



#### 题解

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        //单调栈
        //找到某个柱子左边右边的最大拓展情况 
        //这基于一个常识 如果所有的都比这个柱子大 那么再拓展一个也无所谓
        int size = heights.size();
        vector<int> left(size);
        vector<int> right(size,size);
        stack <int> S;
        //求左边 找比他小的
        for(int i=0;i<size;++i){
            while(!S.empty() && heights[i]<=heights[S.top()]){
                right[S.top()] = i;
                S.pop();
            }
            left[i] = S.empty()?-1:S.top();
            S.push(i);
        }
        //根据left和right 计算最大
        int result = 0;
        for(int i=0;i<size;++i){
         //   cout<<"left:"<<left[i]<<"right:"<<right[i]<<endl;
            int re = heights[i]*(right[i]-left[i]-1);
            if(re>result){
                result = re;
            }
        }
        return result;
    }
};
```





### [912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)

给你一个整数数组 `nums`，请你将该数组升序排列。



#### 思路

熟悉常见排序算法。



#### 题解 快排

```c++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int size = nums.size();
        pos(nums,0,size-1);
        return nums;
    }
    
    void pos(vector<int>& nums,int left,int right){
        if(right<=left) return ;
        int l = left, r = right;
        int x = nums[l];//取出l处的数据等待填补
        while(l<r){
            //右边往左边看
            while(l<r && nums[r]>x){
                r--;
            }
            if(l<r){
                nums[l] = nums[r];
                l++;
            }
            //左边往右边看
            while(l<r && nums[l]<x){
                l++;
            }
            if(l<r){
                nums[r] = nums[l];
                r--;
            }
        }
        //相等的时候弹出
        nums[l] = x;

        pos(nums,left,l-1);
        pos(nums,l+1,right);
    }
};
```



#### 题解 堆排









### [678. 有效的括号字符串](https://leetcode-cn.com/problems/valid-parenthesis-string/)

给定一个只包含三种字符的字符串：（ ，） 和 *，写一个函数来检验这个字符串是否为有效字符串。有效字符串具有如下规则：

任何左括号 ( 必须有相应的右括号 )。
任何右括号 ) 必须有相应的左括号 ( 。
左括号 ( 必须在对应的右括号之前 )。

\* 可以被视为单个右括号 ) ，或单个左括号 ( ，或一个空字符串。
  一个空字符串也被视为有效字符串





#### 思路

考虑这样一个问题，因为\*可以成为左括号也可以成为右括号，如果我们去遍历各种情况显然是不合理的。我们可以考虑一个区间，这个区间记录了左括号的最后范围。我们假定当前的范围为[l,r]，那么遇到左括号的时候，这个范围就会变成[l+1,r+1]，遇到右括号的时候，由于被匹配掉了一个左括号，所以两个都要减，值得注意的是，这里r并不能小于0，这取决于这样的一个思路：如果r小于0，那么说明在个字符串里，左括号的最大值都不够右括号匹配的，那么后面的全部都变得没有意义了。值得注意的是，l并不能等于或小于0，这是因为l表示的是最少的情况，但是因为*号的存在，他可以取空也可以取其他的，这可以自由决定。



#### 题解

```c++
class Solution {
public:
    bool checkValidString(string s) {
        //*可以看成是任意一个字符
        //那么匹配完后 括号数量应该是一个区间
        //看看这个区间是不是能取到0
        int size = s.length();
        int left = 0, right = 0;
        for(int i=0 ;i < size ; ++i){
            if(s[i] == '('){
                left ++;
                right ++;
            }
            else if(s[i]==')'){
                //右括号
                if(left>0){ //匹配掉一个左括号
                    left --;
                }
                right --;
            }
            else if(s[i]=='*'){
                //*号
                if(left>0){ //可以匹配掉一个左括号
                    left --;
                }  
                right ++; //也可以变成一个左括号
            }
                    //如果不够匹配 显然不适合
        if(right < 0)
            return false;
        }
        //看看是否能全部匹配掉
        return left == 0;
    }
};
```



### [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

给定一个链表，旋转链表，将链表每个节点向右移动 *k* 个位置，其中 *k* 是非负数。



#### 思路

原来想着找到断开的位置然后接一下就行，但是发现k的值可能超出链表长度，比较麻烦，于是尝试把他合成一个环，然后找到相关的位置即可。



#### 题解

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        //先闭合成环
        //在切割
        //为了切割方便 我们可以用多个指针记录
        if(head == nullptr || head->next ==nullptr) return head;
        ListNode *now = head->next; //当前指针
        ListNode *pre = head;
        int length = 1;
        while(now!=nullptr){
            now = now->next;
            pre = pre->next;
            length ++;
        }
        pre->next = head;
        //已成环 开始拆
        for(int i=0;i<length-(k)%length;++i){
            //找到断开的那个点
            pre = pre->next;
        }
        now = pre->next;
        pre->next = nullptr;
        return now;
    }
};
```





## 2021-3-25

### [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除链表中所有存在数字重复情况的节点，只保留原始链表中 没有重复出现 的数字。

返回同样按升序排列的结果链表。



#### 思路

通过一个变量来判断是否出现了一致的，出现一致的则把这一段全部删去，否则就保留。注意第一个也可能被删去，所以需要加一个`dummy`。



#### 题解

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr) return head;
        ListNode * dummy = new ListNode(0,head);
        ListNode * now = dummy; //遍历
        while(now!=nullptr){
            ListNode * next = now -> next;
            int flag = 0;
            while(next != nullptr && next->next!=nullptr && next->val == next->next->val){
                //相等 下跳一个
                next = next->next;
                flag = 1;
            }
            if(flag == 1){
                now -> next = next -> next;
            }
            else
                now = now -> next;
        }
        return  dummy->next;
    }
};
```





## 2021-3-26

### [面试题 16.17. 连续数列](https://leetcode-cn.com/problems/contiguous-sequence-lcci/)

给定一个整数数组，找出总和最大的连续数列，并返回总和。



#### 思路

对于某个数，他此时的最大值有两种情况，加入他前面的，或者从自己开始，所以我们比较这两个值，然后在进行 判断即可。



#### 题解

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int size = nums.size();
        int sum = 0; //上一个的和
        int Max = nums[0]; //当前和
        for(int i=0;i<size;++i){
            sum = max(nums[i],sum+nums[i]);
            Max = max(Max,sum);
        }
        return Max;
    }
};
```





### 2021-3-29

### [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

颠倒给定的 32 **位无符号整数的二进制位**。



#### 思路

分治法。一种非常酷炫得位运算解法，没见过确实不太容易想到。



#### 题解

```c++
class Solution {
    const uint32_t M1 = 0x55555555; // 01010101010101010101010101010101
    const uint32_t M2 = 0x33333333; // 00110011001100110011001100110011
    const uint32_t M4 = 0x0f0f0f0f; // 00001111000011110000111100001111
    const uint32_t M8 = 0x00ff00ff; // 00000000111111110000000011111111

public:
    uint32_t reverseBits(uint32_t n) {
        n = n >> 1 & M1 | (n & M1) << 1;
        n = n >> 2 & M2 | (n & M2) << 2;
        n = n >> 4 & M4 | (n & M4) << 4;
        n = n >> 8 & M8 | (n & M8) << 8;
        return n >> 16 | n << 16;
    }
};
```



### [面试题 08.01. 三步问题](https://leetcode-cn.com/problems/three-steps-problem-lcci/)

三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007



#### 思路

设置三个变量，分别保存前面三步的情况。



#### 题解

```c++
class Solution {
public:
    int waysToStep(int n) {
        if(n <= 2) return n;
        else if(n == 3) return 4;
        long long n1=1,n2=2,n3=4;
        for(int i=3;i<n;++i){
            long long x = n1 + n2 + n3;
            n1 = n2%1000000007;
            n2 = n3%1000000007;
            n3 = x%1000000007;
        }
        return n3;
    }
};
```



### [392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。



#### 思路

直接遍历t，看看是否能够在其中顺序找到s的所有字符即可。



#### 题解

```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if(s.length() == 0) return true;
        int size = t.length();
        int j = 0;
        for(int i=0;i<size;++i){
            if(t[i] == s[j]){
                j++;
                if(j == s.length()){
                    return true;
                }
            }
        }
        return false;
    }
};
```





### [1641. 统计字典序元音字符串的数目](https://leetcode-cn.com/problems/count-sorted-vowel-strings/)

给你一个整数 n，请返回长度为 n 、仅由元音 (a, e, i, o, u) 组成且按 字典序排列 的字符串数量。

字符串 s 按 字典序排列 需要满足：对于所有有效的 i，s[i] 在字母表中的位置总是与 s[i+1] 相同或在 s[i+1] 之前。



#### 思路

考虑n和n+1的关系，n+1比n多了个数，可以把这个数放到开头，然后考虑开头和这个n串的关系即可。比如前面是a，那么后面只能跟aeiou，以此类推。



#### 题解

```c++
class Solution {
public:
    int countVowelStrings(int n) {
        vector<int> dp(5,1);// a e i o u
        for(int i=2;i<=n;++i){
            for(int j = 3;j>=0; --j){
                dp[j]+=dp[j+1];
            }
        }
        return accumulate(dp.begin(),dp.end(),0);
    }
};
```





### 2021-3-30

#### [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数





#### 思路

先对块做二分，在对这个块做二分。

思路二，也可以把这个图形看成是从右上角开始的一棵二叉搜索树。



#### 题解

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        //先对块做分析
        int size = matrix.size();
        int left = 0,right = size-1;
        int mid;
        //二分找在哪个块里
        while(left<=right){
            mid = left + (right-left)/2;//二分
            if(matrix[mid][0]>target){
                right = mid - 1;
            }
            else if(matrix[mid][matrix[mid].size()-1]<target){
                left = mid +1 ;
            }
            else{
                break;
            }
        }
        left = 0;
        right = matrix[mid].size()-1;
        while(left<=right){
            int middle = left + (right - left)/2;
            if(matrix[mid][middle] == target){
                return true;
            }
            else if(matrix[mid][middle] > target){
                //大于
                right = middle -1;
            }
            else{
                left = middle +1;
            }
        }
        return false;
    }
};
```





### [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

给定一个非空的整数数组，返回其中出现频率前k高的元素。



#### 思路

统计出现频率，做堆排。



#### 题解

```c++
class Solution {
public:
    static bool cmp(pair<int, int>& m, pair<int, int>& n) {
        return m.second > n.second;
    }

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> occurrences;
        for (auto& v : nums) {
            occurrences[v]++;
        }

        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> q(cmp);
        for (auto& [num, count] : occurrences) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num, count);
                }
            } else {
                q.emplace(num, count);
            }
        }
        vector<int> ret;
        while (!q.empty()) {
            ret.emplace_back(q.top().first);
            q.pop();
        }
        return ret;
    }
};
```



### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的





#### 思路

经典dp。转移方程f[n] = min{f[n-k] +1}，其中k是硬币币值。



#### 题解

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(amount==0) return 0;
        //经典dp
        //用dp[n]表示凑到n个硬币最少的钱
        vector<int> dp(amount +1,0);
        int size = coins.size();
        int flag = 0;
        for(int i=0;i<size;++i){
            if(coins[i]<=amount){
                flag = 1;
                dp[coins[i]] = 1;//设置初值
            }
        }
        if(flag == 0) return -1;//全都比amount大 
        for(int i = 1;i <= amount ;++i){
            //面值 跳过
            if(dp[i] == 1) continue;
            int min = 99999;
            for(int j=0;j<size;++j){
                int d = i - coins[j];
                if(d>0){
                    //能够用这个硬币来换
                    if(dp[d]+1<min){
                        min = dp[d] +1;
                    }
                }
            }
            dp[i] = min;
        }
        return (dp[amount] == 99999)?-1:dp[amount];
    }
};
```

