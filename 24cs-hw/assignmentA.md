# Assignment #A: dp & bfs

Updated 2 GMT+8 Nov 25, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯

dp, bfs, https://www.luogu.com.cn/problem/P1255

思路：这题刚好在课上的时候看过了，已经有了思路所以做的很快，就是在n阶和n+1那里卡了一下因为n阶阶梯其实代表的是要走到n+1这个阶梯才算结束



代码：

```python
N = [0 for _ in range(int(input())+1)] #stairs got n stairs, so need n+1 stepcase to reach another "floor"
N[0] = 1 #the first stage definitely only have one way to reach (start from the first stage)
for i in range(1, len(N)):
    if i >= 2: #start from the third stage, can reach by one/two step, s.t. two ways in total
        N[i] = N[i-1] + N[i-2]
    else:
        N[i] = 1 #the second stage can also only reach by one step

print(N[len(N)-1])

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-12-02 231709](https://github.com/user-attachments/assets/bb71ea31-9db0-452c-b481-aca583c5e482)



### 27528: 跳台阶

dp, http://cs101.openjudge.cn/practice/27528/

思路：只能说这题也挺简单的，和上一题基本没区别，就是说从只记录前两步的信息变成了需要继承当前阶梯之前所有的走法



代码：

```python
N = int(input())
stairs = [1]*(N) # every step at least got one way to reach
for i in range(1, N):
    stairs[i] += sum(stairs[:i])

print(stairs[N-1])

```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-12-02 231837](https://github.com/user-attachments/assets/c418d9bd-7777-40d4-bd45-1e7bd15acc6f)



### 474D. Flowers

dp, https://codeforces.com/problemset/problem/474/D

思路：本来是没打算写这一题的，在课上听了一顿突然感觉另外两题更难就跑过来写这道题了，对这种数据很大的就是几乎不会有任何思路TT，虽然说是和“上楼梯”很像但是就是憋不出%这种写法，只是说能知道是在找“吃法”的数量（虽说走法一样都是累积的，但是把单一数量组合还有总数量的组合分离这个思路确实很强），更深的内涵还得是一点一点理解出来，这种东西只依靠自己根本写不出来TT



代码：

```python
MOD = int(1e9 + 7) #every larger than MOD will only get the modulo
flowers = [0]*(int(1e5+1))
s = [0]*(int(1e5+1))
flowers[0] = 1
t, k = map(int, input().split())
for i in range(1, 100001):
    if i >= k:
        flowers[i] = (flowers[i-1] + flowers[i-k]) % MOD # possible arrangement for i red & white flowers
    else:
        flowers[i] = flowers[i-1]
    s[i] = (s[i-1] + flowers[i]) % MOD # all possible way to eat n flowers

for _ in range(t):
    a, b = map(int, input().split())
    print ((s[b] - s[a-1] + MOD) % MOD)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-12-03 231742](https://github.com/user-attachments/assets/82c9e4d9-580b-414b-988e-44c12c41cdfd)



### LeetCode5.最长回文子串

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/

思路：一开始还以为是从中心扩散然后只要一有不匹配的直接print然后退出，后来才发现可以从开头开始或者从中间到结尾，不一定完全对称，然后就稍微改了一下，变成每个index都找到其中最长的palindrome，然后从头到尾只要找到比有记录的更长的就更新掉之前的，由此找到最长的回文子串（下面是在电脑上的code和leetcode上提交的有点区别）



代码：

```python
def find_palindrome(palindrome, left, right): #find the longest palindrome can get in current index
    while left >= 0 and right < len(palindrome) and palindrome[left] == palindrome[right]:
        left -= 1
        right += 1
    
    return left + 1, right - left -1

def find_longest(s):
    max_length = 1
    left_index = 0
    S = len(s)
    for i in range(S):
        odd_index, odd_palindrome = find_palindrome(s, i, i)
        if i != S-1:
            even_index, even_palindrome = find_palindrome(s, i, i+1)
        else:
            even_index, even_palindrome = -1, -1
        max_current = max(odd_palindrome, even_palindrome)
        if max_current > max_length:
            if max_current == odd_palindrome:
                left_index = odd_index
            elif max_current == even_palindrome:
                left_index = even_index
            max_length = max_current
    
    return s[left_index:left_index+max_length]

s = input()
print(find_longest(s))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-12-03 220031](https://github.com/user-attachments/assets/6a68e118-762b-4d08-b502-3989fc04acbc)



### 12029: 水淹七军

bfs, dfs, http://cs101.openjudge.cn/practice/12029/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 02802: 小游戏

bfs, http://cs101.openjudge.cn/practice/02802/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

刚进入12月就宛如地狱一般的排起了备忘录，大作业逼近（不得不吐槽到底谁还让人没学会Python就去改Java程序啊啊啊啊啊啊），还得组织元旦活动，每日选做重新停摆，快要放弃挣扎了，感觉每门课都要崩，下学期不选这么多专业课了，事多给分还刁钻，还在趁着空闲时间尽量补救计概的基础知识（现在能自主做“模板题”但是再难一点的变形就想不出来了），希望计概可以及格🙏



