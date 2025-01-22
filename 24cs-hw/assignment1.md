# Assignment #1: 自主学习

Updated 0110 GMT+8 Sep 10, 2024

2024 fall, Complied by ==许慧琳 心理与认知科学学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02733: 判断闰年

http://cs101.openjudge.cn/practice/02733/



思路：按照网站上面说的提示，先判断能不能被四整除，然后在再判断能被100整除而不能被400整除，以及能被3200整除这两种情况，排除之后剩下的都是闰年



##### 代码

```python
a = int(input())
if a%4 == 0:
    if (a%100 == 0 and a%400 !=0) or a%3200 == 0:
        print ("N")
    else:
        print ("Y")
else: 
    print ("N")
```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-09-20 155443](https://github.com/user-attachments/assets/432abb54-8c40-49e3-ac5a-80ee82cad605)



### 02750: 鸡兔同笼

http://cs101.openjudge.cn/practice/02750/



思路：鸡有两只脚，兔子有4只脚，所以脚数不可能为奇数，直接打印0 0，在其余可能性都是偶数的情况下，最多的动物总全是鸡，最少的总全是兔子，因此先考虑同时可以被2和4整除的情况下，这时候兔子最多是最少的动物数量，计算a/4，鸡则是maximum，另一种情况则是无法被4整除，这时候最少的动物数量除了有兔子还会有一只鸡（即余数肯定为2），因此以4整除a后+1即是最后的答案



##### 代码

```python
a = int(input())
if a%2 == 0 and a%4 == 0:
    print (int(a/4), int(a/2))
elif a%2 == 0 and a%4 != 0:
    print (int((a//4)+1), int(a/2))
else: 
    print (0, 0)
```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-09-20 160426](https://github.com/user-attachments/assets/0edd3a97-190b-44e5-b9c3-ffe12acac3d2)



### 50A. Domino piling

greedy, math, 800, http://codeforces.com/problemset/problem/50/A



思路：这题其实是问了chat gpt怎么找，由chat gpt给出解答之后直接打成代码就完成了，因为刚开始没想出来要用面积



##### 代码

```python
M, N = map (int, input().split())
print ((M*N)//2)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-09-20 161636](https://github.com/user-attachments/assets/60ec3645-355c-4cf8-8d46-e592bee58d8d)



### 1A. Theatre Square

math, 1000, https://codeforces.com/problemset/problem/1/A



思路：这题其实和50A类似，都是计算面积，但是情况和50A相反，1A填充的面积可以超过地板的面积，所以除了计算填充每一行或每一列各需要填充多少个flagstone之外，当长/宽不能被flagstone的长/宽整除时，就是它不能刚好填满一整行/列时，每行/每列的flagstone数量就需要+1，最后直接相乘得出总数



##### 代码

```python
n, m, a = map (int, input().split())
x = m//a
if m%a != 0:
    x += 1
y = n//a
if n%a != 0:
    y += 1
print (x*y)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-09-20 162516](https://github.com/user-attachments/assets/1814ecc8-5eff-417c-b888-6077ade380c1)



### 112A. Petya and Strings

implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A



思路：这题一开始想的是使用index来比较，然后才发现原来还有ord()，才知道原来直接string可以直接比较顺序，然后误会了以为是比较总体的大小之后才能决定谁大谁小，因为一直到test 4这两个获得的答案都是一样的，之后才发现原来只要有第一个字母有大小区分就可以直接判断出来了，所以使用break，当有大小区分的事后就直接推出loop print answer



##### 代码

```python
str1 = ' '.join(input().lower()).split()
str2 = ' '.join(input().lower()).split()
comp = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']

while str1 == str2:
      print(0)
      break
else:
    for i in range(len(str1)):
        if comp.index(str1[i]) > comp.index(str2[i]):
            print(1)
            break
        elif comp.index(str1[i]) < comp.index(str2[i]):
            print(-1)
            break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-09-20 163147](https://github.com/user-attachments/assets/1cca8d0e-c58f-4946-a636-6e3cffba69fb)



### 231A. Team

bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A



思路：使用list把三人的情况输入然后直接用sum函数，当大于1，就是至少有两人会做的时候，就把最终会写solution的数量+1



##### 代码

```python
n = int(input())
num = 0
for i in range(n):
    a = [int(x) for x in input().split()] 
    if sum(a) > 1: 
        num += 1
print(num)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-09-20 164846](https://github.com/user-attachments/assets/cd396400-cbe1-4298-8e9a-a2e9bdf123ce)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

其实还挺担心计概的，上学年劝退了之后趁着暑假先学了一点基本语法，结果开始做题也卡了很久（虽然之后才发现是input和output文字的问题），目前已经把所有800的题目做完了，同时也收获了很多新的知识比如split()、map()函数等，然后通过看其他人写的代码也能找到一些更简单的思路，前几天刚进入900难度又卡了许久（一直time limit exceed TT），目前一题都还没做出来（似乎），原来打算一天做3题的但是后来就大部分时间就被实验报告占据了，希望接下来找文献顺利，花多一点时间在练习代码上。



