# Assignment #5: Greedy穷举Implementation

Updated 1939 GMT+8 Oct 21, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148

思路：这题卡了挺久的，一直看来看去没发现问题，最后看了其他人的代码才发现原来是输入搞错了，一开始写的是d, p, e, i，所以一直出错误的答案，改正之后就accepted了（哦对然后还发现范围是d+21252不是单纯21252）；思路就是最小公约数（是这么叫吗），就是这一天同时可以被23，28和33整除，然后因为peak_day发生在不同的天，就是“开始”的时间不一样，所以需要减掉这个差异



代码：

```python
# Only one day will get a peak in 21252
n = 0
while True:
    p, e, i, d = map(int, input().split())
    n += 1
    if (d and p and e and i) == -1:
        break

    while p > 23:
        p -= 23
    while e > 28:
        e -= 28
    while i > 33:
        i -= 33

    next_peak = d + 1
    while True:
        if (next_peak - p) % 23 == 0 and (next_peak - e) % 28 == 0 and (next_peak - i) % 33 == 0:
            print (f"Case {n}: the next triple peak occurs in {next_peak - d} days.")
            break
        next_peak += 1
        if next_peak > d + 21252:
            print("wrong")
            break

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src="C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-27 165717.png">



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211

思路：这道题小小卡，但是后来就果断转变成用双指针来完成了，然后WA了两次，最后改成了独立判断i,j指向同一个图纸时的买卖情况就AC了



代码：

```python
p = int(input())
papers = sorted(list(map(int, input().split())))
mine = 0
enemy = 0
i = 0
j = -1
while i - len(papers) < j:
    if p >= papers[i]:
        p -= papers[i]
        mine += 1
        i += 1
    else:
        if mine > enemy:
            enemy += 1
            p += papers[j]
            j -= 1
        else:
            break

if i - len(papers) == j and p >= papers[i]:
    mine += 1

print (mine - enemy)

```



代码运行截图 ==（至少包含有"Accepted"）==

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-27 173603.png">



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554

思路：这题一次AC，通过输入输入顺序的编号来得知他是第几个发邮件的，然后按照由少到多的实验时间使所有人等待时间最少，然后从列表里提取出需要的元素就行（PS：一开始还以为是要依据原来的顺序在对应的学生的index位置上写上做实验的顺序，后来在检查get_order排列是否正确的时候发现其实是直接打印发邮件的顺序，幸运的减少了一次WA）



代码：

```python
n = int(input())
get_time = list(map(int, input().split()))
get_order = []
for i in range(len(get_time)):
    get_order.append([i+1, get_time[i]])
get_order.sort(key = lambda x: x[1])

temp_waiting = 0
waiting = 0
for i in range(len(get_time)):
    waiting += temp_waiting
    temp_waiting += get_order[i][1]
waiting /= n

print(*(get_order[i][0] for i in range(n)))
print(f"{waiting:.2f}") 

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-27 180509.png">



### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/

思路：小略



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C

思路：还是比较习惯用greedy的方法，只要向左倒、向右倒有其中一个不碰到边界就更新条件，独立判断第一个和最后一个，第一个总是向左，如果有大于1棵树，则最后一个总是向右倒；DP的话是先去看了题目的提示才知道要怎么办的，确实是对dp非常不熟悉，然后中间卡了一下稍微手写列出来之后发现存在特例情况，稍微列了一下if else之后就accepted了，把数据列出来然后跟随自己的代码慢慢用人脑推的感觉真的是好慢，看了半个小时才把输入看完然后发现错处（早知道直接开debugger了，虽然应该也不会快到哪里去，又是感叹计算机真的有必要存在的一天）



代码：

```python
#greedy
n = int(input())
trees = []
for _ in range(n):
    c, h = map(int, input().split())
    trees.append([c, h])

last_position = trees[0][0]
chopped = 1
for i in range(1, n-1):
    c, h = trees[i] # get data from list
    left = c - h
    middle = c
    right =  c + h

    if middle <= last_position: # seems like not passible to happen
        continue
    elif left > last_position: # tree can fall to left
        last_position = middle
        chopped += 1
    elif middle > last_position and right < trees[i+1][0]: # check if tree can fall to right (except the last tree can always fall to right)
        last_position = right
        chopped += 1
    else: # since can't fall, but a block to reach more left
        last_position = c

if n > 1: # cannot simply set chopped = 2
    chopped += 1

print(chopped)

```

```python
# DP
n = int(input())
trees = []
for _ in range(n):
    c, h = map(int, input().split())
    trees.append([c, h])

if n == 1: # if only have one tree :)
    print(1)
    exit() # break version without while, not recommend in big program, can use sys.exit() to get an error code

# first tree, can always fall
stay = 0
left = 1
right = 1

for i in range(1, n):
    maxi = max(stay, left, right)
    stay = maxi # when i-th stay

    if right == maxi and left != maxi:
        if trees[i][0] - trees[i][1] > trees[i-1][0] + trees[i-1][1]: # special situation, previous one fall right and overlap with the present tree falling left
            left = maxi + 1
        else:
            left = maxi
    else: 
        if trees[i][0] - trees[i][1] > trees[i-1][0]: # can fall left
            left = maxi + 1
        else:
            left = maxi

    if i == n - 1 or (trees[i][0] + trees[i][1]) < trees[i+1][0]: # i-th fall right / the last (to prevent an error like above done bfr)
        right = maxi + 1
    else:
        right = maxi

print(max(stay, left, right))


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-27 235427.png">
<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-28 004435.png">



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/

思路：小小略



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

题目还是有一定难度的，花了一天的时间解决了4题，还有两个报告要赶，没什么时间花在计概上，非常抱歉T^T，等考完了期中考就应该没事了



