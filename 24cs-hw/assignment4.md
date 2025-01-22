# Assignment #4: T-primes + 贪心

Updated 0337 GMT+8 Oct 15, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



思路：因为要取最大的可以赚钱的数量，所以先进行排序（因为<0的是获得金钱所以取ascending），而后一遍计算已经拿的数量，当大于最大可以拿取的数量就直接退出循环，否则增加已经拿的数量，然后计算可以获得的金钱（因为是负数所以用-），把carry放在if i < 0 前并且起始是1可以保证拿到的刚好=m（因为判断条件使用<=而非<）



代码

```python
n, m = map(int, input().split())
cost = sorted([int(x) for x in input().split()])
carry = 1
earn = 0
for i in cost:
    if carry <= m:
        carry += 1
        if i < 0:
            earn -= i
            continue
    else: 
        break
print (earn) 

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-19 132427.png"></center>




### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A

思路：与上面的类似，只是要取的从最大的开始，所以排序后使用pop返回的最大值作为得到的金币数量，因为考虑的是所获得的币值要更大，因此判断条件为获得的价值和剩余的总的价值（可改变）



代码

```python
n = int(input())
values = sorted(list(map(int, input().split())))
coins = 0
get = 0
while get <= sum(values):
    get += values.pop()
    coins += 1
else: 
    print (coins)

```



代码运行截图 ==（至少包含有"Accepted"）==

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-19 132949.png"></center>



### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B

思路：这题其实一开始并没有思路（应该说是不理解题目在说什么），先去问了助教之后写了一个既判断行列有没有元素又判断cost的代码，结果最后wa了，选择去看答案之后又自己画了一下，才知道原来只需要看最小cost的那一行/列的total cost，然后比较其中最小的便是minimum cost了，随后就写出来了以下代码然后成功AC



代码

```python
for t in range(int(input())):
    n = int(input())
    row = [int(x) for x in input().split()]
    column = [int(x) for x in input().split()]
    costr = 0
    costc = 0
    min_col = min(column)
    min_row = min(row)
    for i in range(n):
        costr += row[i] + min_col
    for i in range(n):
        costc += column[i] + min_row
    print(min(costr, costc))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-19 133817.png"></center>



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B

思路：这题其实和装箱问题比较类似，就是没有不同大小的人所以计算简单的许多，按照4 3都必须有一辆车，2需要2组一辆车，剩下的1来补充的想法很快就能把问题解决



代码

```python
n = int(input())
children = list(map(int, input().split()))
taxi = children.count(4) + children.count(3) + (children.count(2)//2)
if children.count(2) % 2 == 0:
    left = children.count(1) - children.count(3)
else:
    taxi += 1
    left = children.count(1) -children.count(3) - 2
 
if left > 0:
        taxi += left // 4
        if left % 4 != 0:
            taxi += 1
 
print(taxi)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-19 165949.png"></center>



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：答案是不会错的，就是一直超时，外加几次memory limit exceed，花了挺长时间在这一题上的，从群里说的筛法到后来各种直接判断，最好的一次是在test33超了，但是还有其他作业所以不能看了，等我把作业写完了有时间再看看（到目前为止用了大概10个小时在这题上）



代码

```python
#只能有1, sqrt(y), y三个因子
import math
def is_prime(i):
    if i < 2:
        return False
    if i in (2, 3):
        return True
    if i % 2 == 0:
        return False
    sqrt = math.isqrt(i) + 2
    primes = [True]*(sqrt)
    primes[0] = primes[1] = False
    
    for j in range(3, sqrt, 2):
        if not primes[j]:
            continue
        elif primes[j] and i % j == 0:
            return False
        elif primes[j] and i % j != 0:
            for k in range(i, sqrt, j):
                primes[k] = False 
    return True

def get_count(y):
    if y < 4:
        return 0
    
    if y % 2 == 0:
        return 3 if y < 5 else 0
    
    if is_prime(y):
        return 0
    
    limit = math.isqrt(y)

    if limit * limit == y:
        if is_prime(limit):
            return 3
        else:
            return 0
    else:
        return 0

n = int(input())
x = list(map(int, input().split()))
for i in range(n):
    y = x[i]
    if get_count(y) == 3:
        print("YES")
    else:
        print("NO")

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559

思路：没做，有空再写



代码

```python


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

因为最近是真的忙所以计概几乎没在看，还好作业除了选做花的时间并不多，等到下一周忙完了再开始看计概



