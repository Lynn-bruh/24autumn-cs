# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：先输入数据，有在想能不能一边输入一遍去找积累的面积，但是只是一个想法，还是按照所有数据都输入之后才来找，这样的话只需要定好8个方向，然后用dfs去找碰到第一个1（没计算过的面积，为了避免重复计算需要先not visited）之后这8个方向上是否=1然后慢慢延伸扩展出去逐渐积累到当下最大的面积就行，同时因为只要最大的，所以计算完之后直接比较当前最大面积和当下计算面积，最后等到整个matrix都查完了就找到了最大值，这题算是一半依靠AI写的吧，实施到一半就卡了:(



代码：

```python
def largest_connected_area(matrix, N, M):
    if not matrix or not matrix[0]:
        return 0

    visited = [[False for _ in range(M)] for _ in range(N)]
    max_area = 0
    directions = [(-1, -1), (-1, 0), (-1, 1),
                  (0, -1),         (0, 1),
                  (1, -1), (1, 0), (1, 1)]

    def dfs(r, c): # put inside a function since this used variable used before
        stack = [(r, c)]
        area = 0

        while stack:
            x, y = stack.pop() # the "last" element of connected area
            if visited[x][y]: # to make sure not find before
                continue

            visited[x][y] = True
            area += 1

            for dr, dc in directions: # explore all neighbours
                nr, nc = x + dr, y + dc
                if 0 <= nr < N and 0 <= nc < M and not visited[nr][nc] and matrix[nr][nc] == 1: # caution for searched connected 1
                    stack.append((nr, nc))

        return area

    for row in range(N):
        for col in range(M):
            if matrix[row][col] == 1 and not visited[row][col]:
                area = dfs(row, col) # Perform DFS and calculate the area
                max_area = max(max_area, area)

    return max_area

T = int(input())
for _ in range(T):
    N, M = map(int, input().split())
    chess = []
    for _ in range(N):
        chess_row = []
        chess_input = ' '.join(input()).split()
        for i in range(M):
            if chess_input[i] == "W":
                chess_row.append(1)
            else:
                chess_row.append(0)
        chess.append(chess_row)

    print(largest_connected_area(chess, N, M))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-11-26 003654](https://github.com/user-attachments/assets/a18abab0-6923-4323-8de2-5c54eebe387e)



### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：bfs不太熟悉，先按照课上说的模板，然后记得判断valid条件，剩下的其实和dfs没什么太大区别就是由popright变成popleft以此来得出最快的路径



代码：

```python
from collections import deque
def is_valid(x, y):
    if 0 <= x < m and 0 <= y < n:
        if not visited[x][y] and grid[x][y] != 2: # cannot visited and cannot be 2 simultaneously
            return True
    return False

def bfs(x, y):
    q = deque()
    q.append((x, y))
    visited[x][y] = True
    step = 0

    while q:
        for _ in range(len(q)): # Iterate all possibles of current step
            cx, cy = q.popleft()
            if grid[cx][cy] == 1:
                return step
            
            for dx, dy in direction: # all possible movements
                nx, ny = cx + dx, cy + dy
                if is_valid(nx, ny):
                    visited[nx][ny] = True
                    q.append((nx, ny))
        step += 1
    return 'NO' # if the target does not found

m, n = map(int, input().split())
direction = [(-1, 0), (1, 0), (0, 1), (0, -1)] # left, right, up, down
visited = [[False for _ in range(n)] for _ in range(m)]
grid = [] # dun use 'map' as variable name!
for i in range(m):
    grid.append([int(x) for x in input().split()])

print(bfs(0, 0)) # since definitely starting from (0, 0)

```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-11-26 232520](https://github.com/user-attachments/assets/3a352714-97ba-47b0-b521-b34dba9f8f3c)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：这题一开始是以为只要找一条路径然后使用的步数（因为课上说的马走日是计算最短路径），之后才发现是要找所有可能路径的总数，然后求教了AI，给出了类似permutation一样先标记为True再标记为False的recursion解法，然后稍微改了一下多加一个函数用以计算沿着某一点持续进行的当前path是否完成，然后一旦可以就返回+1，接着unmark找下一个可能点以及接下来的路径，很快就通过了



代码：

```python
direction = [(-2, 1), (-1, 2), (1, 2), (2, 1), 
             (-2, -1), (-1, -2), (1, -2), (2, -1)]

def passing(n, m, x, y):
    visited = [[False for _ in range(m)] for _ in range(n)]
    visited[x][y] = True
    total_cell = n*m # total_cell need to be visited
    return count_path(n, m, x, y, visited, 1, total_cell)

def count_path (n, m, x, y, visited, count, total_cell):
    if count == total_cell: # all cell visited, this path completed
        return 1
    
    total_paths = 0 # if no path is completed, none returned, total path still = 0
    for dx, dy in direction:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny]:
            visited[nx][ny] = True # visited this cell
            total_paths += count_path(n, m, nx, ny, visited, count+1, total_cell) # using this step as "previous" step, and find "next" step
            visited[nx][ny] = False # backtrack, use the other step as "previous" step

    return total_paths
T = int(input())

for _ in range(T):
    n, m, x, y = map(int, input().split())
    print(passing(n, m, x, y))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-11-26 204819](https://github.com/user-attachments/assets/b2d2bd2d-9609-4223-9008-eb034b94a008)



### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：这道题卡了挺久的，卡在如何解决相等的weight的条件上，以及后面才发现不止可以走右和下，还可以走上和左，因为所求的不是要最短的最大，只要得到最大的weight就可以，然后还有关于输出的问题，之后参考了答案直接引用global不要在function里更改后才解决，本质思路也还是mark as visited + recursion + backtrack



代码：

```python
def is_valid(x, y):
    return 0 <= x < n and 0 <= y < m and not visited[x][y]

def finding_paths(x, y, cur_weight, path:list):
    global max_weight, best_path
    if (x, y) == (n-1, m-1): # reach the end
        if cur_weight > max_weight:
            max_weight = cur_weight
            best_path = path[:]
        return # already make changes on global best_path
    
    for dx, dy in direction:
        nx, ny = x + dx, y + dy

        if is_valid(nx, ny):
            visited[nx][ny] = True
            path.append((nx+1, ny+1))
            finding_paths(nx, ny, cur_weight+weight[nx][ny], path) # recursion
            path.pop() # backtrack
            visited[nx][ny] = False

n, m = map(int, input().split())
weight = []
for i in range(n):
    weight.append(list(map(int, input().split())))
direction = [(-1, 0), (1, 0), (0, 1), (0, -1)] # up, down, right, left
visited = [[False for _ in range(m)] for _ in range(n)]
best_path = []
max_weight = -float('inf') # since got negative weight, need to get the larger's
visited[0][0] = True
finding_paths(0, 0, weight[0][0], [(1, 1)]) # must start from top-left
for x, y in best_path:
    print(x, y)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![Screenshot 2024-11-26 224326](https://github.com/user-attachments/assets/caa5b1dd-863c-46e1-b37b-d8872b73738c)





### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

目前仍在跟进每日选做，但是进度缓慢，还有各种事情又开始忙起来了，同时也有在补做之前作业来不及做的部分，比如田忌赛马，也在看其他人的解法，收获很多，但是还没完全消化成自己的，只能记住相关概念但是具体应该怎么写出来仍需要对照



