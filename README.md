# --10-
美团算法笔试第二题（2019.09.11）

## 题目大概意思是：
    1.有n个箱子
    2.每个箱子装了ai份外卖，每份外卖大小相同
    3.每个箱子的最大容量是装bi份外卖
    4.每次转移一份外卖花费的时间都是1秒
    求用最短的时间b装最少的箱子k

## 解题思路：
### 1.求的最少用多少个箱子
### 求出总的外卖数量suma->对所有箱子的容量进行逆排序，最大的k个箱子，容量刚好等于或者刚超过外卖总量suma
### 2.求最少的时间t，也就是k个箱子容量总和满足条件的情况下，最少转移时间
### 目标：使选出的k个箱子中已经装的外卖最多，不足的就是转移时间b
    
    n=int(input())
    ai=list(map(int,input().split()))
    bi=list(map(int,input().split()))

    ba=[]
    for i in range(n):
        ba.append([bi[i],ai[i]])
    ba.sort(reverse=True)

    suma=sum(ai)
    sumkb=0
    sumka=0
    for i in range(n):
        if sumkb>=suma:
            k=i
            break
        sumkb+=ba[i][0]
        sumka+=ba[i][1]

    def func(cura,curb,ba,k,sumka):
        if k==0 or len(ba)<k:
            return sumka
        temp=[ba[i][1] for i in range(k)]
        if sum(temp)+curb<sumka:
            return sumka
        if k==1 and sum(temp)+curb>=sumka:
            return max([cura+ba[0][1],sumka,func(cura,curb,ba[1:],k,sumka)])
        return max(func(cura+ba[0][1],curb+ba[0][0],ba[1:],k-1,sumka),func(cura,curb,ba[1:],k,sumka))

    res=func(0,0,ba,k,sumka)
    print(k,suma-res)
