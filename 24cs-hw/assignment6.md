# Assignment #6: Recursion and DP

Updated 2201 GMT+8 Oct 29, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### sy119: 汉诺塔

recursion, https://sunnywhy.com/sfbj/4/3/119  

思路：这题其实之前看过了，主要思路就是当只有一个直接移动到目标的柱（确定是C，也确定从A开始），如果多于一个就把它拆成顶上n-1和底下1（或者也可以说是顶上1和底下n-1，顺序都是一样的），先处理顶上n-1的移动（会慢慢call到最后只剩下2个），使得所有的plate移动到不用的柱上，然后把底下1移动到另一个空的柱子，再把顶上的plate全部移到底下的plate上，然后又空出来了一个柱子，可以继续移动（但是这个function其实只靠着移动2个plate就能出来）



代码：

```python
saving_data = []
def hanoi(plate, previous_tower, current_tower, not_used):
    if plate == 1:
        saving_data.append([previous_tower, current_tower])        
    else:
        hanoi (plate-1, previous_tower, not_used, current_tower)
        hanoi (1, previous_tower, current_tower, not_used)
        hanoi (plate-1, not_used, current_tower, previous_tower)

plate = int(input())
hanoi(plate, "A", "C", "B")

print(len(saving_data))
for i in range(len(saving_data)):
    pre = saving_data[i][0]
    cur = saving_data[i][1]
    print ("{}->{}".format(pre, cur))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-05 001039.png">



### sy132: 全排列I

recursion, https://sunnywhy.com/sfbj/4/3/132

思路：一开始的想法是类似用双指针来进行，只要找到最小可以交换的单位就能够不停交换然后找到所有可以被交换的顺序，然而因为recursive实在是不熟练（对它的应用可能只停留在非常简单的加减法TAT），所以求助了AI，然后获得了改进版的代码，也学到了很多，比如说可以通过前后交换来避免current_permutation的数值顺序搞混导致数列出错，使用[:]才能正确复制当前迭代中的list而避免一直指向同样的list，可以使用for loops来代指其中一个pointer，交换开头的元素，只通过改变start的pointer来改变范围，逐渐缩小到可以找到所有“最后两个”交换的元素为止，真的非常精妙



代码：

```python
all_permutation = []
def permutation(current_permutation, start, end):
    if start == end:
        all_permutation.append(current_permutation[:])
    else:
        for i in range(start, end+1):
            current_permutation[start], current_permutation[i] = current_permutation[i], current_permutation[start]
            permutation(current_permutation, start+1, end)
            current_permutation[i], current_permutation[start] = current_permutation[start], current_permutation[i]

n = int(input())
current_permutation = [i for i in range(1, n+1)]
permutation(current_permutation, 0, n-1)

all_permutation.sort()
for perm in all_permutation:
    print(*perm)

```



代码运行截图 ==（至少包含有"Accepted"）==

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-05 011907.png">



### 02945: 拦截导弹 

dp, http://cs101.openjudge.cn/2024fallroutine/02945

思路：下面是来自gpt的代码，实际上在做的时候也有想到用前后来回比较，但是又在想会不会没有这么复杂就一直在死磕单线比较（当然可能也有可以解决的解法只是没想到），但是实在是没有时间所以只能直接求助ai了



代码：

```python
k = int(input())
height = list(map(int, input().split()))
dp = [1] * k

for i in range(k - 2, -1, -1):
    for j in range(i + 1, k):
        if height[j] <= height[i]:
            dp[i] = max(dp[i], dp[j] + 1)
 
print(max(dp))

# k = int(input())
# height = list(map(int, input().split()))
# stopped = [1] * k

# for i in range(1, k):
#     if height[i] <= height[i-1]:
#         stopped[i] = stopped[i-1] + 1
#     else: 
#         stopped[i] = 1

# if height[0] == max(height):
#     print(max(stopped)+1)
# else:
#     print(max(stopped))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-05 115226.png">



### 23421: 小偷背包 

dp, http://cs101.openjudge.cn/practice/23421

思路：参考答案的来的结果，实际不是很清楚为什么是这样的，但是能大致理解就是通过找到每个能组成B的price并且记录下来，通过找到最大的价格来得到最优结果，然而不是很明白为什么需要[0]



代码：

```python
N, B = map(int, input().split())
price = [0] + list(map(int, input().split()))
weight = [0] + list(map(int, input().split()))

bag=[[0]*(B+1) for _ in range(N+1)]
for i in range(1, N+1):
    for j in range(1,B+1):
        if weight[i]<=j:
            bag[i][j]=max(price[i]+bag[i-1][j-weight[i]], bag[i-1][j])
        else:
            bag[i][j]=bag[i-1][j]
            
print(bag[-1][-1])

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-05 165008.png">



### 02754: 八皇后

dfs and similar, http://cs101.openjudge.cn/practice/02754

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 189A. Cut Ribbon 

brute force, dp 1300 https://codeforces.com/problemset/problem/189/A

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

好难，除了第一题自己写出来，然后第二第三题有点思路之外根本写不出来，每日选做也因为课业繁忙没做很久了，现在只能慢慢追赶，尚且不知道机考能不能写出4题，现在真感觉悬了，希望老师手下留情T^T



