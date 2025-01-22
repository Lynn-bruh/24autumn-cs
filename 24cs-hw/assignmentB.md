# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）⽉考：<mark>没在机考上考</mark>。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：一开始直接上了O(n^2)的代码然后超时了，就在想能不能减少到O(n)，一开始就是说遍历了整个list两两比较然后用max用来找最大的涨幅，后来想着只要能找到依照时间顺序而来的最小的min，这样就可以得到最大的max_increase，然后就多加了一个比较min的，由此变成了只需要遍历一次



代码：

```python
a = list(map(int, input().split()))
max_increase = 0
min_price = 10001 # the maximum ele is 10000
for i in range(len(a)):
    min_price = min(min_price, a[i])
    max_increase = max(max_increase, a[i] - min_price)

print(max_increase)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-09 231019.png">



### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：没有什么思路，基本是看着答案写的T^T，有想出可以用sum(chicken)/k，但是具体并不理解



代码：

```python
n, k = map(int, input().split())
chicken = list(map(int, input().split()))
chicken.sort()
total_time = sum(chicken)
max_time = total_time / k
while True:
    if chicken[-1] > max_time:
        total_time -= chicken.pop()
        k -= 1
        max_time = total_time / k
    else:
        break

print(f"{max_time:.3f}")

```



代码运行截图 ==（至少包含有"Accepted"）==

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-10 164437.png">



### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：这题其实没什么思路，因为刚没看md文档，然后没发现写着要用dp来做，就很烦躁的想了几种方式但是无一例外都失败了，就去看了答案才发现用的dp，总之就是对题型非常的陌生，根本不知道什么样的题应该用什么样的算法（考试的时候也不会有时间给我慢慢想了吧TT）



代码：

```python
def list_to_buy(shopping: list):
    not_putting_back = [0]*len(shopping)
    putting_back = [0]*len(shopping)

    not_putting_back[0], putting_back[0] = shopping[0], shopping[0] # at least need to buy one thing
    for i in range(1, len(shopping)):
        not_putting_back[i] = max(not_putting_back[i-1]+shopping[i], shopping[i]) # either add one more thing into bucket or choose the one which have larger value
        putting_back[i] = max(not_putting_back[i-1], putting_back[i-1]+shopping[i]) # either put back the last or not (only one can be put back, if inherit the putting_back, shopping[i] must be added)
    
    max_of_not = max(not_putting_back)
    max_of_yes = max(putting_back)
    return max(max_of_not, max_of_yes)

buying = list(map(int, input().split(sep = ",")))
print(list_to_buy(buying))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-09 233324.png">



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：这题，就是看到了greedy，然后去做了，然后经历几次看错题目，重看题目，改了错误的地方之后，莫名其妙就过了，原本“看”到的是在该大臣前面所有人的和除以自己右手上的数，然后就想着这不就左手由小到大排列就可以了吗，然后发现是乘积，然后靠着题目的提示（有点不好意思，但是这真的挺误打误撞的）发现金币数最大的情况下都是左右手乘积最小的那个大臣在最后面，所以就用左右手乘积由小到大排列，然后就过了



代码：

```python
n = int(input())
king = list(map(int, input().split()))
courtiers = []
for _ in range(n):
    a, b = map(int, input().split())
    courtiers.append([a, b])
courtiers.sort(key = lambda x:(x[0]*x[1]))

max_gold_not_divided = king[0]
max_gold = max_gold_not_divided // courtiers[0][1]
for i in range(1, n):
    max_gold_not_divided *= courtiers[i-1][0]
    max_gold = max(max_gold, max_gold_not_divided//courtiers[i][1])

print(max_gold)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-10 001228.png">



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

人已经半废了，感觉java更简单；D（因为都是gpt写的哈哈哈哈哈），每天都在激烈讨论大实验，还有个人实验等着构思，还有期末论文和考试等着我，月考题真的是差点一题都不会写，希望机考可以容易一点，不然可能就等着AC0了QAQ，然后机考不会，笔试不会，最后总评就挂了XOX


