## 1. 题目

### E29952: 咒语序列

Stack, http://cs101.openjudge.cn/practice/29952/

思路：
在考试中的想法就是迁移之前做到过的有个括号匹配问题，所以很自然是使用栈进行匹配，主要要点在于匹配成功时将栈底先pop。但本题的主要难度在于在即使无法全部配对的情况下需判断最大匹配长度。我主要遇到的问题是如何在确认部分匹配时判断当前匹配的长度。如果我认为栈清空时可以改写最大长度则对于样例（（）会出现判断失误，因为永不为空；而如果我一匹配到一次左右括号为信号的话就会将之前已经匹配好的与中段未匹配的无法区分，例如样例'(())(()'。
解决方案是不在选择记录目前已经连续匹配了多少个括号，因为这时无法清楚的更新已配对的这些括号是否能和后面连上。而是选择记录能匹配上次匹配上的左括号的坐标，这样可以在同样的空间内记录上位置信息，同时便于判断和谐序列的长度。


代码：

```python
s=input()
l={')':'(',']':'[','}':'{'}
a=[-1]  # 栈底放一个-1作为起始位置
maxx=0
for i in range(len(s)):
    if s[i] in l.values():
        a.append(i)
    else:
        if len(a) > 1 and s[a[-1]] == l[s[i]]:  
            a.pop()
            maxx = max(maxx, i - a[-1])
        else:
            a.append(i)
print(maxx)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image.png)



### M01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/


思路：
这是一道典型的求最值的贪心题目，我们很自然的想法就是求出每个岛屿可以放置的雷达的横坐标的区间。对于最少的雷达放置数量，我的想法是将所有放置区间按右边界排列，以此从小到大遍历右边界，直到找到一个右侧边界已经大于等于了所有的左边界，此时该右边界的序号即为所求。
细节：
1.判断岛屿是否能被覆盖
2.多组数据样例的同时处理时的输入输出问题
3.样例组之间的空行如何读取

代码：

```python
import math

t = 0
while True:
    t += 1
    n, d = map(int, input().split())
    if n == 0 and d == 0:
        break
    
    islands = []
    for _ in range(n):
        x, y = map(int, input().split())
        islands.append((x, y))
    impossible = False
    for x, y in islands:
        if y > d or y < 0 or d < 0:
            impossible = True
            break
    if impossible:
        print(f"Case {t}: -1")
        try:
            input()  
        except:
            pass
        continue
    # 计算每个岛屿对应的雷达可放置区间
    intervals = []
    for x, y in islands:
        dx = math.sqrt(d * d - y * y)
        left = x - dx
        right = x + dx
        intervals.append((left, right))
    
    # 按右端点排序
    intervals.sort(key=lambda x: x[1])
    # 贪心选择雷达位置
    count = 0
    current_radar = -float('inf')
    for left, right in intervals:
        if left > current_radar:
            count += 1
            current_radar = right
    print(f"Case {t}: {count}")
    # 尝试读取空行
    try:
        line = input().strip()
        if line == "":
            continue
    except:
        pass
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image-1.png)




### M02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/

思路：
最最经典的dfs问题之一，这里题目要求按照组成的数字顺序查找解法，其实就是要求按照从左到右的顺序查找每一行的解法，这样解构成的八位数就会按照从小到大的顺序。
细节：
1.使用全局变量result存储答案
2.处理好是否合法的解法时的边界判定问题
3.如何将一串列表的一个个数字变为一个按位排列的整数

代码：

```python
result=[]
def queens(l):
    global result
    if len(l)==8:
        ans=0
        for t in l:
            ans=ans*10+t
        result.append(ans)
        return
    else:
        for i in range(1,9):
            if i in l:
                continue
            elif l==[]:
                l.append(i)
                queens(l)
                l.pop()
            else:
                si=1
                for j in range(len(l)):
                    if  abs(i-l[j])==abs(len(l)-j):
                        si=0
                        break
                if si==1:
                    l.append(i)
                    queens(l)
                    l.pop()
    return
n=int(input())
queens([])
for i in range(n):
    m=int(input())
    print(result[m-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-2.png)



### M25570: 洋葱

matrices, http://cs101.openjudge.cn/practice/25570/

思路：
本题主要考察矩阵数据的提取，我这里采用的是比较愚笨的办法，直接从外到内为洋葱的每一层命序号，按照序号规律一行一行判断该行哪些位置属于该层。


代码：

```python
def summ(ma,t,q):
    s=0
    for i in range(q,t-q):
        if i==q or i==t-1-q:
            for j in range(q,len(ma[i])-q):
                s+=ma[i][j]
        else:
            s+=ma[i][q]
            s+=ma[i][-1-q]
    return s
