# Cheat Sheet
## Basic Grammar
### enumerate
for index, char in enumerate(iterable) -> return unique index with element (can get element without repeat / not only the first would found)    

### output
print(f'{a} is {b:.1f}')    
print('{} is not {:.2f}'.format(a, b))    
print(*list, sep = ' ' / '\n')    

## Data Structures
### strings
S = str(object = '')    
S + / == S    
char **in** S (__contains__)    
**only the first char**: S.find() -> -1 if not find / S.capitalize() -> Only the first / S.title() -> Every First / 
S.index() / S.replace(self, old, new)
S.count()/S.lower()/S.upper()/S.swapcase()/S.split(sep = None) -> return list (whitespace as default sep)     
/S.strip(sep = None) -> return str copy (remove 1st and last whitespace in default, sep is removed if indicated)    
'str'.join(iterable) -> return str    

for char in str: (not necessary for i in range(len(str)), char can be >= single char)    
ord() -> return ASCII; chr() -> return char   
**ASCII FOR** 49 -> 1; 65 -> A; 97 -> a   

### tuple
T = () / tuple(iterable)    
T + / == / >= value   
**only first**T.index(); len(T); t in T   

### list
self + value: +; -; in; ==; >=; >; +=; *=;    
self: len(); reversed(); sorted(key = lambda x: ); L.reverse(); L.sort()    
self + object: L.count(object); L.append(object); L.append(iterable); L.copy() 浅拷贝; L.index() 只有第一个; L.pop();    

### dictionary
dict() 创建空字典; dict[key] = value 创建新link; dict.update() 自己用help搜   
key **in** dict; del(key)(__delitem__(self, key)); dict.pop() 删除key返回value; dict.popitem() 删除后返回(key, value)   
dict.keys(); dict.items()    

### set
set() 空白set; set(iterable) 记得设变量名    
集合：交集 &; 并集 |; 包含 in; 全等 ==; 大小(表示包含关系) >= / > / <= / <; 共同元素 &=; 独特元素 |=; 差集 -=; 全部独特元素 ^=; 对称差 ^    
.add(element); .copy() 浅拷贝; .update(); .pop() 随机    

## Libraries
### deque
from collections import deque    
双端队列；类似list，重点在于'left'的存在   
deque.popleft() / deque.extendleft() / deque.appendleft()   

### heapq
import heapq   
优先队列；可以快速找到最小（大）的值   
heapq.heapify(list) # now the list is a heap   
heapq.heappush(list, item)    
heapq.heappop(list) # pop the smallest   
largest = -1 * heapq.heappop(-list) # pop the largest   
heapq.nlargest(n, list, key=None) / heapq.nsmallest(n, list, key=None) 可以找到第n大/小的元素（返回n个值）   
heapq.heappushpop(list, item) 先推再弹 / heapq.heapreplace(list, item) 先弹再推    

### defaultdict
from collections import defaultdict   
类似dict但会为缺失的键提供一个默认值(int->0,list->[],set->set(),str->"")而不是抛出KeyError   
dictionary = defaultdict(list)  # 自定义默认值defaultdict(lambda: XXX)   

### OrderedDict
from collections import OrderedDict   
可以保留key的插入顺序，input方式与dict一致   
od = OrderedDict() 创建新的特殊dict   
del ob[key] 删除元素; od.popitem() 删除最后一个元素; od.popitem(last = False) # 删除第一个元素   

### math
import math   
math.: sqrt() / log(x, [base=math.e]) / sin() / asin() etc. / floor() -> got accuracy issue / ceil() / pow(x, y) -> x**y / factorial(x)   
comb(n, k) -> nCk / radians() -> ° to rad / degrees() -> rad to °

### bisect
**sort before use**   
import bisect   
bisect.bisect_left(a: list, finding_value) -> return index   
bisect.bisect_right(a: list, finding_value)   
```python
def binary_search(arr, target):
  left, right = 0, len(arr) - 1
  while left <= right:
    mid = (left + right) // 2
    if arr[mid] == target:
      return mid
    elif arr[mid] <target:
      left = mid + 1
    else:
      right = mid - 1
	return -1
```

