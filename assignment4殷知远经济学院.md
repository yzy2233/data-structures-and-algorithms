## 1. 题目

### E18161: 矩阵运算

matrices, http://cs101.openjudge.cn/pctbook/E18161/

请使用`@`矩阵相乘运算符。

思路：
用时： 40min
这里没完全理解如何使用@运算符，因为@似乎是numpy里的运算符？
所以我没使用@运算符，直接手写了矩阵乘法。

后续查询了解到是要自己打包矩阵运算，所以以下给出两种代码
代码：

```python
a1,b1=input().split()
a1,b1=int(a1),int(b1)
m1=[]
for i in range(a1):
    m1.append(list(map(int,input().split())))
a2,b2=input().split()
a2,b2=int(a2),int(b2)
m2=[]
for i in range(a2):
    m2.append(list(map(int,input().split())))
a3,b3=input().split()
a3,b3=int(a3),int(b3)
m3=[]
for i in range(a3):
    m3.append(list(map(int,input().split())))
if a1!=a3 or b2!=b3 or b1!=a2:
    print('Error!')
else:
    m4=[]
    for i in range(a1):
        s=[]
        for j in range(b2):
            sum=0
            for k in range(a2):
                sum+=m1[i][k]*m2[k][j]
            s.append(sum)
        m4.append(s)
    for i in range(a3):
        for j in range(b3):
            if j!=b3-1:
                print(m3[i][j]+m4[i][j],' ',end='',sep='')
            else:
                print(m3[i][j]+m4[i][j],end='')
        if i!=a3-1:
            print('')
```
```python
def read_matrix():
    try:
        row, col = map(int, input().split())
        matrix = []
        for _ in range(row):
            matrix.append(list(map(int, input().split())))
        return matrix, row, col
    except:
        return None, 0, 0

def matrix_multiply(A, B):
    """矩阵乘法 A @ B"""
    rows_A, cols_A = len(A), len(A[0])
    rows_B, cols_B = len(B), len(B[0])
    if cols_A != rows_B:
        return None
    
    result = [[0] * cols_B for _ in range(rows_A)]

    for i in range(rows_A):
        for j in range(cols_B):
            for k in range(cols_A):
                result[i][j] += A[i][k] * B[k][j]
    
    return result

def matrix_add(A, B):
    rows_A, cols_A = len(A), len(A[0])
    rows_B, cols_B = len(B), len(B[0])
    if rows_A != rows_B or cols_A != cols_B:
        return None
    result = [[0] * cols_A for _ in range(rows_A)]
    for i in range(rows_A):
        for j in range(cols_A):
            result[i][j] = A[i][j] + B[i][j]
    
    return result

def main():
    A, rows_A, cols_A = read_matrix()
    B, rows_B, cols_B = read_matrix()
    C, rows_C, cols_C = read_matrix()

    if A is None or B is None or C is None:
        print("Error!")
        return
    
    # 计算 A @ B
    AB = matrix_multiply(A, B)
    if AB is None:
        print("Error!")
        return
    
    # 计算 (A @ B) + C
    result = matrix_add(AB, C)
    if result is None:
        print("Error!")
        return
    for row in result:
        print(' '.join(map(str, row)))

if __name__ == "__main__":
    main()
```
代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image.png)




### E19942: 二维矩阵上的卷积运算

matrices, http://cs101.openjudge.cn/pctbook/E19942/


思路：
用时15min
循环结构进行处理，直接模拟卷积算法即可。

代码：

```python
a1,b1,a2,b2=input().split()
a1,b1=int(a1),int(b1)
a2,b2=int(a2),int(b2)
m1=[]
for i in range(a1):
    m1.append(list(map(int,input().split())))
m2=[]
for i in range(a2):
    m2.append(list(map(int,input().split())))
result=[]
for i in range(a1-a2+1):
    s=[]
    for j in range(b1-b2+1):
        t=0
        for n in range(a2):
            for m in range(b2):
                t+=m1[i+n][j+m]*m2[n][m]
        s.append(t)
    result.append(s)
for i in range(a1-a2+1):
        for j in range(b1-b2+1):
            if j!=b1-b2:
                print(result[i][j],' ',end='',sep='')
            else:
                print(result[i][j],end='')
        if i!=a1-a2:
            print('')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-1.png)



### M06640: 倒排索引

data structures, http://cs101.openjudge.cn/pctbook/M06640/

思路：
用时15min
直接按照题目要求查找单词即可

代码：

```python
n=int(input())
s=[]
for i in range(n):
    s.append(list(input().split()))
