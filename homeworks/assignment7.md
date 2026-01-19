# Assignment #7: bfs、🌲

Updated 0851 GMT+8 Oct 21, 2025

2025 fall, Complied by <mark>同学的姓名、院系</mark>





>**说明：**
>
>1. **解题与记录：**
>
>     对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
>2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的本人头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
> 
>3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。  
>
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### M23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555

要求用节省内存的方式实现，不能还原矩阵的方式实现。

思路：
将Y矩阵保存为其转置并且用Y矩阵的下标来表示在矩阵中的位置是有利于简化处理的关键。


代码：

```python
from sys import stdin
from collections import defaultdict
n, m1, m2 = map(int, stdin.readline().split())
list_x = []
for _ in range(m1):
    i, k, val = map(int, stdin.readline().split())
    list_x.append((i, k, val))
# 建立 Y 的按行索引: y_by_row[k] = list of (j, val)
y_by_row = defaultdict(list)
for _ in range(m2):
    k, j, val = map(int, stdin.readline().split())
    y_by_row[k].append((j, val)) 
# 累加结果
tempZ = defaultdict(lambda: defaultdict(int))
for ix, ik, xval in list_x:
    if ik in y_by_row:
        for j, yval in y_by_row[ik]:
            tempZ[ix][j] += xval * yval
# 收集非零结果并排序
result = []
for i in sorted(tempZ.keys()):
    for j in sorted(tempZ[i].keys()):
        if tempZ[i][j] != 0:
            result.append((i, j, tempZ[i][j]))
for i, j, val in result:
    print(i, j, val)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image.png)




### M102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/


思路：
用两个列表，一个保存节点，一个保存节点值。经典的bfs


代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        result = [[root.val]]  
        current_level_nodes = [root]  
        while current_level_nodes:
            next_level_nodes = []  
            next_level_values = []  
            for node in current_level_nodes:
                if node.left:
                    next_level_nodes.append(node.left)
                    next_level_values.append(node.left.val)
                if node.right:
                    next_level_nodes.append(node.right)
                    next_level_values.append(node.right.val)
            if next_level_values:  
                result.append(next_level_values)
            current_level_nodes = next_level_nodes   
        return result

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image-1.png)




### M131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

思路：
采用dfs，在搜索到第i位后向下搜索i位以后所有的进一步划分情况，直至将整个字符串划分完。这里使用记忆化可以有一些简化。
另一种方法是先检索出所有可能形成的字符串，存储好之后用dp搜索这些回文字符的拼接方法。


代码：

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        ans=[]
        def check(a):
            for i in range(len(a)//2):
                if a[i]!=a[-i-1]:
                    return False
            return True
        def path(string,result):
            if not string:
                ans.append(result[:])
                return 
            for i in range(1,len(string)+1):
                if check(string[0:i]):
                    result.append(string[0:i])
                    path(string[i:],result)
                    result.pop()
        path(s,[])
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image-2.png)



### M200.岛屿数量

dfs, bfs, https://leetcode.cn/problems/number-of-islands/ 

思路：
每找到一块陆地便以其为起始点进行深搜搜索出所有与其相连的陆地并且在原网格中进行标记以免重复搜索。
直至完全找不到未标记过的陆地。


代码

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        steps=[[-1,0],[1,0],[0,-1],[0,1]]
        def island(i,j,k):
            if grid[i][j]!='1':
                return 
            grid[i][j]=k
            for q in range(4):
                new_i=i+steps[q][0]
                new_j=j+steps[q][1]
                if 0<=new_i<m and 0<=new_j<n:
                    island(new_i,new_j,k)
        m=len(grid)
        n=len(grid[0])
        l=0
        for s in range(m):
            for t in range(n):
                if grid[s][t]=='1':
                    l+=1
                    island(s,t,l)
                    
        return l
```



<mark>（至少包含有"Accepted"）</mark>

![alt text](image-3.png)



### 1123.最深叶节点的最近公共祖先

dfs, https://leetcode.cn/problems/lowest-common-ancestor-of-deepest-leaves/

思路：
这题我一开始的想法是用bfs一直按照层数向下搜索找到最深的一层的所有叶子，再根据他们的位置一层层回溯查找他们的最近公共祖先，但是这种方法不仅繁琐而且时间复杂度高。
后续我参考一些想法后看到了这个类似于归并的思想，对于一个节点他的最深叶节点的最近公共祖先，要么在他的左子树上，要么在他的右子树上，再或者就是他自己，所以我们只需要对他的左右子树各求深度即可判断大致位置，加入确定在其左子树上再对其左子树进行同样的搜索进一步确定大致位置，如此将逐渐锁定位置。而在实际实现时我们是从下往上逐渐确定祖先位置的，我们找到左右子树的祖先再判定谁的祖先得以保留，这样一步步归并最后得到总的公共祖先。


代码

```python
class Solution:
    def lcaDeepestLeaves(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(node):
            if not node:
                return None,0
            left_lca, left_depth = dfs(node.left)
            right_lca, right_depth = dfs(node.right)
            if left_depth > right_depth:
                return left_lca, left_depth + 1
            elif right_depth > left_depth:
                return right_lca, right_depth + 1
            else:
                return node, left_depth + 1
        lca, _ = dfs(root)
        return lca
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-4.png)




### M79.单词搜索

回溯，https://leetcode.cn/problems/word-search/

思路：
dfs，注意经过的点需要标记，且需要尝试所有起点
代码：

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        steps = [[-1,0],[1,0],[0,-1],[0,1]]
        m = len(board)
        n = len(board[0])
        a = len(word)
        def find(i, j, k):
            nonlocal found
            if found:  
                return
            if board[i][j] != word[k]:
                return
            if k == a - 1:
                found = True
                return
            # 保存当前字符并标记为已访问
            temp = board[i][j]
            board[i][j] = '#'
            # 尝试四个方向
            for dx, dy in steps:
                new_i = i + dx
                new_j = j + dy
                if 0 <= new_i < m and 0 <= new_j < n and board[new_i][new_j] != '#':
                    find(new_i, new_j, k + 1)
                    if found:  
                        break
            # 回溯
            board[i][j] = temp
        found = False
        for i in range(m):
            for j in range(n):
                find(i, j, 0)
                if found:
                    return True
        return False
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image-5.png)


## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>
这次作业主要也还是加强了对树的理解，基本已经掌握一些常用技巧。二叉树这一结构非常方便用于dfs和bfs求解问题，应通过练习了解二叉树的深层逻辑，方便用于以后解决实际问题。广搜和深搜的具体应用也在不同背景下略有差异，仔细体会其中区别并会自己做出判断。