### sys
import sys   
sys.stdin()   
sys.stdout()   
sys.exit()   

### lru_cache
from functools inport lru_cache   
@lru_cache(maxsize = None) 超过了记忆再限制   

## Algorithms
### Sieve
#### Euler
```python
def euler_sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    primes = []
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
        for p in primes:
            if i * p > n:
                break
            is_prime[i * p] = False
            if i % p == 0:
                break
    return primes
```
### Eratosthenes
```python
def sieve_of_eratosthenes(n): # 创建⼀个Boolean，初始化为True，表示所有数字都假设为素数
    primes = [True] * (n + 1)
    primes[0] = primes[1] = False # 0 & 1不是素数
    for i in range(2, int(n**0.5) + 1): # 从 2 开始，处理每个数字
        if primes[i]: # 如果i是素数
        # 将i的所有倍数标记为⾮素数
            for j in range(i * i, n + 1, i):
                primes[j] = False
    # 返回所有素数
    return [x for x in range(2, n + 1) if primes[x]]
```

### recursion
#### Hanoi Tower
```python
steps = []
def hanoi(plate, previous_tower, current_tower, not_used):
    if plate == 1:
        steps.append([previous_tower, current_tower])        
    else:
        hanoi (plate-1, previous_tower, not_used, current_tower)
        hanoi (1, previous_tower, current_tower, not_used)
        hanoi (plate-1, not_used, current_tower, previous_tower)
```

### Permutations (for small size of data)
```python
what_to_permute = [] # input
all_permutation = []
def permutation(current_permutation, start, end):
    if start == end:
        all_permutation.append(current_permutation[:])
    else:
        for i in range(start, end+1):
            current_permutation[start], current_permutation[i] = current_permutation[i], current_permutation[start]
            permutation(current_permutation, start+1, end)
            current_permutation[i], current_permutation[start] = current_permutation[start], current_permutation[i]
```
#### Find Next Permutation
从右往左找到第一个降序相邻数对nums[i],nums[i+1],未找到说明已经逆序,直接返回反转值. 找到后在右边找第一个nums[j]比nums[i]大,调换. 反转nums[i+1]
```python
nums = [] # input
def next_permutation(nums):
    i = len(nums) - 2
    while i >= 0 and nums[i] >= nums[i + 1]:
        i -= 1
    if i >= 0:
        j = len(nums) - 1
        while nums[j] <= nums[i]:
            j -= 1
        nums[i], nums[j] = nums[j], nums[i]
    nums[i + 1:] = reversed(nums[i + 1:])
    return nums
```
#### Combinations
```python
import itertools
for item in itertools.product('AB', repeat=2):  # 生成笛卡尔积
    print(item)  # ('A', 'A')\n('A', 'B')\n('B', 'A')\n('B', 'B')
for item in itertools.product('AB', '12'):
    print(item)  # ('A', '1')\n('A', '2')\n('B', '1')\n('B', '2')
```