m=int(input())
result=[]
for i in range(m):
    l=input()
    ans=[]
    for j in range(n):
        if l in s[j]:
            ans.append(j+1)
    if not ans:
        ans.append(0)
    result.append(ans)
for i in range(m):
    if result[i]==[0]:
        print('NOT FOUND')
    else:
        for j in result[i]:
            print(j,end=' ')
    print('')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image-2.png)




### E160.相交链表

two pinters, https://leetcode.cn/problems/intersection-of-two-linked-lists/

思路：
用时25min
使用双指针，两个指针同时向前，并在末尾转到另一个链表，可以计算长度得知俩指针将在链表相交处相会。

代码：

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        if not headA or not headB:
            return None
        p1, p2 = headA, headB
        while p1 != p2:
            p1 = p1.next if p1 else headB
            p2 = p2.next if p2 else headA
        return p1
            
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>


![alt text](image-3.png)


### E206.反转链表

three pinters, recursion, https://leetcode.cn/problems/reverse-linked-list/

思路：
用时25min，但这里去搜索了链表相关知识，加起来时间会更久。
选用另一个指针，存储反向的移动，再利用原链表的正向移动为其赋值。


代码

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        prev = None
        current = head
        while current:
            next_node = current.next  # 保存下一个节点
            current.next = prev       # 反转指针
            prev = current            # 移动prev
            current = next_node       # 移动current
        return prev
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-4.png)




### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/

思路：
用时四十分钟
该题是做作业以来最复杂的问题了，首先看上去是一个简单的遍历问题，我第一想法是dfs对八个方向进行暴力搜索，找到路径并返回，在这个过程中只要注意好边界控制和行列字母数字的坐标表达即可。在这里我提交之后又发现出现了WA，仔细审题才发现输出要求输出最小字典序结果。这里我第一反应想的过于简单，直接把steps按照字典序排序如何照样搜索，发现还是不对，这里意识到对于不同的结点steps对字典序的影响是不一样的，于是先将所有的moves列出来，再按照字典序排序后挨个搜索，由于深度搜索数它是以从左到右顺序搜索的，第一个返回的自然就是字典序最小的，即满足要求。

代码

```python
def journey(r0, l0, steps, r, l):
    if len(steps) == r * l:
        return ''.join(steps)
    possible_moves = []
    for i in range(8):
        new_r = r0 + step[i][0] 
        new_l = l0 + step[i][1]
        if 0 <= new_r < r and 0 <= new_l < l:
            position = chr(ord('A') + new_l) + str(new_r + 1)
            if position not in steps:
                possible_moves.append((position, new_r, new_l))
    possible_moves.sort(key=lambda x: x[0])
    for pos, new_r, new_l in possible_moves:
        steps.append(pos)
        result = journey(new_r, new_l, steps, r, l) 
        if result:  
            return result
        steps.pop()  
    return None 

step = [(-2, -1), (-2, 1), (-1, -2), (-1, 2),
        (1, -2), (1, 2), (2, -1), (2, 1)]

n = int(input())
for i in range(n):
    r, l = map(int, input().split())
    print('Scenario #', i+1, ':', sep='')
    start_position = 'A1'
    result = journey(0, 0, [start_position], r, l)
    if result:
        print(result)
    else:
        print("impossible")
    print()

```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-5.png)




## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>


本次作业还是主要意识到自己对指针和链表的内容不够熟悉，需要更多的学习。回溯与搜索是相对来说比较擅长的内容。双指针算法也是给了很多新的思考，对于处理两个链表之间的关系有了很多妙用。
最后一题的dfs几乎是基础dfs中最困难的那类了，对搜索条件，顺序，以及搜索数组的处理都做出了一定的要求。下一步得重新回忆dfs剪枝以及记忆化等手法，想必会是更困难题目的要点之一。

