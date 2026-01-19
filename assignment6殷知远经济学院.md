## 1. 题目

### E24588: 后序表达式求值

Stack, http://cs101.openjudge.cn/practice/24588/

思路：
我们在每遇到一个操作符时便将该操作符前的两个操作数弹出进行运算，因为要用最晚进入列表的数进行运算，我们会使用栈来对操作数进行存储以便完成后进后出的操作。


代码：

```python
n = int(input())
operator = ['+', '-', '*', '/']
def op(p, q, t):
    if t == '+':
        return p + q
    if t == '-':
        return p - q
    if t == '*':
        return p * q
    if t == '/':
        return p / q
for i in range(n):
    l = list(input().split())
    stack = []  
    for s in l:
        if s in operator:
            # 弹出两个操作数
            if len(stack) < 2:
                # 处理错误情况
                break
            b = stack.pop()
            a = stack.pop()
            result = op(a, b, s)
            stack.append(result)
        else:
            # 数字入栈
            stack.append(float(s))
    print(f"{stack[0]:.2f}")
 
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image.png)




### M234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

<mark>请用快慢指针实现</mark> `O(1)` 空间复杂度。


思路：
结合之前所做的一道反转链表题，我们想到回文链表会与其反转链表相同，但又希望用快慢指针实现，则使用速度快一倍的快指针来找到中点，然后比较前半链表和反转的后半链表。


代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        #快慢指针找链表中点
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        prev = None
        while slow:
            next_node = slow.next
            slow.next = prev
            prev = slow
            slow = next_node
        left, right = head, prev
        while right:  
            if left.val != right.val:
                return False
            left = left.next
            right = right.next
        
        return True
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image-1.png)




### M27217: 有多少种合法的出栈顺序

http://cs101.openjudge.cn/practice/27217/

思路：
我首先是尝试直接模拟出栈过程，发现时间复杂度到了0(n^3),超时了，加了记忆化搜索的方式也仍然不行

另一种想法就比较简单，这是一道看起来就像能解出通解的数学题。我们注意到对于任意一点，push数一定大于等于pop数，假设在某个点栈空了，则该点前与该点后均为该问题的子问题，只是
问题的规模变小了，这里就是经典的卡特兰数的递推，所以我们得到了答案的通解。
代码：

```python
n=int(input())
C = 1
for i in range(n):
    C = C * (2 * (2 * i + 1)) // (i + 2)
print(C)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-2.png)



### M24591:中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/

思路：
我们主要考虑的问题是如何将压入栈的符号按某种顺序吐出。首先是关于括号的处理，在读取到右括号时要将左右括号之间的运算符按某种顺序吐出。第二点就是吐出一系列不含括号的运算符序列时，应该按照优先级进行识别后吐出，顺序应该是在识别到某运算符是将其之前所有优先级大于等于其的吐出。


代码

```python
n=int(input())
op=['+','-','*','/','(',')']
for i in range(n):
    s=input()
    stack_op=[]
    num=[]
    t=0
    ans=[]
    for j in range(len(s)):
        if s[j] in op:
            if j>t:
                ans.append(s[t:j])
            t=j+1
            if s[j]=='(':
                stack_op.append(s[j])
            elif s[j]==')':
                while stack_op[-1]!='(':
                    ans.append(stack_op.pop())
                stack_op.pop()
            else:
                if s[j] in op[2:4]:
                    while stack_op and stack_op[-1] in op[2:4]:
                        ans.append(stack_op.pop())
                    stack_op.append(s[j])
                if s[j] in op[0:2]:
                    while stack_op and stack_op[-1] in op[0:4] :
                        ans.append(stack_op.pop())
                    stack_op.append(s[j])
    if t!=len(s):
        ans.append(s[t:])
    while stack_op:
        ans.append(stack_op.pop())
    print(' '.join(ans))
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-3.png)




### M02299:Ultra-QuickSort

merge sort, http://cs101.openjudge.cn/practice/02299/

思路：
看到这道题第一反应肯定是冒泡排序，相邻两两进行交换的方式完全符合冒泡排序的原理。但是用冒泡排序将显著超时。第二想法是想到所谓交换次数，其实在相邻两数交换时有个经典结论是逆序数在交换至最后时归零，且正好等于交换次数，又想到找一个更省时间且稳定的排序，则使用归并排序，并在实行归并排序的时候记录交换次数，其实也就是逆序数。


代码

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr, 0
    
    # 分解
    mid = len(arr) // 2
    left, count_left = merge_sort(arr[:mid])
    right, count_right = merge_sort(arr[mid:])
    
    # 合并
    merged, count_merge = merge(left, right)
    total_count = count_left + count_right + count_merge
    return merged, total_count

def merge(left, right):
    result = []
    i = j = 0
    count = 0
    
    # 比较两个数组的元素，按顺序合并
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
            count += len(left) - i  # 计算逆序对
    
    # 添加剩余元素
    result.extend(left[i:])
    result.extend(right[j:])
    return result, count
while 1:
    n=int(input())
    if n==0:
        break
    arr=[]
    s=0
    for j in range(n):
        arr.append(int(input()))
    r,ans=merge_sort(arr)
    print(ans)
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-4.png)




### M146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

思路：
通过一个队列来实现最久没调用的元素的删除，使用其先进先出的概念。同时在调用某个元素时将其从队列里删除并插入队列的最后以维护队列的性质。



代码：

```python
from collections import deque

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {} 
        self.order = deque() 

    def get(self, key: int) -> int:
        if key in self.cache:
            # 将访问的键移到队列末尾
            self.order.remove(key)  
            self.order.append(key)  
            return self.cache[key]
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache[key] = value
            self.get(key)  # 利用get方法中的顺序调整逻辑
        else:
            if len(self.order) == self.capacity:
                lru_key = self.order.popleft()
                del self.cache[lru_key]
            self.cache[key] = value
            self.order.append(key)

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-5.png)

## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

这周主要在力扣上找了一些数据结构的题目进行练习，包括链表，栈，树，以加强练习上课所包含的内容。现在对于这些数据结构使用的一些技巧和方法都有了基本的掌握，能够基本识别出应该使用的合适的数据结构。