### dp
#### 0-1背包(选择只有拿/不拿(重点:初始化,转移方程,提取结果))
```python
def knapsack_v1(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if w >= weights[i - 1]: # 第i个物品能装进,判断不选/选这个物品
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1])
            else:
                dp[i][w] = dp[i - 1][w]
    return dp[n][capacity]

def knapsack_v2(weights, values, capacity):  # 一维视为二维的滚动数组实现
    n = len(weights)
    dp = [0] * (capacity + 1)
    for i in range(n): # 遍历每件物品
        for w in range(capacity, weight[i] - 1, -1): # 倒序遍历背包容量(保证每件物品只能选一次)
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[capacity]
```
#### 完全背包（允许在不超容量的前提下无限次选取）
```python
def knapsack_complete(weights, values, capacity):
    dp = [0] * (capacity + 1) # dp[j]为当背包容量为j时,背包所能容纳的最大价值
    for i in range(len(weights)):
        for w in range(weights[i], capacity + 1): # 从当前物品的重量开始，计算每个容量的最大价值
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[capacity]
```
#### 完全背包（必须装满）
```python
def knapsack_complete_fill(weights, values, capacity):
    dp = [-float('inf')] * (capacity + 1) # 初始值为负无穷，表示不能达到该容量
    dp[0] = 0 # 容量为 0 时，价值为 0
    for i in range(len(weights)):
        for w in range(weights[i], capacity + 1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[capacity] if dp[capacity] != -float('inf') else 0  # 如果 dp[capacity] 仍为 -inf，说明无法填满背包
```
#### 多重背包（二进制优化）
```python
def binary_optimized_multi_knapsack(weights, values, quantities, capacity):
    n = len(weights)
    items = []
    # 将每种物品根据数量拆分成若干子物品
    for i in range(n):
        w, v, q = weights[i], values[i], quantities[i]
        k = 1
        while k < q:
            items.append((k * w, k * v))  # 添加子物品(weight, value)
            q -= k
            k <<= 1  # 位运算,相当于k *= 2,按二进制拆分,物品时间复杂度由q变为log(q)
        if q > 0:
            items.append((q * w, q * v))  # 添加剩余部分,如果有的话
    # 动态规划求解0-1背包问题
    dp = [0] * (capacity + 1)
    for w, v in items:  # 遍历所有子物品
        for j in range(capacity, w - 1, -1):  # 01背包的倒序遍历
            dp[j] = max(dp[j], dp[j - w] + v)
    return dp[capacity]
```
#### 最长上升子序列
```python
def length_of_lis(nums):
    if not nums:
        return 0
    dp = [1] * len(nums)
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)
```
#### 双重dp（土豪购物）
```python
def list_to_buy(shopping: list):
    not_putting_back = [0]*len(shopping)
    putting_back = [0]*len(shopping)

    not_putting_back[0], putting_back[0] = shopping[0], shopping[0] # at least need to buy one thing
    for i in range(1, len(shopping)):
        not_putting_back[i] = max(not_putting_back[i-1]+shopping[i], shopping[i]) # 选择加多一样东西或选择价值最大的
        putting_back[i] = max(not_putting_back[i-1], putting_back[i-1]+shopping[i]) # 是否放回最后一个 (only one can be put back, if inherit the putting_back, shopping[i] must be added)
    
    max_of_not = max(not_putting_back)
    max_of_yes = max(putting_back)
    return max(max_of_not, max_of_yes)
```

### dfs
#### 模板
```python
# 可以用sys.setrecursionlimit()来阻止搜索太深
# 可以用@lru_cache()来储存避免重复计算
maps = [] # input
directions = [] # define moves
visited = [False]*len(maps) # mark (can reverse to find more paths)
def dfs(x, y):
    # 可添加最基础的返回值
    # visited[x][y] = True # 也可直接再maps上修改
    for dx, dy in directions:
        nx, ny = x+dx, y+dy
        if 0 <= nx < row_len and 0 <= ny < col_len: # and maps[nx][ny] 判断条件，是否未被访问
            # 修改（取决于是否已在maps修改）
            dfs(nx, ny)
# 记得回溯修改（取决于题目）
# 如果需要返回记得return dfs() or sth else不然不会得到正确答案
```
#### dfs + stack模拟recursion
```python
def dfx(x, y):
    stack = [(x, y)]
    while stack:
        x, y = stack.pop()
        if maps[x][y] != #条件:
            continue # 跳过
        maps[x][y] = # 标记访问
        for dx, dy in directions:
            nx, ny = x+dx, y+dy
            if #略（除了边界记得判断题目要求的是否可以通过的要求）:
                stack.append((nx, ny))
    # return 如果要什么特殊值的话
```
#### dfs + dp 滑雪：反向找到递增
```python
matrix = # input
dp = [] # flw input
def dfs(x, y):
    dire = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    for dx, dy in dire:
        nx, ny = x + dx, y + dy
        if 0 <= nx < r and 0 <= ny < c and matrix[x][y] > matrix[nx][ny]:
            if dp[nx][ny] == 0:
                dfs(nx, ny)
            dp[x][y] = max(dp[x][y], dp[nx][ny] + 1)
    if dp[x][y] == 0:
        dp[x][y] = 1
max_len=0
for i in range(r):
    for j in range(c):
        if not dp[i][j]:
            dfs(i, j)
        max_len = max(max_len, dp[i][j])
``` 
```python
# 搭配heap，但是因为弹出的永远是最小的，所以要记录坐标
matrix_coor = [(matrix[i][j], i, j) for i in range(R) for j in range(C)] # also can use a sort (from lowest height to highest, rmb to save the coordinate)
heapq.heapify(matrix_coor) 
dp = [[1]* C for _ in range(R)]
directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
max_len = 1
while snow_map:
    height, x, y = heapq.heappop(snow_map)
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < R and 0 <= ny < C and snow_area[nx][ny] < height:
            dp[x][y] = max(dp[x][y], dp[nx][ny] + 1)
    longest_path = max(dp[x][y], longest_path)
```

