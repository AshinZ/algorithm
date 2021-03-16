# 剑指Offer



## 2021-3-16

### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。



#### 思路

截取字符串然后拼接。



#### 题解

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        return s.substring(n,s.length())+s.substring(0,n);
    }
}
```



### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

请完成一个函数，输入一个二叉树，该函数输出它的镜像。



#### 思路

遍历二叉树，交换节点。



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
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null )return null;
        Stack <TreeNode> stack = new Stack<TreeNode>();
        TreeNode p = root;
        stack.push(p);
        while(!stack.empty()){
            TreeNode q = stack.pop();
            if(q.right!=null){
                stack.push(q.right);
            }
            if(q.left !=null){
                stack.push(q.left);
            }
            TreeNode temp = q.left;
            q.left = q.right;
            q.right = temp;
        }
        return p;
    }
}
```





### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。



#### 思路

层次遍历/DFS。



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
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        int max = 0;
        List<TreeNode> queue = new ArrayList<TreeNode>();
        queue.add(root);
        int head = 0, rail = 0,floor = 0;
        while(head<=rail){
            TreeNode p = queue.get(head);
            if(p.left != null){
                queue.add(p.left);
                rail++;
            }
            if(p.right != null){
                queue.add(p.right);
                rail++;
            }
            if(head == floor){
                floor = rail;
                max++;
            }
            head++;
        }
        return max;
    }
}
```





### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。



#### 思路

法一：先算长度。然后在遍历

法二：双指针，一个先跑k个，另一个才开始跑，这样跑到底慢的指针就符合要求了。



#### 题解一

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        int length=0;
        ListNode p = head;
        //算出总长度
        while(p!=null){
            p = p.next;
            length++;
        }
        p = head;
        for(int i=0;i<length-k;++i){
            p = p.next;
        }
        return p;
    }
}
```



#### 题解二

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode p = head;
        //先拿出k个指针 然后开始双指针
        for(int i=0;i<k;++i){
            p = p.next;
        }
        ListNode q = head;
        while(p!=null){
            p = p.next;
            q=q.next;
        }
        return q;
    }
}
```



### [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。



#### 思路

先算出多少次，在打印即可。



#### 思考

本题返回的是`int`，所以不需要考虑溢出等问题，但是如果不是`int`的话，应该用字符串尝试实现。



#### 题解

```java
class Solution {
    public int[] printNumbers(int n) {
        int max = 1;
        for(int i=0;i<n;++i)
            max*=10;
        max--;
        int [] result = new int [max];
        for(int i=0;i<max;++i){
            result[i]=i+1;
        }
        return result;
    }
}
```





### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。



#### 思路

用`StringBuffer`模拟即可，遍历一遍。



#### 题解

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuffer result = new StringBuffer();
        int size = s.length();
        for(int i=0;i<size ;++i){
            char x = s.charAt(i);
            if(x!=' '){
                result.append(x);
            }
            else{
                result.append("%20");
            }
        }
        return result.toString();
    }
}
```





### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

给定一棵二叉搜索树，请找出其中第k大的节点。



#### 思路

中序遍历，从右往左，右、中、左，这样就可以实现先访问大的再访问小的了。



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
    public int kthLargest(TreeNode root, int k) {
        //中序遍历
        //右中左即可
        Stack <TreeNode> stack = new Stack<>();
        int min;
        while(root!=null || !stack.empty()){
            while(root!=null){
                stack.push(root);
                root = root.right;
            }
            root = stack.pop();
            k--;
            if(k==0) return root.val;
            root = root.left;
        }
        return 0;
    }
}
```



### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）



#### 思路

加入到数组，然后翻转下。



#### 题解

```c++
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







### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。



#### 思路

在遍历链表的过程中，把后面的往前接。



#### 题解

```c++
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







### [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。



#### 思路

根据`a`&`a-1`的二进制的特性，我们可以快速的计算1的个数。



#### 题解

```java
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int result = 0;
        while(n!=0){
            n = n&(n-1);
            result ++;
        }
        return result;
    }
};
```





### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。



#### 思路

一个链表一个指针，插入即可。



#### 题解

```c++
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
```





### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )



#### 思路

一个是输入栈，一个是输出栈。队列插入的时候，往输入栈中push，出队的时候，从输出栈里pop，如果输出栈是空，那就把输入栈倒着加进来即可，如果两个都空，就返回-1。



#### 题解

```java
class CQueue {
    Stack <Integer> in;
    Stack <Integer> out;

    public CQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }
    
    public void appendTail(int value) {
        in.push(value);
    }
    
    public int deleteHead() {
        if(!out.empty()){
            //输出的不为空
            return out.pop();
        }
        if(in.empty()&&out.empty()){
            //都空
            return -1;
        }
        else{
            //输出为空 输入不为空
            while(!in.empty()){
                out.push(in.pop());
            }
        }
        return out.pop();
        
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

