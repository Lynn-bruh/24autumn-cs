# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：似会非会，看了题解之后才知道怎么做，选了一个和自己思路最相近的，之前是碰到even的时候把它加入不怀疑的俩表里，然后改成了从所有的set里面删除，这样就可以直接去掉这些coin不会重复碰到他们，然后补充了一些需要判断的条件来把不是假币的真币删掉



代码：

```python
n = int(input())
for _ in range(n):
    coins = set("ABCDEFGHIJKL")
    suspicion = {}
    for _ in range(3):
        left, right, state = input().split()
        # left = ' '.join(left).split() # dun need, can simply use index since it is a string
        # right = ' '.join(right).split()
        if state == "even": # only 1 counterfeit coin, condition even must have got all real coin
            coins = coins - set(left) - set(right)
        elif state == 'up': # right up 
            for coin in left:
                if coin not in suspicion: # left heavy
                    suspicion[coin] = 'heavy'
                else:
                    if suspicion[coin] == "light": # a conterfeit coin is not possible to be heavy and light at the same time 
                        coins -= set(coin)
            for coin in right:
                if coin not in suspicion: # right light
                    suspicion[coin] = 'light'
                else:
                    if suspicion[coin] == "heavy":
                        coins -= set(coin)
            coins = coins & (set(left)|set(right)) # set's knowledge, to find the only appeared coins
        elif state == "down": # right down
            for i in range(4): # or can directly use for i in range(4) to handle left and right at the same time
                if left[i] in suspicion:
                    if suspicion[left[i]] == "heavy":
                        coins -= set(left[i])
                else:
                    suspicion[left[i]] =  "light"
                if right[i] in suspicion:
                    if suspicion[right[i]] == "light":
                        coins -= set(right[i])
                else:
                    suspicion[right[i]] = "heavy"
            coins = coins & (set(left)|set(right))
    counterfeit = ''.join(coins) # turn remain coin into string
    print(f'{counterfeit} is the counterfeit coin and it is {suspicion[counterfeit]}.')

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-12-23 175051](https://github.com/user-attachments/assets/9e037e06-0774-4ac5-9073-070419c6c086)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：看了解答明白了，靠自己想不出来



代码：

```python
import heapq
R, C = map(int, input().split())
snow_area = []
for _ in range(R):
    snow_area.append(list(map(int, input().split())))

snow_map = [(snow_area[i][j], i, j) for i in range(R) for j in range(C)] # also can use a sort (from lowest height to highest, rmb to save the coordinate)
# Equal to:
# for i in range(R):
#   for j in range(C):
heapq.heapify(snow_map) 

# initial length & longest path & direction
dp = [[1]* C for _ in range(R)]
directions = [(0, 1), (0, -1), (1, 0), (-1, 0)] # down, up, right, left
longest_path = 1 # definitely 1 since one point can always be chosen

while snow_map: # while there is still elements in snow_map (different here if using sorting method)
    height, x, y = heapq.heappop(snow_map) # start from the shorter height (= shorter path)
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < R and 0 <= ny < C and snow_area[nx][ny] < height:
            dp[x][y] = max(dp[x][y], dp[nx][ny] + 1) # choose the longer path, remew the length
    longest_path = max(dp[x][y], longest_path) # renew the longest path after the related points' length was finish renew

print(longest_path)

```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-12-23 233740](https://github.com/user-attachments/assets/f0a8958b-5d64-4f9e-8472-12164a1adc2a)



### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：这题比较特殊的是要找到两个5的坐标，然后因为有两个点所以不能用visited的True False，而是要记录共同的两个点的坐标，只有两个点都一样才能算是visited，所有的移动都是两个x和两个y一起移动的，因为不能旋转，所以情况比较简单一点，和一般单个点的搜索一致



代码：

```python
from collections import deque
n = int(input())
crab_map = [list(map(int, input().split())) for _ in range(n)]
direction = [(0, 1), (0, -1), (1, 0), (-1, 0)]

# find mushroom
def crab_find(crab_start):
    crab = deque()
    crab.append(crab_start)
    visited = set()
    visited.add(crab_start)

    while crab:
        x1, y1, x2, y2 = crab.popleft()
        if crab_map[x1][y1] == 9 or crab_map[x2][y2] == 9:
            return 'yes'
        
        for dx, dy in direction:
            nx1, nx2 = x1 + dx, x2 + dx
            ny1, ny2 = y1 + dy, y2 + dy

            if 0 <= nx1 < n and 0 <= ny1 < n and 0 <= nx2 < n and 0 <= ny2 < n:
                if crab_map[nx1][ny1] != 1 and crab_map[nx2][ny2] != 1 and (nx1, ny1, nx2, ny2) not in visited:
                        crab.append((nx1, ny1, nx2, ny2))
                        visited.add((nx1, ny1, nx2, ny2))
        
    return 'no'

# find crab
start = []
for i in range(n):
    for j in range(n):
        if crab_map[i][j] == 5:
            start.extend([i, j])

print(crab_find((start[0], start[1], start[2], start[3])))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-12-24 224907](https://github.com/user-attachments/assets/94cfe74f-29d8-401c-b897-86b430740d01)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：只会到乘以两倍长度，后续参考了答案，然后又去问了gpt为什么是这样的一个公式才完全明白（看到10**len(num)着实懵了一下），总的来说就是比较当前位置的num和用m-num长度的数字把它“拉”去前几个digit然后加上当前位置的数字（虽然它的解释也有点不完全准确）



代码：

```python
import math
m = int(input())
n = int(input())
nums = input().split()
max_len = 0
for i in range(n): # find the longest str
    max_len = max(max_len, len(nums[i]))
nums.sort(key = lambda x: x * math.ceil(2 * max_len/len(x)), reverse = True) # sorting according to 2 times of the length

dp = [0] * (m+1)
for num in nums:
    for i in range(m, len(num)-1, -1): # at most m digits, num consist len(num)
        dp[i] = max(dp[i], dp[i - len(num)] * (10 ** len(num)) + int(num))

print(dp[-1])

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-12-24 002248](https://github.com/user-attachments/assets/227ea24d-aa69-4228-b041-2e318e39e329)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：想做这题，但是来不及了TT



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

最后一次作业了T^T，不知不觉也到了学期末，基本就是学期初壮志凌云立志征服计概，学期末可怜兮兮求求老师出点简单的题，标注大一学果然是有大一学的道理的TT，现在想想大一真是轻松的不得了，开始准备cheat sheet了，cheat sheet对我这种连基础模板都得对照着抄的人来说还是很有用的，希望可以AC2拜托拜托（求求E出的简单点QAQ）