### bfs
#### 模板
```python
from collections import deque
def bfs(s, e):
    inq = set()
    inq.add(s) # 用来判断是否找过
    q = deque()
    q.append((0, s))

    while q:
        now, top = q.popleft()
        if top == e: # 也不一定必要
            retun now
        
        if #什么条件:
            q.append((next_s, next_e))
```

### intervals
#### 合并所有有交集的区间
```python
# 把所有重叠的区间合并
intervals = [] # input, 有左右端点
def merge(intervals):
    intervals.sort(key = lambda x: x[0]) # 按左端点排序
    ans = []
    for interval in intervals:
        if ans and intervals[0] <= ans[-1][1]: # 已有第一个区间，且可连上前一个区间的右端点
            ans[-1][1] = max(ans[-1][1], intervals[1]) # 更新新的右端点
        else: # 第一个区间 / 连不上的区间
            ans.append(interval)
```
#### 不相交/无重叠区间
```python
# 选择尽量多的不相交区间（可计算总区间数：count）
def erase(intervals):
    intervals.sort(key = lambda x: x[1]) # 按右端点小到大排序
    count = 0
    end = intervals[0][1] # 设置一个极小的数比较适合
    for interval in intervals:
        if interval[0] >= end: # 下一个的左端点大于前一个的右端点（不相交）
            count += 1
            end = interval[1]
```
#### 区间选点
```python
# 每个重叠区间只选一个点
# 同上
```
#### 覆盖目标区间的最小区间数量
```python
def cover(intervals, start, end): # start, end为目标区间端点
    intervals.sort(key = lambda x: x[0]) # 左端点从小到大排序
    ans = 0
    i = 0
    while i < len(clips) and start < end:
        maxR = -1  # 维护:满足能覆盖初始值的区间的右侧最大时间
        while i < len(clips) and clips[i][0] <= start:
            maxR = max(maxR, clips[i][1])
            i += 1
        if maxR < start: # 无法继续覆盖
            return -1
        ans += 1
        if maxR >= end: # 覆盖完全
            return ans
        start = maxR  # 未达到结尾
    return -1 # 若i超出列表长度
```
#### 区间分组使组内区间互不相交
```python
import heapq
def group(n, intervals):
	intervals.sort() # 按左端点排序
	min_heap = []  # 创建最小堆,维护所有组的末端并快速找到最小末端
	for interval in intervals:
		start, end = interval
		if not min_heap or min_heap[0] > start: # 若空堆或最小的右端大于当前左端，加入新的一组
			heappush(min_heap, end)
		else: # 弹出当前最小的（最早结束的）并压入新的值（当前结束时间）
			heappop(min_heap)
			heappush(min_heap, end)
	return len(min_heap) # 最终堆中的元素个数即一共多少组
```

### Sorting
#### Bubble Sort
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n): # 由一个元素开始扩大排列的arr
        swapped = False
        for j in range(0, n-i-1): # 对小的arr进行排列
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped: # 当遍历完所有arr并发现没有发生变化后，直接退出
            break
    return arr # 返回排列好的
```
#### Selection Sort
```python
def selectsort(arr):
    for i in range(len(arr)):
        minIndex = i
        for j in range(i+1, len(arr)):
            if arr[j] < arr[minIndex]: # 找到当前元素后边最小的
                minIndex = j
        arr[i], arr[minIndex] = arr[minIndex], arr[i] # 直接把最小的挪过来最前面，可保证每次都找到最小
    return arr
