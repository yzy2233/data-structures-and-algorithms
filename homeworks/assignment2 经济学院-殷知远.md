## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/

思路：

简单题，斐波那契数列的非常直白的衍生，在数组上用一层循环即可处理，本题只要求处理一组数据，若多组数据可用记忆化方法递归节约时间

代码：

```python
n=int(input())
T=[0,1,1]
for i in range(3,n+1):
    T.append(T[i-3]+T[i-2]+T[i-1])
print(T[n])


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![alt text](image.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A


思路：
简单题，在字符串中按顺序查找hello即可，与其说是greedy不如说是最初始的想法，比较直白
与这道题相比更有收获的可能是第一次使用codeforces，感觉这是一个非常全面的网站

代码：

```python
s=input()
a='hello'
t=0
for i in s:
    if i==a[t]:
        t+=1
    if t==5:
        break
if t==5:
    print('YES')
else:
    print('NO')

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](<屏幕截图 2025-10-07 194907.png>)




### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A

思路：
简单题，主要考察的是字符串的处理，使用lower()函数处理大小写问题，然后用replace完成要求即可


代码：

```python
s=input()
s=s.lower()
a=["a","e","o","y","u","i"]
for i in a:
    s=s.replace(i,'')
for i in s:
    print('.',i,sep='',end='')

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](<屏幕截图 2025-10-07 200524.png>)




### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/

思路：
经典问题哥德巴赫猜想，我这里采用自己写一个prime判定函数，然后对比n的一半还小的数一个个进行尝试。其实直接爆搜写出范围内素数表或许时间上更优，因为这里的范围并不大。同时我这里的素数判定也可用更优，只需要对该数的平方根以下的数进行因子判断即可。（但是题目要求不高我就暂时没写了，只需要对循环范围改一下即可）


代码：

```python
n=int(input())
def primeif(a):
    for i in range(2,a):
        if a%i==0:
            return 0
    return 1
for i in range(2,n//2):
    if primeif(i) and primeif(n-i):
        print(i,n-i)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![alt text](image-1.png)




### E23563: 多项式时间复杂度

http://cs101.openjudge.cn/pctbook/E23563/

思路：
字符串处理题目，第一反应是直接查找^字符然后对两侧进行解读，后续发现并不方便，遂直接用正则表达式暴力处理，效果也是不错的，这里要注意可能有0系数，非常无聊的坑点，不过也不费事。


代码

```python
import re
s = input()
exponents = re.findall(r'(?<!0)n\^(\d+)', s)
if exponents:
    maxx = max(map(int, exponents))
    print(f"n^{maxx}")
else:
    print("n^0")
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-2.png)




### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/

思路：
计数问题，比较基础，甚至在很多题目中都能看到该问题只是一个初始处理数据的步骤，也算泛用性很强了。我这里直接用字典进行计数，然后找到最大值排序输出key即可。


代码

```python
a=list(map(int,input().split()))
result={}
maxx=0
for i in a:
    if i not in result:
        result[i]=1
        maxx=max(maxx,result[i])
    else:
        result[i]+=1
        maxx=max(maxx,result[i])
t=[]
for key in result.keys():
    if result[key]==maxx:
        t.append(key)
t.sort()
for i in t:
    print(i,end=' ')
```



<mark>（至少包含有"Accepted"）</mark>
![alt text](image-3.png)




## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>
这次作业确实比较基础，自己也是额外找了些题目，在openjudge上找了NOI小组里的一些习题，主要做了一些循环控制，不乏“金币”这种经典竞赛题，有效的复习。
![alt text](image-5.png)
![alt text](image-4.png)