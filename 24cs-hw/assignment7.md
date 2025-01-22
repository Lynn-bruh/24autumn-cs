# Assignment #7: Nov Mock Exam立冬

Updated 1646 GMT+8 Nov 7, 2024

2024 fall, Complied by <mark>许慧琳 心理与认知科学学院</mark>



**说明：**

1）⽉考： AC2 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E07618: 病人排队

sorttings, http://cs101.openjudge.cn/practice/07618/

思路：按照题目的要求，把病人分为两类，其中老年人按年龄顺序由大到小排列，然后就可以得出看诊的顺序了（先打印老年人，再打印年轻人）



代码：

```python
num_pat = int(input())
patients_old = []
patients_young = []
for _ in range(num_pat):
    id, age = map(str, input().split())
    if int(age) >= 60:
        patients_old.append([id, int(age)])
    else:
        patients_young.append([id, int(age)])
patients_old.sort(key = lambda x: x[1], reverse = True)

if patients_old:
    for i in range(len(patients_old)):
        print(patients_old[i][0])
if patients_young:
    for i in range(len(patients_young)):
        print(patients_young[i][0])

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-11 202349.png">



### E23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555/

思路：矩阵乘法要求列数和行数一致，所以遍历两个矩阵，找到里面一样的然后通过更改第三个matrix的对应行列位置的值来对这些数值进行保存，最后打印matrix3里非0的元素



代码：

```python
n, m1, m2 = map(int, input().split())
matrix1 = []
matrix2 = []
for _ in range(m1):
    a, b, c = map(int, input().split())
    matrix1.append([a, b, c])
for _ in range(m2):
    a, b, c = map(int, input().split())
    matrix2.append([a, b, c])

matrix3 = [[0]*n for _ in range(n)]
for i in range(m1):
    for j in range(m2):
        if matrix1[i][1] == matrix2[j][0]:
            matrix3[matrix1[i][0]][matrix2[j][1]] += matrix1[i][2] * matrix2[j][2]

for i in range(n):
    for j in range(n):
        if matrix3[i][j] != 0:
            print(i, j, matrix3[i][j])

```



代码运行截图 ==（至少包含有"Accepted"）==

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-11 202450.png">



### M18182: 打怪兽 

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：一个TLE的解法T^T，求助AI也没用



代码：

```python
nCases = int(input())
for _ in range(nCases):
    n, m, b = map(int, input().split())
    skill = []
    for _ in range(n):
        ti, xi = map(int, input().split())
        skill.append([ti, xi])
    skill.sort(key = lambda x: x[0]) # sort according to time

    dietime = 1 # too slow to iterate one by one
    i = 0
    while i < len(skill):
        if b <= 0:
            print(dietime)
            break

        if skill[i][0] > dietime:
            dietime += 1
        else:
            b -= skill[i][1]
            i += 1
    else:
        if b == 0:
            print(dietime)
        else:
            print("alive")

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M28780: 零钱兑换3

dp, http://cs101.openjudge.cn/practice/28780/

思路：还是不会dp T^T，没有思路



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### T12757: 阿尔法星人翻译官

implementation, http://cs101.openjudge.cn/practice/12757

思路：这题改了挺久的，先是全部混合在一起然后慢慢更改，之后发现会有hundred twenty million这种情况，又改了一下，然后发现还有可能存在hundred million，才想到要把million，thousand和hundred分开来进行运算，最后就是先将函数拆开，之后分段转换，最终加起来print；def函数原来是分情况讨论的，但后来被AI简化了一下就变得更加美观了



代码：

```python
eng_to_num = {
    'negative': -1, 'zero': 0, 'one': 1, 'two': 2, 'three': 3, 'four': 4, 'five': 5, 'six': 6, 'seven': 7, 'eight': 8, 'nine': 9, 
    'ten': 10, 'eleven': 11, 'twelve': 12, 'thirteen': 13, 'fourteen': 14, 'fifteen': 15, 'sixteen': 16, 'seventeen': 17, 'eighteen': 18, 
    'nineteen': 19, "twenty": 20, 'thirty': 30, 'forty': 40, 'fifty': 50, 'sixty': 60, 'seventy': 70, 'eighty': 80, 'ninety': 90, 
    'hundred': 100, "thousand": 1000, 'million': 1000000
    }

def transfer_eng_to_num(give_a_list):
    if not give_a_list:
        return 0
    
    total = 0 # for value > 1000
    current = 0 # for value < 1000
    
    for word in give_a_list:
        value = eng_to_num[word]
        if value == 100: 
            current *= value
        elif value == 1000 or value == 1000000:
            total += current * value 
            current = 0 # reset
        else:
            current += value
    
    return total + current

eng = input().split()
is_negative = False
if eng[0] == 'negative':
    is_negative = True
    eng = eng[1:]  # Remove negative from the list for processing

millions = []
thousands = []
hundreds = []
left = []

temp = 0
# Segment into millions, thousands, hundreds, and the rest
for i in range(len(eng)):
    if eng[i] == 'million':
        millions = eng[0:i+1]
        temp = i + 1
        break
for i in range(temp, len(eng)):
    if eng[i] == 'thousand':
        thousands = eng[temp:i+1]
        temp = i + 1
        break
for i in range(temp, len(eng)):
    if eng[i] == 'hundred':
        hundreds = eng[temp:i+1]
        temp = i + 1
        break
left = eng[temp:]

numer = transfer_eng_to_num(millions) + transfer_eng_to_num(thousands) + transfer_eng_to_num(hundreds) + transfer_eng_to_num(left)
if is_negative:
    numer = -numer
print(numer)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-11 202525.png">



### T16528: 充实的寒假生活

greedy/dp, cs10117 Final Exam, http://cs101.openjudge.cn/practice/16528/

思路：不会用dp的方法，greedy的话就是首先先对活动进行排序，要求参加尽可能多的活动，而活动的开始时间最多会和结束时间同时，因此按照结束时间来排列的话，就可以找到每个开始中（如果有同一天开始的话）最早结束的，而后判断是否下一个活动的开始时间在上一个活动结束之后，然后就可以得出结果了



代码：

```python
event = int(input())
events = []
for _ in range(event):
    start, over = map(int, input().split())
    if over <= 60:
        events.append([start, over])

events.sort(key = lambda x: x[1]) # sort depending on the over time, over earlier, participate more
last_end = -1 # the minimum is 0, so set a primary -1
event_engage = 0

for start, over in events:
    if start > last_end: # last one finished, so can participate next
        event_engage += 1
        last_end = over

print(event_engage)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

<img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-11-11 202557.png">



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

月考那天因为在赶着写实验报告的原因没有去，后来自己设了两个小时的闹钟最后发现也只能AC两题最简单的，加上一题TLE，后面花了几个小时的时间在改进翻译的那道题，然后写寒假生活，然后到现在Medium的两题也还没解决OAO，在考试的时候TLE真的会难受，完全没办法两个小时里面就推出最好的解法来，完全就是大脑里的代码语言资料库不够用，还得多学习学习，然后dp的话还是很难有思路，还得多练和dp相关的思路培养这种思维模式，这大概就是接下来的目标了