```
#### Insertion Sort
```python
def insertsort(arr):
    for i in range(1, len(arr)):
        key = arr[i] # 提前保存当前元素
        j = i-1
        while j >= 0 and key < arr[j]: #向前搜索
            arr[j+1] = arr[j] # 元素向后挪
            j -= 1
        arr[j+1] = key # 直到在key前面的比key大
    return arr
```
#### Merge Sort
```python
def merge_sort(arr):
    if len(arr) <= 1: return arr  # 如果数组长度小于等于1,则无需排序
    mid = len(arr) // 2
    left = arr[:mid]
    right = arr[mid:]
    sorted_left = merge_sort(left)  # 递归地对两半进行分割排序
    sorted_right = merge_sort(right)
    return merge(sorted_left, sorted_right)  # 合并排序后的两半
def merge(left, right):
    result = []  # 用于存放合并后的结果
    i = j = 0    # 两个指针，分别指向左半部分和右半部分的开头
    while i < len(left) and j < len(right):  # 比较左右两部分的元素，将较小的放入结果数组
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])  # 将剩余的元素加入结果数组
    result.extend(right[j:])
    return result
```

### Kadane求最大子序列
```python
def maxSubArray(arr):
    current_sum = 0
    max_sum = float('-inf')  # 初始化为负无穷，处理全负数组的情况
    for num in arr:
        current_sum = max(num, current_sum + num)  # 更新当前子数组和
        max_sum = max(max_sum, current_sum)  # 更新最大子数组和
    return max_sum
```
```python
# +储存路径
def maxSubArray(arr):
    current_sum = 0
    max_sum = float('-inf')
    start = 0  # 当前子数组的起始索引
    end = 0    # 最大子数组的结束索引
    temp_start = 0  # 临时存储起始索引
    for i, num in enumerate(arr):
        if num > current_sum + num:  # if……else状态转移
            current_sum = num
            temp_start = i  # 重新开始新的子数组
        else:
            current_sum += num
        if current_sum > max_sum:
            max_sum = current_sum
            start = temp_start  # 更新最大子数组的起始索引
            end = i  # 更新最大子数组的结束索引
    return max_sum, arr[start:end+1]
```

### Dijkstra（走山路）
```python
# 用最小堆实现；适用于寻找加权图单源最短路径(起始点到其他节点的最短路径,需要权值非负)
import heapq
directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
def dijkstra(xs, ys, xe, ye):
    if region[xs][ys] == "#" or region[xe][ye] == "#":
        return "NO"
    if (xs, ys) == (xe, ye):
        return 0
    pq = []  # 初始化堆
    heapq.heappush(pq, (0, xs, ys))  # (体力消耗, x坐标, y坐标)
    visited = set()  # 已访问坐标集
    efforts = [[float('inf')] * n for _ in range(m)]  # 到达图中位置需要的体力,初始化为inf,即不可达到
    efforts[xs][ys] = 0  # 初始化体力记录表
    while pq:
        current_effort, x, y = heapq.heappop(pq)  # 取出堆顶元素
        if (x, y) in visited: continue  # 若访问过,跳过这个节点
        visited.add((x, y))  # 未访问过,加入已访问
        if (x, y) == (xe, ye):  return current_effort  # 达到终点则返回
        for dx, dy in directions:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n and (nx, ny) not in visited:
                if region[nx][ny] == "#":  continue  # 无法达到,跳过
                effort = current_effort + abs(int(region[x][y]) - int(region[nx][ny]))
                if effort < efforts[nx][ny]:
                    efforts[nx][ny] = effort
                    heapq.heappush(pq, (effort, nx, ny))  # 放入堆中
    return "NO"
```

## Other technique
### Matrices
#### 保护圈
在边长的基础上+2的0，可以避免超出index
#### spiral matrice
主要技巧就是设置4个index，left, right, top, down（无论是spiraling or onion都可以用）
```python
spiral_matrix = []
left, right, top, bottom = 0, n-1, 0, n-1 # left index, right index
while spiral_matrix:
    for i in range(left, right+1):
        # 操作
    top += 1
    if top > bottom: # this check is not necessary
        break
    for i in range(top, bottom+1):
        # 操作
    right -= 1
    if left > right: # not necessary
        break
    for i in range(right, left-1, -1): 
        # 操作
    bottom -= 1
    if top > bottom: 
        break
    for i in range(bottom, top-1, -1): # from bottom to top
        # 操作
    left += 1
    if left > right: 
        break
