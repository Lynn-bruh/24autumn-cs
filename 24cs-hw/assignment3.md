# Assign #3: Oct Mock Exam暨选做题目满百

Updated 1537 GMT+8 Oct 10, 2024

2024 fall, Complied by 许慧琳 心理与认知科学学院



**说明：**

1）Oct⽉考： ==AC5== 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/



思路：这题其实比较简单，就是一开始以为偏移是向后偏移，之后输入样例之后发现其实是想前移动所以后来改了一下，花的时间并不长，然后一开始本来想用字典序的，但是后来发现没把对应的代码记上，只能用最传统的方法了



代码

```python
k = int(input())
s = input()
key = ' '.join("abcdefghijklmnopqrstuvwxyz").split()
Key = ' '.join("ABCDEFGHIJKLMNOPQRSTUVWXYZ").split()
passout = ""
for char in s:
    if char in key:
        idx = key.index(char) - k
        while idx < 0:
            idx += 26
        passout += key[idx]
    if char in Key:
        idx = Key.index(char) - k
        while idx < 0:
            idx += 26
        passout += Key[idx]
print (passout)

```



代码运行截图 ==（至少包含有"Accepted"）==

<div align = center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-11 235422.png"></div>



### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



思路：这题应该不用多说，略



代码

```python
A = input()
a = int(A[0:2]) + int(A[4:6])
print (a)

```



代码运行截图 ==（至少包含有"Accepted"）==

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-11 235515.png"></center>



### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



思路：这题也不会很难，按照题目的要求一步一步implement就可以了，然后需要注意的就是最后realcode和输入给予的code的比较，因为里面包含一个string所以比较的事后ic的最后一位数也需要转换成string才能正确比较



代码

```python
n = int(input())
for _ in range(n):
    ic = input()
    icnumbers = ic[:-1]
    icnumbers = list(map(int, ' '.join(icnumbers).split()))
    code = [7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]
    adding = 0
    for i in range(17):
        adding += icnumbers[i] * code[i]
    mods = adding % 11
    realcode = ['1','0','X','9','8','7','6','5','4','3','2']
    if realcode[mods] == str(ic[-1]):
        print("YES")
    else:
        print("NO")

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-11 235544.png"></center>



### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



思路：这题其实选做题好像有类似的，所以没花多少精力就做出来了，还用了做那道题的时候才学会的format()函数



代码

```python
n = int(input())
while True:
    a = n
    if n == 1:
        print("End")
        break
    elif n % 2 != 0:
        a = n * 3 + 1
        print("{}*3+1={}".format(n, a))
        n = a
    elif n % 2 == 0:
        a = n // 2
        print("{}/2={}".format(n, a))
        n = a

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-11 235620.png", width = 90%></center>



### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/



思路：这道题除了打字的时间废了比较长时间，但是实际上并没有花太多的脑细胞，比较困难的可能是数字转罗马数字的顺序问题，一开始是想要分情况来，就是四位数、三位数……的数字分情况来转换，但是后来发现其实重复的代码挺多，而且情况只要从数字长度大到小来判断的话罗马数字加入getrome变量里面的顺序就不会乱，最后就改成了现在这样的代码，然后从罗马数字到阿拉伯数字的就是算出现频率然后判断是否有例外最后做乘法和加法就完成了



##### 代码

```python
n = input()
numtest = ["1", '2', '3', '4', '5', '6', '7', '8', '9']
getrome = ""
getnum = 0
if n[0] in numtest:
    n = list(map(int, " ".join(n).split()))
    if len(n) == 4:
        getrome += "M"*n[0]
    
    if len(n) > 2:
        if n[-3] < 4:
            getrome += "C"*n[-3]
        elif n[-3] == 4:
            getrome += "CD"
        elif 4 < n[-3] < 9:
            getrome += "D" + "C"*(n[-3]-5)
        elif n[-3] == 9:
            getrome += "CM"
        else:
            print("HAHA")
    
    if len(n) > 1:
        if n[-2] < 4:
            getrome += "X"*n[-2]
        elif n[-2] == 4:
            getrome += "XL"
        elif 4 < n[-2] < 9:
            getrome += "L" + "X"*(n[-2]-5)
        elif n[-2] == 9:
            getrome += "XC"
        else:
            print("HA")
    
    if n[-1] < 4:
        getrome += "I"*n[-1]
    elif n[-1] == 4:
        getrome += "IV"
    elif 4 < n[-1] < 9:
        getrome += "V" + "I"*(n[-1]-5)
    elif n[-1] == 9:
        getrome += "IX"
    else:
        print("H")
    
    print(getrome)

else:
    countM = n.count("M")
    countD = n.count("D")
    countC = n.count("C")
    countL = n.count("L")
    countX = n.count("X")
    countV = n.count("V")
    countI = n.count("I")
    if "CM" in n:
        countM -= 1
        countC -= 1
        getnum += 900
    if "CD" in n:
        countD -= 1
        countC -= 1
        getnum += 400
    if "XC" in n:
        countC -= 1
        countX -= 1
        getnum += 90
    if "XL" in n:
        countL -= 1
        countX -= 1
        getnum += 40
    if "IX" in n:
        countX -= 1
        countI -= 1
        getnum += 9
    if "IV" in n:
        countV -= 1
        countI -= 1
        getnum += 4
    getnum += countM*1000 + countD*500 + countC*100 + countL*50 + countX*10 + countV*5 + countI*1
    print (getnum)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

<center><img src = "C:\Users\Lynn\Pictures\Screenshots\Screenshot 2024-10-11 235710.png", width = 90%></center>



### *T25353: 排队 （选做）

http://cs101.openjudge.cn/practice/25353/



思路：这题其实没做出来，题目也有点没完全想明白，有点头绪但不多，但是附上未完成的代码，等过一段时间把文献综述写完了再去看一下群聊的思路



代码

```python
N, D = map(int, input().split())
height = []
for _ in range(N):
    height.append(input())

height_pre = ["0"]*N
mheight = sorted(height)
while True:
    if mheight == height:
        height_pre = height
        break
    
    for i in range(1, len(height)):
        if abs(int(height[i-1]) - int(height[i])) > D:
            continue
        elif (int(height[i-1]) - int(height[i])) <= 0:
            continue
        elif 0 < int(height[i-1]) - int(height[i]) <= D:
            height[i-1], height[i] = height[i], height[i-1]
    
    if "0" not in height_pre and height_pre <= height:
        break

    height_pre = height

print (*height_pre, sep = '\n')

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

这次月考其实忘了记上OJ的密码，然后全程看着题目只能用测试案例来判断结果对不对，幸好OJ不用登入也可以看题目，就是不能提交答案，但是也给了一个小教训，以后记得把密码存手机上。选做题的话目前卡在了装箱问题上，感觉对题目的理解可能存在歧义，因为后来想到了另一种可能性，但是还没去实施，总体来说现在还能跟得上选做题的难度。





