## 1. 题目

### 1078: Bigram分词

https://leetcode.cn/problems/occurrences-after-bigram/

思路：
简单的对字符串首先按空格分离之后，用循环挨个搜索是否有满足要求的连续单词，有就提取second即可


代码：

```python
class Solution(object):
    def findOcurrences(self, text, first, second):
        """
        :type text: str
        :type first: str
        :type second: str
        :rtype: List[str]
        """
        s=text.split(' ')
        n=len(s)
        a=[]
        for i in range(n-2):
            if s[i]==first and s[i+1]==second:
                a.append(s[i+2])
        return a
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image.png)




### 283.移动零

stack, two pinters, https://leetcode.cn/problems/move-zeroes/


思路：
这里采用的办法比较暴力，将字符串中的零的个数进行计数，删去再在末尾加上。


代码：

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        n=len(nums)
        x=0
        sum=0
        for i in range(n):
            if nums[i]==0:
                sum+=1
        for i in range(sum):
            nums.remove(0)
        for i in range(sum):
            nums.append(0)
        return nums
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-1.png)



### 20.有效的括号

stack, https://leetcode.cn/problems/valid-parentheses/

思路：
使用栈进行处理，注意到只要搜索到右括号的时候上一个栈末尾不能对应皆为不合法，由此判断即可。
代码：

```python
class Solution:
    def isValid(self, s):
        pairs = {
            ")": "(",
            "]": "[",
            "}": "{",
        }
        a = list()
        for ch in s:
            if ch in pairs:
                if not a or a[-1] != pairs[ch]:
                    return False
                a.pop()
            else:
                a.append(ch)
        
        return not a
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-2.png)



### 118.杨辉三角

dp, https://leetcode.cn/problems/pascals-triangle/

思路：

最基础的dp问题，转移方程可以根据规则一目了然，即s.append(a[i-1][j]+a[i-1][j+1])，这里的注意到python相较于c++不能自己先定义一个固定长度的数组，在规划dp数组时有区别

代码：

```python
class Solution(object):
    def generate(self, numRows):
        """
        :type numRows: int
        :rtype: List[List[int]]
        """
        a=[[1],[1,1]]
        if numRows==1:
            return [[1]]
        if numRows==2:
            return a
        for i in range(2,numRows):
            s=[1]
            for j in range(i-1):
                s.append(a[i-1][j]+a[i-1][j+1])
            s.append(1)
            a.append(s)
        return a
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-3.png)



### 46.全排列

backtracking, https://leetcode.cn/problems/permutations/

思路：

这里我想直接使用深度搜索，按位爆搜即可完成。但是这里由于python的列表地址问题遇到了一点困难，因为python的列表名都是一个指向地址的指针，所以很麻烦，需要复制副本进行提交，并且在返回上一层循环后列表就会带着下一层的改动返回，这里需要注意将列表元素进行回溯，这里感觉不如c++方便。

代码

```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        result = []
        n = len(nums)
        
        def backtrack(path):
            if len(path) == n:
                result.append(path[:])
                return
            
            for num in nums:
                if num not in path:
                    path.append(num)
                    backtrack(path)
                    path.pop() 
        backtrack([])
        return result
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-4.png)




### 78.子集

backtracking, https://leetcode.cn/problems/subsets/

思路：
我这里采用的也是搜索的方法，在每一步判断是否加入新元素，相当于一个完全二叉树，其实与上题极其相似，我只做了些许修改。
另一方面，这里也容易想到迭代的方法，与杨辉三角相似，我们可以迭代形成各列表长度的结果。


代码

```python
class Solution(object):
    def subsets(self, nums):
        result = []
        n = len(nums)
        
        def backtrack(path, index):
            if index == n:
                result.append(path[:])
                return
            backtrack(path, index + 1)
            path.append(nums[index])
            backtrack(path, index + 1)
            path.pop()
        backtrack([], 0)
        return result
```



<mark>（至少包含有"Accepted"）</mark>


![alt text](image-5.png)


## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>
这次的题目开始接触简单的算法，主要是写了一些比较基础的搜索方法，体会到了python在列表处理搜索上的一些麻烦，总体还算顺利。
我在课外也开始接触一些ai，在我本研项目中我着重展开了对katago这一智能体的研究，不仅完成了本地部署，并且对其应用和输出处理都有了一定的了解，感受到了ai时代分析工具的强大，开始学着与ai合作，以及本地如何更好的处理ai。