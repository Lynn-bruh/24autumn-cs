# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：标准的recursion式的dfs思路，感觉很快就想出来了，就是卡在不知道怎么return上（因为忘了怎么把更小的function的结果return回上一级），然后去查了一下就解决了


代码：

```python
def game(left, right, player):
    if left < right: # make sure left is larger
        left, right = right, left
    
    if right == 0:
        return player - 1

    if left // right >= 2: # from hint, the first player always win
        return player
    
    left %= right # since left is always larger
    return game(right, left, player+1) # now left is smaller

while True:
    a, b = map(int, input().split())
    if a + b == 0:
        break
    get_player = game(a, b, 1)
    if get_player % 2 == 1:
        print("win")
    else:
        print("lose")

```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-17 204718.png">



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：这题就是按照课上说的螺旋矩阵的方式做的，虽然有想到以其他方式做hhh，但是螺旋矩阵挺好用的，就是复杂度比较高不能适用于大样本



代码：

```python
def onion(matrix: list):
    results = [] # going to save the results of each layer
    left, right, top, bottom = 0, len(matrix)-1, 0, len(matrix)-1 # the largest index of each layer

    while matrix:
        temp = 0
        for i in range(left, right+1):
            temp += matrix[top][i]
        top += 1
        for i in range(top, bottom+1):
            temp += matrix[i][right]
        right -= 1
        for i in range(right, left-1, -1):
            temp += matrix[bottom][i]
        bottom -= 1
        if top > bottom:
            results.append(temp)
            break
        for i in range(bottom, top-1, -1):
            temp += matrix[i][left]
        left += 1
        if left > right:
            results.append(temp)
            break
        results.append(temp)
    
    return max(results)

n = int(input())
onions = []
for _ in range(n):
    onions.append(list(map(int, input().split())))
print(onion(onions))

```



代码运行截图 ==（至少包含有"Accepted"）==

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-17 211245.png">



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：想到了要用heap做，但是一开始用的是先全部喝了，然后再用heappop把扣最多health的potion去掉，但是这方法不行，然后去参考了一下答案，再对照了一下题目发现必须要从左边的药水喝到右边，然后以push记录喝过的，当health一旦变成0，就再用heappop把喝过的且最小的去除，但对于为什么一个一个去除（不依照顺序，但是最后也可以变得“依照顺序”）和一遍push一遍pop有答案上的区别，但是从第二个test来看，似乎是因为有可能一开始的health就是负的，而要求是health从开始到结束不能成为负数，时间上的先后顺序会导致最大能喝的药水有变化（当不要求时间，而是总量的时候，有可能某一时刻的health会变成负数）（哦对然后一样的代码hard的也能过haha）


代码：

```python
import heapq
def max_potions(potions):
    health = 0
    drank = []

    for potion in potions:
        health += potion
        heapq.heappush(drank, potion) # simply drink the current potion (must drink from left to right)
        if health < 0 and drank: # health can't be negative at any time
            health -= drank[0]
            heapq.heappop(drank) # cancelled to drink the most negative potions, so that the health can return to non-negative

    return len(drank)

n = int(input())
potions = list(map(int, input().split()))
print(max_potions(potions))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-17 220814.png">


### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：依照答案的思路稍微学习了一下Dijkstra，从本质上来说就是每次从最小消耗的point开始，然后和一般的search一样只是找到下一个消耗最少的点，把这个点加入到path中，如果最后path到终点的距离全部都被挡住了，那么就会跳出loop返回不成功，总结就是heap+search



代码：

```python
import heapq
def find_min_cost_path(m, n, maps, start, end):
    direction = [(1, 0), (0, 1), (0, -1), (-1, 0)]
    dist = {(start[0], start[1]): 0}  # Distance dictionary to keep track of minimum cost to each node
    path = [(0, start[0], start[1])]  # Priority queue: (cost, row, col)
    
    while path:
        cost, x, y = heapq.heappop(path)
        if (x, y) == (end_x, end_y): # reached target
            return cost
            # dfs
        for dx, dy in direction:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n and maps[nx][ny] != '#':
                new_cost = cost + abs(int(maps[nx][ny]) - int(maps[x][y])) # rmb to change it into int
                if (nx, ny) not in dist or new_cost < dist[(nx, ny)]:
                    dist[(nx, ny)] = new_cost # update cost
                    heapq.heappush(path, (new_cost, nx, ny)) # update path (from the smallest cost)

    return 'NO'

m, n, p = map(int, input().split())
topo = []
queries = []
for _ in range(m):
    topo.append(list(map(str, input().split()))) # might have '#'
for _ in range(p):
    start_x, start_y, end_x, end_y = map(int, input().split())
    if topo[start_x][start_y] == '#' or topo[end_x][end_y] == '#':
        print("NO")
    else:
        print(find_min_cost_path(m, n, topo, (start_x, start_y), (end_x, end_y)))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-12-17 233923.png">



### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

前面两题应该都能算是自己做的吧，算是有稍微抚慰到即将破碎的心境，第一次直观的体会到heap的好用之处！突然发现下周就是最后一周了，然后也会迎来机考TT，照例求求手下留情，专业课的任务量真的太大了根本学不过来QAQ，正在从头开始重新看老师的讲义，确实说的很详细，看着上面题解的代码写下思路自己梳理也能学到很多，希望机考可以AC2（剩下的求捞捞OAO）



