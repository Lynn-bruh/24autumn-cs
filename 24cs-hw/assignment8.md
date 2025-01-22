# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周⻓

matices, http://cs101.openjudge.cn/practice/12558/ 

思路：久违的一次提交即成功T^T，学会了matrices的题目可以通过加保护圈的方式来做，很快就通过了，思路就是因为岛屿必然连接，并且没有特殊情况，所以只需要判断有岛屿的地方上下左右是不是海（0），然后只要是边界边长就+1即可完成



代码：

```python
n, m = map(int, input().split())
islands = [[0 for _ in range(m+2)]]
for _ in range(n):
    islands.append([0] + list(map(int, input().split())) + [0])
islands.append([0 for _ in range(m+2)])

length = 0
for i in range(n+2):
    for j in range(m+2): # since all land were connected, only four direction need to judge
        if islands[i][j] == 1 and islands[i-1][j] == 0:
            length += 1
        if  islands[i][j] == 1 and islands[i+1][j] == 0:
            length += 1
        if islands[i][j] == 1 and islands[i][j-1] == 0:
            length += 1
        if islands[i][j] == 1 and islands[i][j+1] == 0:
            length += 1

print(length)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-18 235945.png">



### LeetCode54.螺旋矩阵

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106

思路：这题算是有想法，但是不知道怎么实现，就是spiral_matrix的数字增加的顺序必然是左到右，上到下，右到左，下到上完成一圈，然后再往更里边的一圈开始再完成下一圈直到最中心，可是不知道怎么把“圈”缩小，然后去看了一下leetcode的题解，发现可以用四个index来解决我的问题（一开始只用了两个，但是这样不能记住另外两个方向到哪里了，所以怎么都搞不出来），然后很顺利就做出来了



代码：

```python
import math
n = int(input())
ele = 1
left, right, top, bottom = 0, n-1, 0, n-1 # left index, right index
spiral_matrix = [[0]*(n) for _ in range(n)]
while spiral_matrix:
    for i in range(left, right+1): # from left to right (starting from first row, thn copying another row)
        spiral_matrix[top][i] = ele
        ele += 1
    top += 1
    if top > bottom: # complete all rows
        break
    for i in range(top, bottom+1): # from top to bottom (only the right half column need to copy from top to bottom)
        spiral_matrix[i][right] = ele
        ele += 1
    right -= 1
    if left > right: 
        break
    for i in range(right, left-1, -1): # from right to left (left index need to be smaller than left, so that fully copied since range stop at stop-1)
        spiral_matrix[bottom][i] = ele
        ele += 1
    bottom -= 1
    if top > bottom: 
        break
    for i in range(bottom, top-1, -1): # from bottom to top
        spiral_matrix[i][left] = ele
        ele += 1
    left += 1
    if left > right: 
        break

if spiral_matrix:
    for i in range(n):
        print(*spiral_matrix[i])

```



代码运行截图 ==（至少包含有"Accepted"）==

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-19 220136.png">



### 04133:垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/

思路：这个有点思路，而且在课件里也有看过，但是写作业的时间有点紧，matrices的题目会耗费比较多的时间（主要卡在实现上），之后再写



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### LeetCode376.摆动序列

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/

思路：上半部分就是统计摆动的情况，下半部分计算摆动的次数，最短的子序列必然含有第一个元素，然后开始计算第一到第二、第二到第三...的情况，一开始设的是当前不能等于前一个，后来发现可能存在1,0,1的情况，然后发现只要前一个上后一个下，或者前一个下后一个上，二者相乘必然等于-1，然后又发现这个时候第一个的会被忽略因为初始的pre设为0，所以多加了一个判断条件，然后在里边设置只有遇到不等于0的时候才需要更改前一个的记录，不然一直保留直到遇到下一个进行比较



代码：

```python
n = int(input())
seq = list(map(int, input().split()))
updown = []
for i in range(1, n):
    if seq[i] > seq[i-1]: # latter larger than former, up
        updown.append(1)
    elif seq[i] < seq[i-1]: # latter smaller than former, down
        updown.append(-1)
    elif seq[i] == seq[i-1]: # not change
        updown.append(0)

longest = 1
pre = 0 # default not change
for j in range(len(updown)):
    if updown[j]*pre == -1 or (pre == 0 and updown[j] != 0): # either the first one or the others
        longest += 1
        if updown[j] != 0:
            pre = updown[j]

print(longest)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-19 223105.png">



### CF455A: Boredom

dp, 1500, https://codeforces.com/contest/455/problem/A

思路：这题可以说是靠着cf的tutorial完成，因为tutorial里面四舍五入其实就已经把题解写出来了，首先就是计算sequence里面每个integers分别含有多少个，因为题目给予的条件是integer不能相邻而不是在原来的sequence中不能相邻（就是这里卡了一会，之后把题目又重新看了一遍才发现错误的地方），然后通过决定要不要take（max()，因为要求取得的integer不能相邻，不要拿就是继承前一个的value，拿就是继承前两个的已经计算的值加上当前这一个的integers*它的数量的总value）改变当前index对应的integer所可以积累的最大value，因为最终都会继承到最后一个上，所以只需要print最后一个value就行



代码：

```python
n = int(input())
sequence = list(map(int, input().split()))
Max = 10**5 + 1
freq = [0]*Max
dp = [0]*Max
 
for num in sequence:
    freq[num] += 1 # How many we got in every value

for i in range(1, Max):
    dp[i] = max(dp[i-1], dp[i-2] + i*freq[i]) # take or not take the value

print(dp[Max-1]) # largest index = length-1

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-19 230439.png">



### 02287: Tian Ji -- The Horse Racing

greedy, dfs http://cs101.openjudge.cn/practice/02287

思路：来不及了，私下里慢慢想



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

重新开始慢慢跟进每日选做中，感受到确实是落后了很多，除了标准的题目可以一眼看出解法之外剩余的都需要很长时间的思考和试错，现在就是能有一个大框架的想法，但是具体怎么实现却经常想不出来，需要提示才能完整写出，接下来还需要尽量抽时间来补充更多代码的知识（比如lru_cache, deque, 还有matrices, dp的题目）