```

## 题
### 水淹七军
```python
def dfs_iterative(x, y, h, m, n, grid, water_height):
    stack = [(x, y)]
    water_height[x][y] = h
    while stack:
        cx, cy = stack.pop()
        for dx, dy in directions:
            nx = cx + dx
            ny = cy + dy
            if (0 <= nx < m and 0 <= ny < n and
                grid[nx][ny] < h and
                    water_height[nx][ny] < h):
                water_height[nx][ny] = h
                stack.append((nx, ny))
```
### 螃蟹采蘑菇（bfs）
```python
def bfs(n, grid):
    start, end = find_start_end(n, grid)
    q = deque([start])
    visited = set()
    visited.add(tuple(start))
    flag = False
    while q:
        if flag:
            break
        current = q.popleft()
        if end in current:
            flag = True
            break
        for dx, dy in directions:
            nx1, ny1 = current[0][0]+dx, current[0][1]+dy
            nx2, ny2 = current[1][0]+dx, current[1][1]+dy
            if 0 <= nx1 < n and 0 <= ny1 < n and 0 <= nx2 < n and 0 <= ny2 < n:
                if grid[nx1][ny1] != 1 and grid[nx2][ny2] != 1:
                    next_state = [(nx1, ny1), (nx2, ny2)]
                    if tuple(next_state) not in visited:
                        visited.add(tuple(next_state))
                        q.append(next_state)
    return flag
```
### 迷宫最短路径（bfs）
```python
def bfs(start_x, start_y, n, m, maze):
    queue = deque()
    queue.append((start_x, start_y))
    in_queue = set()
    prev = [[(-1, -1)] * m for _ in range(n)]
    in_queue.add((start_x, start_y))
    while queue:
        x, y = queue.popleft()
        if x == n - 1 and y == m - 1:
            return prev
        for i in range(MAX_DIRECTIONS):
            next_x = x + dx[i]
            next_y = y + dy[i]
            if 0 <= x < n and 0 <= y < m and maze[x][y] == 0 and (x, y) not in in_queue:
                prev[next_x][next_y] = (x, y)
                in_queue.add((next_x, next_y))
                queue.append((next_x, next_y))
        return None
```
### 跨步迷宫
```python
def bfs(start_x, start_y, n, m, maze):
    q = deque()
    q.append((0, start_x, start_y))  # (step, x, y)
    in_queue = {(start_x, start_y)}
    while q:
        step, x, y = q.popleft()
        if x == n - 1 and y == m - 1:
            return step
        for i in range(MAXD):
            next_x = x + dx[i]
            next_y = y + dy[i]
            next_half_x = x + dx[i] // 2
            next_half_y = y + dy[i] // 2
            if  0 <= x < n and 0 <= y < m and maze[x][y] == 0 and (x, y) not in in_queue and 
            maze[next_half_x][next_half_y] == 0:
                in_queue.add((next_x, next_y))
                q.append((step + 1, next_x, next_y))
        return -1
```
### 矩阵中的块
```python
def bfs(x, y):
    q = deque([(x, y)])
    inq_set.add((x,y))
    while q:
        front = q.popleft()
        for i in range(MAXD):
            next_x = front[0] + dx[i]
            next_y = front[1] + dy[i]
            if matrix[next_x][next_y] == 1 and (next_x,next_y) not in inq_set:
                inq_set.add((next_x, next_y))
                q.append((next_x, next_y))
n, m = map(int, input().split())
matrix=[[-1]*(m+2)]+[[-1]+list(map(int,input().split()))+[-1] for i in range(n)]+[[-1]*(m+2)]
inq_set = set()
counter = 0
for i in range(1,n+1):
    for j in range(1,m+1):
        if matrix[i][j] == 1 and (i,j) not in inq_set:
            bfs(i, j)
            counter += 1
print(counter)
```