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