n=int(input())
mat=[]
sum=0
for i in range(n):
    l=list(map(int,input().split()))
    mat.append(l)
if n%2==1:
    for i in range((n-1)//2):
        #print(summ(mat,n,i))
        sum=max(summ(mat,n,i),sum)
    sum=max(sum,mat[(n+1)//2-1][(n+1)//2-1])
else:
    for i in range(n//2):
        sum=max(summ(mat,n,i),sum)
        #print(summ(mat, n, i))
print(sum)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-3.png)





### M29954: 逃离紫罗兰监狱

bfs, http://cs101.openjudge.cn/practice/29954/

思路：
对于最短路径问题最常用的就是bfs方法。我的理解就是bfs像石头掉进水里泛起的一圈圈涟漪，一层一层的遍历这些涟漪，记录每个点的状态，并且存储好为下一层树搜索提供起始状态。
我所认为的bfs方法的精要在于x, y, flash, steps = queue.popleft()这一语句，实现了传递不同层级之间的状态，并且用popleft（）的队列方法既节约了空间又保证了不重不漏，按顺序遍历每一层。这样只需要一层队列即可实现。

代码

```python
from collections import deque
def path():
    queue = deque()
    start_state = (start[0], start[1], t, 0)  
    visited[start[0]][start[1]][t] = True
    queue.append(start_state)
    
    while queue:
        x, y, flash, steps = queue.popleft()
        # 到达终点
        if (x, y) == (end[0], end[1]):
            return steps
        # 遍历四个方向
        for dx, dy in [(0,1),(0,-1),(1,0),(-1,0)]:
            nx, ny = x + dx, y + dy
            if not (0 <= nx < r and 0 <= ny < c):
                continue
            # 情况1：普通空地或终点
            if m[nx][ny] in ['.', 'E']:
                if not visited[nx][ny][flash]:
                    visited[nx][ny][flash] = True
                    queue.append((nx, ny, flash, steps + 1))
                    
            # 情况2：需要闪光的障碍
            elif m[nx][ny] == '#':
                if flash > 0 and not visited[nx][ny][flash-1]:
                    visited[nx][ny][flash-1] = True
                    queue.append((nx, ny, flash-1, steps + 1))
    
    return -1  # 无法到达
r,c,t=map(int,input().split())
steps=[[-1,0],[1,0],[0,-1],[0,1]]
m=[]
start=[]
end=[]
result=r*c+1
visited = [[[False] * (t+1) for _ in range(c)] for _ in range(r)]
for i in range(r):
    s=list(input())
    m.append(s)
    if 'E' in s:
        end=[i,s.index('E')]
    if 'S' in s:
        start=[i,s.index('S')]
m[start[0]][start[1]]='*'
print(path())
```



<mark>（至少包含有"Accepted"）</mark>


![alt text](image-4.png)


### T27256: 当前队列中位数

backtracking, http://cs101.openjudge.cn/practice/27256/

思路：
考试时由于时间有限我只尝试了最暴力的方法，以下给出的堆方法也是经过查找题解才逐渐理解，对堆这一数据结构有了较为清晰的认知。
以下解法为双堆法：
我们的做法是想要维护两个堆，两个堆分别放置大的那一半的数和小的那一半的数，最后去中位数时就不需要遍历查询而是直接考虑两个堆的堆顶进行求取。
第一个技术是，python只提供了最小堆的维护函数heapq，所以我们将较小的那一部分的堆的数全部取相反数，让最大的数反而最小到堆顶位置，这样就实现了最大堆。
第二个技术在于维护两个堆数量之间的关系，以保证中位数在堆顶附近。
第三个技术在于延迟删除，因为我们不能按照给出的操作顺序进行删除，因为我们希望节约时间只从堆顶进行删除，所以我们在每次加入新元素时对堆进行修正，删除待删除的元素
代码

```python
import heapq
from collections import defaultdict

class MedianFinder:
    def __init__(self):
        self.max_heap = []  # 较小的一半（最大堆，存储负数）
        self.min_heap = []  # 较大的一半（最小堆）
        self.delayed = defaultdict(int)  # 延迟删除记录
        self.size1 = 0  # max_heap实际大小
        self.size2 = 0  # min_heap实际大小
        self.queue = []  # 按添加顺序存储
        
    def add(self, num):
        self.queue.append(num)
        
        if not self.max_heap or num <= -self.max_heap[0]:
            heapq.heappush(self.max_heap, -num)
            self.size1 += 1
        else:
            heapq.heappush(self.min_heap, num)
            self.size2 += 1
            
        self._balance()
        
    def delete_first(self):
        if not self.queue:
            return
            
        num_to_delete = self.queue.pop(0)
        self.delayed[num_to_delete] += 1
        
        # 更新实际大小
        if self.max_heap and num_to_delete <= -self.max_heap[0]:
            self.size1 -= 1
        else:
            self.size2 -= 1
            
        # 清理堆顶
        self._prune(self.max_heap)
        self._prune(self.min_heap)
        self._balance()
        
    def query(self):
        # 清理堆顶
        self._prune(self.max_heap)
        self._prune(self.min_heap)
        
        if self.size1 > self.size2:
            result = -self.max_heap[0]
        else:
            result = (-self.max_heap[0] + self.min_heap[0]) / 2
        
        # 输出格式处理
        if isinstance(result, float) and result.is_integer():
            return int(result)
        return result
        
    def _balance(self):
        # 平衡两个堆的大小
        while self.size1 > self.size2 + 1:
            num = -heapq.heappop(self.max_heap)
            # 检查是否被延迟删除
            while num in self.delayed and self.delayed[num] > 0:
                self.delayed[num] -= 1
                if self.delayed[num] == 0:
                    del self.delayed[num]
                if not self.max_heap:
                    return
                num = -heapq.heappop(self.max_heap)
                
            heapq.heappush(self.min_heap, num)
            self.size1 -= 1
            self.size2 += 1
            
        while self.size2 > self.size1:
            num = heapq.heappop(self.min_heap)
            # 检查是否被延迟删除
            while num in self.delayed and self.delayed[num] > 0:
                self.delayed[num] -= 1
                if self.delayed[num] == 0:
                    del self.delayed[num]
                if not self.min_heap:
                    return
                num = heapq.heappop(self.min_heap)
                
            heapq.heappush(self.max_heap, -num)
            self.size2 -= 1
            self.size1 += 1
            
    def _prune(self, heap):
        # 清理堆顶的延迟删除元素
        while heap:
            if heap is self.max_heap:
                num = -heap[0]
            else:
                num = heap[0]
                
            if num in self.delayed and self.delayed[num] > 0:
                heapq.heappop(heap)
                self.delayed[num] -= 1
                if self.delayed[num] == 0:
                    del self.delayed[num]
            else:
                break

def main():
    import sys
    input = sys.stdin.read().splitlines()
    n = int(input[0].strip())
    finder = MedianFinder()
    
    for i in range(1, n + 1):
        operation = input[i].strip()
        
        if operation.startswith('add'):
            num = int(operation[4:])
            finder.add(num)
            
        elif operation == 'del':
            finder.delete_first()
            
        elif operation == 'query':
            result = finder.query()
            print(result)

if __name__ == "__main__":
    main()


<mark>（至少包含有"Accepted"）</mark>

![alt text](image-5.png)



## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>
由于之前学习的是C++,计算概论则是因为院系要求只修了相对简单的C级课程，面对这次月考可谓敲响了警钟。考试时经常会存在对于某一步骤有正确的想法但是不熟悉语法导致书写代码的困难，亦或是对于数据结构如列表，队列的不熟悉导致连一些基础操作都不能调用。
另一方面就是感觉自己的一次性代码的形成能力很差，在形成了思路并完成了初步代码之后，我发现我总是对于边界情况和极端情况考虑欠妥，经常存在条件语句的判定不完备或者经不起推敲，亦或是思路存在漏洞，这让我痛定思痛一定得想办法解决这些问题，一定要强化训练。
再回顾这次考试的几个重要的知识点，bfs的实现明显存在问题，数据结构也掌握的很差劲，需要重点克服，简单题不能快速的一次通过，多数在于语法的不熟悉和oython思路的欠考虑，输入与输出格式甚至都会为难半天，感到自身的极度不足，决心加强练习。