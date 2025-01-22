# Assignment #2: 语法练习

Updated 0126 GMT+8 Sep 24, 2024

2024 fall, Complied by ==许慧琳 心理与认知科学学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



思路：由于矩阵的大小确定，因此移动到中心的步数也是确定的，通过画图发现当矩阵在第一/第五行是需要2步，而第二和第四行需要1步，对应index恰好等于行数-2的绝对值（矩阵对称，最大值=2），列数同理可以得出



##### 代码

```python
matrix = [[int(x) for x in input().split()] for i in range(5)]
 
for i in range(5):
    if 1 in matrix[i]:
        j = matrix[i].index(1)
        print(abs(i-2)+abs(j-2))
        break

```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-10-04 202212](https://github.com/user-attachments/assets/23839f2c-7d0f-4a8c-8978-7ee2efe80770)



### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A



思路：刚开始的时候用的是暴力解法，后来超时了，之后就用数学的方式想了一下，a/b的余数就是a距离可以被b整除的距离，然后就很快找出了解答



##### 代码

```python
for i in range(int(input())):
    a, b = map(int, input().split())
    res = a % b
    if res == 0:
        print (0)
    else:
        print (b - res)

```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-10-04 203112](https://github.com/user-attachments/assets/f7c53a25-1daf-44e6-858a-8f23ba3436f7)



### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A



思路：按照题目的要求还有提示来编写代码，没有什么特别的思路



##### 代码

```python
n = int(input())
events = [int(x) for x in input().split()]
recruits = 0
crime = 0
for i in range(n):
    if events[i] >= 1:
        recruits += events[i]
    elif events[i] == -1:
        if recruits > 0:
            recruits -= 1
        else:
            crime += 1
print (crime)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-04 203601](https://github.com/user-attachments/assets/aad93b81-cd2d-47ef-b08c-be75feb0f8de)



### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：因为是一维空间，所以直接设置一个list长度为L+1（因为左右两端都要有树，因此树的总数需要+1），当树需要被挖掉时，则说明这个点上面的树没了，由1变为0，由输入的起始和终点距离决定有多少树需要被去除，最后将剩余的数量加起来，其实可以用sum(tree)的但是当时应该是没想到，但是整体复杂度都是linear所以没有太大区别



##### 代码

```python
L, M = map(int, input().split())
tree = [1]*(L+1)
for i in range(M):
    start, stop = map(int, input().split())
    for idx in range(start, stop + 1):
        tree[idx] = 0
left = 0
for i in tree:
    if i == 1:
        left += 1
print(left)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-04 204453](https://github.com/user-attachments/assets/d272773f-1325-445d-8278-1aced9872734)



### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



思路：a, b是题目给予的闭区间，因此range的end需要+1，用map和join split函数把三位数的数字拆成3个个位数数字，而后进行运算，如果相等则append，最后判断是否有水仙花数并print结果



##### 代码

```python
a, b = map(int, input().split())
waterfairy = []
for i in range(a, b+1):
    c, d, e = map(int, ' '.join(str(i)).split())
    if (c**3 + d**3 + e**3) == i:
        waterfairy.append(i)
if len(waterfairy) != 0:
    print(*waterfairy)
else:
    print("NO")

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-04 210635](https://github.com/user-attachments/assets/c32d5391-79f4-4124-9fbd-28088cc3bc9e)



### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



思路：这题花费了很长的时间，但是最后还是没有做出来，最多是卡在怎么去定位两个距离刚好一样的rider，一开始是想用比较mathematical的想法去计算的，但后来发现无论怎样都解释不明白，就算是用了ChatGPT把代码改了一遍之后也还是WA，然后转而想去用+1的方式解决，但是看到时间动辄700起就放弃了，最后实在想不出来就去看答案，然后答案似乎说明速度的data是stricly increase? 但仍然不太理解为什么最少的时间只需要是最快速度的耗时，耗时应该是不一样的才对。然后看了答案之后就想说能不能把它和自己的答案结合一下，但是最后的结果也仍然是错的，彻底放弃挣扎，附自己写的答案一份（第二个；未完成）



##### 代码

```python
import math

while True:
    n = int(input())
    if n == 0:
        break

    max_time = float("inf")
    for _ in range(n):
        speed, time = map(int, input().split())
        if time < 0:
            continue
        arrival_time = math.ceil((4500 / speed) * 3.6 + time)
        max_time = min(max_time, arrival_time)

    print(max_time)

```
```python
from math import ceil
while True:
    num_of_ride = int(input())
    distance = 4500
    riders = []
    if num_of_ride == 0: #当n=0，input结束
        break
    else:
        for i in range(num_of_ride): #输入数据
            Vi, Ti = map(int, input().split())
            Vi *= (1000/3600) #速度的单位从km/h改成m/s
            riders.append([Vi, Ti])
        riders.sort(key = lambda x:x[1])

        time_finished = 0
        current_speed = 0
        distance_covered = 0

        for i in range(len(riders)):
            if riders[i][1]*current_speed > distance:
                break
            current_speed = min(riders[i][1] for i in range(len(riders)) if riders[i][1] > 0)
            met_time = []
            for i in range(len(riders)):
                if riders[i][1]-time_finished > 0:
                    met_time.append((riders[i][1]-time_finished) / riders[i][0])
                else:
                    met_time.append(10000)
            time_finished += min(x for x in met_time if x > 0)
            distance_covered += current_speed * time_finished
            idx = met_time.index(time_finished)
            current_speed = riders[idx][0]

```





代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-05 054050](https://github.com/user-attachments/assets/a1d54a9d-9536-4366-9173-59cebdd7663f)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

目前的情况比较两极分化，不会的就会卡的很久，然后需要花比较多的时间反复在看然后debug，但是其他都一般上都是第一次就accepted了，不然就是有些小bug比如忘记删写代码时用来检查数据输入的对不对的print忘记删掉，或者提交的时候忘记把语言换成python，选做题的进度因为假期了也勉强有多写一点，虽然大部分时间还是被各种论文综述报告占据（不想看articles的时候就看看代码聊以慰籍T^T），目前看来是无望在国庆假期的时候把所有进度追赶上了，但好消息应该是也没有停滞下来吧。另外，这一次作业比上一次多了深刻的感触，那就是AI改的代码页不一定正确，最后还是得要自己再用debugger（有时候效率反而更高）。最后小小许愿一下能在期中前后把进度追上来吧。



