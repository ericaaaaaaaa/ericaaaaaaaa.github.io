---
layout: post
title: Discrete Mathematics
subtitle: 离散数学课程笔记
categories: Mathematics
tags: discrete-mathematics math note graph-theory
---

# 图论

## 基本概念

### 绘图程序

```python
import networkx as nx
import matplotlib.pyplot as plt

def drawgraph(E):
#    G = nx.Graph() # 无向图
    G = nx.DiGraph() # 有向图
    G.add_edges_from(E)
    nx.draw(G, node_size = 200, node_color = 'k', 
            with_lables = True, font_color = 'w')
    plt.show()
    return

V = {'a', 'b', 'c', 'd', 'e', 'f', 'g'}
E = [('a', 'b'), ('a', 'f'), ('b', 'c'), ('b', 'e'), ('b', 'f'), ('c', 'd'),
     ('c', 'e'), ('c', 'f'), ('f', 'e')]
drawgraph(E)
```

![image-20210528102258319](/assets/images/post/image-20210528102258319.png)

![image-20210528102310190](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210528102310190.png)



### 基本概念

#### 无序偶和有序偶

##### 无序偶

$V$ 是非空集合，$x\in V$ 并且 $y\in V$，称 $(x,y)$ 是无序偶

##### 有序偶

$V$ 是非空集合，$x\in V$ 并且 $y\in V$，称 $<x,y>$ 是有序偶

#### 有向图和无向图

##### 无向图

无向图是由无序偶构成的边的集合

```python
def Cartesianproduct(X, Y): # 笛卡尔乘积
    XY = set({})
    for x in X:
        for y in Y:
            XY.add((x, y))
    return XY

def creategraph(m, n): # 随机生成 m 点, n 边图
    global V, XY
    V = range(m)
    XY = Cartesianproduct(V, V)
    E = random.sample(XY, n)
    return [V, E]
```

##### 有向图

有向图是由有序偶构成的边的集合

#### 带权图

```python
def Cartesianproductweight(X, Y): # 带权笛卡尔乘积
    XY = set({})
    for x in X:
        for y in Y:
            weight = random.randint(1, 100)
            XY.add((x, y, weight))
    return XY

def weightedgraph(V0, E0, W0): # 随机生成带权图
    V = V0
    E = set({})
    for (u, v) in E0:
        w.random.randint(1, W0)
        E = E | {(w, u, v)}
    return [V, E]
```

#### 图判断 

```python
def isgraph(V, E): # 是否为图
    tv = True
    for (u, v) in E:
        tv = tv and (u in V) and (v in V)
    return tv
```

### 基本结构

#### 关系

**图的基本结构**是指图和顶点之间，边之间以及边与顶点之间的连接关系

* **邻接关系**：顶点之间
* **关联关系**：顶点与边之间
* **相邻关系**：边与边之间

#### 度

##### 入度 $di(u)$

以 $u$ 为终点的边数称为点 $u$ 的入度。

##### 出度 $do(u)$

以 $u$ 为起点的边数称为点 $u$ 的入度。

##### 度 $deg(u)$

###### 顶点的度

$deg(u) = di(u) + do(u)$

###### 图的度

$deg(V) = \sum_{u\in V}deg(u)$

#### 握手定理

设 $G = <V, E>$ 是 $(n, m)$ 图，则 $\sum_{u\in V}d(u) = 2m$

#### 同构 (isomorphism)

设无向图 $G = <V, E>$ 和 $G' = <V', E'>$, 如果存在**双射函数** $f: v\rightarrow v'$, 并且当且仅当 $e = (u_i, u_j)\in E$, 有 $e' = (f(u_i), f(u_j))\in E'$, 则称 $G$ 和 $G'$ **同构**

### 子图及算法

#### 子图 (subgraph)

##### 定义

设 $G = <V, E>$ 和 $G_s = <V_s, E_s>$ 是两个图，若 

* $V_s \subseteq V$
* $E_s \subseteq E$

则称 $G_s$ 为 $G$ 的子图，记为 $G_s \subseteq G$

##### 判断子图算法

```python
def issubgraph(V, E, Vs, Es):
    tv = (Vs <= V) and (Es <= E)
    return tv
```

#### 真子图 (proper subgraph)

##### 定义

* $G_s \subseteq G$
* $V_s \subset V$
* **或**
* $E_s \subset E$

则称 $G_s$ 为 $G$ 的真子图，记为 $G_s \subset G$

##### 判断真子图算法

```python
def ispropersubgraph(V, E, Vs, Es):
    tv = (Vs <= V) and (Es <= E) or ((Vs < V) or (Es < E))
    return tv
```

#### 生成子图 (spanning subgraph)

##### 定义

* $G_s \subseteq G$
* $V_s = V$

则称 $G_s$ 为 $G$ 的生成子图

##### 判断生成子图算法

```python
def isspanningsubgraph(V, E, Vs, Es):
    tv = (Vs <= V) and (Es <= E) and ((Vs == V) and (Es <= E))
    return tv
```

#### 导出子图 (induced subgraph)

##### 定义

* $G_s \subseteq G$
* `\forall (v; v in V_s; \forall (u; u in V_s; (u, v) in E ==> (u, v) in E_s))`

则称 $G_s$ 为 $G$ 的导出子图

##### 判断导出子图算法

```python
def isinducedsubgraph(V, E, Vs, Es):
    tv = (Vs <= V) and (Es <= E)
    for (u, v) in E:
        tv = tv and ((not((u in Vs) and (v in Vs))) or ((u, v) in Es))
    return tv
```

### 特殊图及算法

#### 补图

设 $G = <V, E>$ 是 $n$ 阶无向简单图

* 以 $V$ 为顶点
* 以所有使 $G$ 称为 **完全图** $K_n$ 的添加边所组成的集合为边

的图称为 $G$ 的子图，记为 $\sim G$

#### 相对补图

$G_1$ 是 $G_2$ 相对于 $G$ 的补图

## 连通问题

### 连通问题

#### 通路 (path)

若 $u_k\in V, (u_k, u_{k+1}) \in E, k = 0, 1,...,n-1$，则顶点 $u_0$ 与顶点 $u_{n-1}$ 之间存在通路，记为 $u_0\Rightarrow u_{n-1}$

#### ==简单==通路 (simple path)

每条 **边** 的出现不超过一次

#### ==基本==通路 (basic path)

每个 **顶点** 的出现不超过一次

#### 回路 (circuit)

设有向图 $D = <V,A>$，若 $u\in V$，从 $u$ 出发并返回 $u$ 的通路，称为回路，记为 $u\Rightarrow u$

#### 简单回路 (simple circuit)

每条 **边** 的出现不超过一次

#### 基本回路 (basic circuit)

每个 **顶点** 的出现不超过一次

#### 圈 (acyclic)

#### 定理

设 $G = <V, E>$ 是 $n$ 阶图，若顶点 $u, v$ 存在通路，则存在小于等于 $n-1$ 的通路

### 通路算法

##### 初始通路

$path0 = \{(u0, v) | (u0, v)\in E\or (v, u0)\in E\}$

```python
def pathset0(V, E, u0):
    # 找到所有从 u0 出发的路径
    path1 = set({})
    for (u, v) in E:
        if (u == u0):
            path1 = path1 | {(u, v)}
    return path1
```

$(\omega, x, y) \in path(n) \rightarrow (\omega,x,y,v)\in path(n+1)$

* $u == y\and v\ not\ in\ path(n) \rightarrow(\omega, x,y,v)\in path(n+1) $
* $v == y\and v\ not\ in\ path(n) \rightarrow(\omega, x,y,u)\in path(n+1) $

```python
def pathset(path0, E):
    path = set({})
    for p0 in path0:
        y = p0[-1]
        for (u, v) in E:
            if u == v:
                continue
            p = p0
            if (u == y):
                p = p + tuple([v])
                path = path | {p}
    return path
```

```python
def basicpathset(path0, E):
    path = set({})
    for pk in path0:
        x = pk[-1]
        for (u, v) in E:
            if u == v:
                continue
            p = pk
            if (u == x and (v not in pk)):
                p = p + tuple([v])
                path = path | {p}
    return path
```

### 连通图

#### 连通图的构建

```python
def connectedgraph(V, E, V0, E0):
    # 连通图
    Vc = V0
    Ec = E0
    while E != set({}):
        n = len(Ec)
        for (u, v) in E:
            if (u, v) not in Ec and (u in Vc or v in Vc):
                Vc = Vc | {u, v}
                Ec = Ec | {(u, v)}
        if (len(Ec) == n):
            break
        return [Vc, Ec]
```

#### 连通图判断

[Vc, Ec] 是图 G = <V, E> 的连通子图，若 Ec == E，则 G 为连通图，否则不然

```python
def isconnectedgraph(V, E):
    # 判断是否为连通图
    V = list(V)
    E = list(E)
    (u, v) = E[0]
    V0 = {u, v}
    E0 = {(U, v)}
    [Vc, Ec] = connectedgraph(V, E, V0, E0)
    tv = (set(V) == set(Vc)) & (set(Ec) == set(E))
    return tv
```

## 图的矩阵表示

### 邻接矩阵

设$G=<V,E>$是简单图，$|V|=n$，矩阵$An\times n=[ai,j]$，若边$(vi,vj)\in E$，则$a_{i,j}=1$，否则，$a_{i,j}=0$，称矩阵$An\times n$是邻接矩阵。

```python
def graph2array(V, E):
    n = len(V)
    A = np.zeros((n, n), dtype = 'int')
    A = np.array(A)
    for i in range(n):
        for j in range(n):
            if (i, j) in E:
                A[i, j] = 1
    return A
```

### 多重边图邻接矩阵

邻接矩阵$An×n=[ai,j]$，也可以表示有自环以及有多重边的图，其中，$a_{i,j}$表示顶点$v_i$和$v_j$邻接的边数目。

### 有向图邻接矩阵

邻接矩阵也可以表示有向图。矩阵$An×n=[a_{i,j}]$，若边$<v_i,v_j>\in E$，则$a_{i,j}=1$，否则，$a_{i,j}=0$。

### 无向图关联矩阵

设$G=<V,E>$是简单无向图，$|V|=n$，$|E|=m$，矩阵$An×m=[a_{i,j}]$，若顶点$v_i\in V$，$e_j\in E$，$e_j=(v_i,u)$或$e_j=(u,v_i)$，则$a_{i,j}=1$，否则，$a_{i,j}=0$，称矩阵$A_n×m$是关联矩阵。

### 有向图的关联矩阵

设$G=<V,E>$是简单有向图，$|V|=n$，$|E|=m$，矩阵$A_n×m=[a_{i,j}]$，顶点$u_i\in V$，$e_j\in E$，若$e_j=<u_i,v>$则$a_{i,j}=1$，若$e_j=<v,u_i>$，则$a_{i,j}=-1$，否则，$a_{i,j}=0$，称矩阵$A_n×m$是关联矩阵。

### 可达矩阵

设A是相邻矩阵，I是单位矩阵，B是布尔矩阵，R是可达矩阵，则$R=B(I+A+A^2+…+A^{n-1})=B[(I+A)^{n-1}]$

```python
def reachabilityarray(A, n):
    # 可达矩阵
    m = len(A)
    I = np.array(np.eye(m, dtype = 'int')) # 自身可达
    Ai = I + A
    An = Ai ** (n-1)
    R = np.zeros((m ,m), dtype = 'int')
    for i in range(m):
        for j in range(m):
            if An[i, j] != 0:
                R[i, j] = 1
            else:
                R[i, j] = 0
    return R
```

## 穿程问题

### 欧拉图

#### 欧拉链

（欧拉链、欧拉圈、欧拉图）：设G＝<V,E>是连通无向图，经过G中每条**边**一次且仅一次的非闭合链称为欧拉链（通路）

#### 欧拉圈

经过G中每条边一次且仅一次的闭合链称为欧拉圈

#### 欧拉图

具有欧拉圈的图称为欧拉图

#### 判定定理

##### 定理1

设G是无向连通图，则如下三个命题等价

* G是欧拉图。
* G中所有顶点的度数是偶数。
* G是若干不重边圈的并。

##### 定理2

无向图G为欧拉图，当且仅当G是连通图，并且G的每一个顶点都是偶顶点。

#### 图的度、入度、出度

```python
def degreeset(V, E):
    # 获得图的度，入度，出度
    V = sorted(V)
    m = len(V)
    d = [0] * m  # 度
    di = [0] * m # 入度
    do = [0] * m # 出度
    for (u, v) in E:
        i = V.index(u)
        j = V.index(v)
        di[j] += 1
        do[i] += 1
        d[i] += 1
        d[j] += 1
    return [d, di, do]
```

#### 欧拉通路

无向图G中有连接$u_i$和$u_j$的欧拉通路，当且仅当G是连通图，并且G中只有$u_i$和$u_j$ 是奇顶点。

#### 欧拉图的构建方法

```python
# 欧拉图的构建算法
def subEulercircuit(E,v0):
    # 返回边 E 中以 v0 作为起点和终点的回路 circuit
    # 返回回路中所经过的点集 S
    circuit = tuple([v0])
    S = set({})
    while (circuit[0] != circuit[-1] or len(circuit) == 1):
        y = circuit[-1]
        for (u, v) in E:
            if(u == y):
                circuit = circuit + tuple([v])
                E = E - {(u, v)}
                S = S | {(u, v)}
                break
    return [circuit, S]

def set2V(E):
    # edges to vertexes
    V = set({})
    for (u, v) in E:
        V = V | {u, v}
    return V

def Eulercircuit(E,v0):
    [circuit, S] = subEulercircuit(E, v0)
    E = E - S
    while (E != set({})):
        V1 = set(circuit)
        V2 = set2V(E)
        V1V2 = V1 & V2
        for v0 in V1V2:
            [subcircuit, S] = subEulercircuit(E, v0)
            if S == set({}):
                # 在现有边集 E 中无法找到 v0 开头的回路
                continue
            k = circuit.index(v0)
            # 将找到的回路添加到原回路的适当位置
            circuit = circuit[0:k] + subcircuit + circuit[k+1:-1] + tuple([circuit[-1]])
            E = E - S
            break
    return circuit
```

#### 一笔画问题（格雷码）

#### 构建偶数度顶点图

```python
# 构建偶数度顶点图
# 对于圈图，任取顶点 [u0, v0]，用 v0 替换 u0，并且 V = V - {u0}
# 重复 n 次
def replacevertex(V0,E0,n):
    V = set(V0)
    E = set({})
    for k in range(n):
        [u0, v0] = random.sample(V, 2)
        V = V - {u0}
        E = set({})
        for (u, v) in E0:
            if u == u0:
                E = E | {(v0, v)}
            elif v == u0:
                E = E | {(u, v0)}
            else:
                E = E | {(u, v)}
        E0 = E
    return [V, E]
```

### 哈密尔顿图

#### 哈密尔顿图

无向图G中穿过每个**顶点**一次且仅一次的圈，称为哈密顿圈,具有哈密顿圈的图称为哈密顿图。

#### 哈密尔顿通路

无向图G中穿过每个顶点一次且仅一次的非闭合链，称为哈密顿链. 有向图D中穿过每个顶点一次且仅一次的非闭合通路，称为哈密顿通路。

#### 定理

* 设G是具有n个顶点的连通无向图。若G中每一对顶点的次数之和大于或等于n-1，则G中存在一条哈密顿链。
* 若G中每一对顶点的次数之和大于或等于n，则G中存在一条哈密顿圈。
* 在一个有向完全图中必存在一条哈密顿通路。

#### 构建邻接表

```python
def adjacentlist(V,E):
    # 返回与美国顶点直接相连的点集
    Ea = []
    for w in V:
        e0 = [w]
        e1 = []
        for (u, v) in E:
            if u == w and (v not in e1):
                e1 = e1 + [v]
            if v == w and (u not in e1):
                e1 = e1 + [u]
        e0 = e0 + [sorted(e1)]
        Ea = Ea + [e0]
    return sorted(Ea)
```

#### 哈密尔顿图周游算法

**程序tourpath0算法：**
（1）取路径path的末尾顶点path[-1]为w。
（2）取顶点w的邻接表E2。
（3）若邻接表E2中顶点u不在path上，则u加入path，否则，返回。

```python
def tourpath0(V, E, path, m):
    while(len(path) < m):
        w = path[-1]
        i = V.index(w)
        E1 = E[i]
        E2 = E1[1]
        for u in E2:
            if(u not in path):
                path.append(u)
                break
        if(path[-1] == w):
            break
    return path
```

**程序tourpath1算法：**
（1）取路径path的末尾顶点v=path.pop( )为v。
（2）若 path非空，取路径path的末尾顶点path[-1]为u。
（3）从顶点u的邻接表从顶点v以后依次检查k=E2.index(v)。
（4）若v=E2[k]不在path，则v为path的新末尾顶点，直至邻接表结束。
（5）若path有新顶点，则结束，否则，path弹出末尾顶点。

```python
def tourpath1(V, E, path, m):
    v = path.pop( )
    while(len(path) != 0):
        u = path[-1]
        i = V.index(u)
        E1 = E[i]
        E2 = E1[1]
        k = E2.index(v)
        while(k < (len(E2) - 1)):
            k = k + 1
            v = E2[k]
            if(v not in path):
                path.append(v)
                break
        if(u != path[-1]):
            break
        v = path.pop( )
    return path
```

**程序tourpath算法：**
（1）若len(path) == m，则求新的周游通路，即tourpath1(V,E,path,m)。
（2）若len(path) != 0，依次迭代执行tourpath0与tourpath1。

```python
def tourpath(V, E, path, m):
    if(len(path) == m):
        path = tourpath1(V, E, path, m)
    while(len(path) != 0):
        path = tourpath0(V, E, path, m)
        if(len(path) == m):
            break
        path = tourpath1(V, E, path, m)
    return path
```

#### 哈密尔顿图算法

* 构建邻接表
* 设置初始顶点 v0
* 搜寻一条路径
* 若 `len(path) == len(V)`，则搜寻下一条路径

```python
def Hamiltonpath(V, E, v0):
    global Ea, paths
    V = sorted(V)
    Ea = adjacentlist(V, E)
    paths = set({})
    path = [v0]
    m = len(V)
    path = tourpath(V, Ea, path, m)
    paths = paths | {tuple(path)}
    while(len(path) == m):
        path = tourpath(V, Ea, path, m)
        if len(path) == m:
            paths = paths | {tuple(path)}
            print(path)
    return paths
```

#### 彼得森图 (Pertersen Graph)

![image-20210617090501213](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210617090501213.png)

#### 完全图哈密尔顿圈

> 完全图G=<V,E>非重复边的哈密尔顿圈数为多少？

### 骑士周游问题

```python
def Knighttouregraph(n):
    P8=[[-1,-2],[-2,-1],[-2,1],[-1,2],[1,2],[2,1],[2,-1],[1,-2]]
    N = n * n
    V = list(range(N))
    E = set({})
    for k in range(N):
        j = k % n
        i = int(k / n)
        for [u, v] in P8:
            if 0 <= (i + u) and (i + u) < n and 0 <= (j + v) and (j + v) < n:
                E = E | {(k,(i + u) * n + (j + v))}
    return [V, E]
```

> 骑士周游图是欧拉图吗？

### 完全图哈密尔顿圈

* 完全图有哈密尔顿圈
* 寻找不重复的哈密尔顿圈
* 完全图的哈密尔顿圈数为 `int(len(v)/2)`

### 旅行商问题

## 通路问题

### 最短路径

在带权有向图G中，给定一个称为始点的顶点u和一个称为终点的顶点v，如果P是从u到v的通路中**权值最小的通路**，则称P为从u到v的最短通路。

#### Djikstra 最短通路算法

```python
def shortpath(V, E, di, Hx, path, zn):
    V = sorted(V)
    Pm = []
    for p in path:
        vn = p[-1]
        if vn == zn:
            Pm = Pm + [p]
            continue
        if di[V.index(vn)] != 0:
            Pm = Pm + [p]
            continue
        for (w, u, v) in E:
            if u == vn:
                kv = V.index(v)
                if di[kv] > 0:
                    di[kv] = di[kv] - 1
                vw = p[0] + w
                if vw <= Hx[kv]:
                    Hx[kv] = vw
                    e = [vw] + p[1:] +[v]
                    Pm = Pm +[e]
    return [di, Hx, Pm]

def shortestpath(V, E, v0, vn):
    V = sorted(V)
    [d, di, do] = degreesetw(V, E)
    Pm0 = []
    Pm = [[0, v0]]
    inf = 10000
    Hx = [inf for x in range(len(V))]
    Hx[0] = 0
    while Pm0 != Pm:
        Pm0 = Pm
        [di, Hx, Pm] = shortpath(V, E, di, Hx, Pm0, vn)
    Pm = sorted(Pm)
    return [Hx, Pm]
```

### 关键路径

#### 最早完成时间算法

#### 最晚完成时间算法

```python
# 入度为 0
def stepu0v(di0,E):
    S=set({})
    for u0 in di0:
        for (w,u,v) in E:
            if u == u0:
                S=S|{(w,u,v)}
    return S

# 出度为 0
def stepuv0(do0,E):
    S=set({})
    for v0 in do0:
        for (w,u,v) in E:
            if v == v0:
                S=S|{(w,u,v)}
    return S

# 最早时间路径
def TEpath(S,Hx,di):
    di0=set({})
    for (w,u,v) in S:
        if Hx[v] < Hx[u]+w:
            Hx[v]=Hx[u]+w
        if di[v]>0:
            di[v]=di[v]-1
        if di[v] == 0:
            di0=di0 | {v}
    return [Hx,di,di0]

# 最晚时间路径
def TLpath(S,Hy,do):
    do0=set({})
    for (w,u,v) in S:
        if Hy[u] > Hy[v]-w:
            Hy[u]=Hy[v]-w
        if do[u]>0:
            do[u]=do[u]-1
        if do[u] == 0:
            do0=do0 | {u}
    return [Hy,do,do0]


def craticalTE(V, E, di, v0, vn):
    Hx = [0] * len(V)

    di0 = {v0}
    while vn not in di0:
        S = stepu0v(di0, E)
        [Hx, di, di0] = TEpath(S, Hx, di)
        E = E - S
    return Hx

def craticalTL(V,E,do,Hn,v0,vn):
    Hy= [1000]*len(V)
    Hy[len(V)-1]=Hn
    do0={vn}
    while v0 not in do0:
        S=stepuv0(do0,E)
        [Hy,do,do0]=TLpath(S,Hy,do)
        E=E-S
    return Hy
```

#### 关键路径算法

```python
# 关键通路
def craticalpath(V,E,di,do,v0,vn):
    Hx=craticalTE(V,E,di,v0,vn)
    Hn=Hx[len(V)-1]
    Hy=craticalTL(V,E,do,Hn,v0,vn)
    V=list(V)
    N=len(V)
    C=set({})
    for k in range(N):
        if Hx[k] == Hy[k]:
            C=C|{V[k]}
    Pc=set({})
    for (w,u,v) in E:
        if u in C and v in C:
            Pc=Pc|{(w,u,v)}
    return Pc
```

## 树

### 引论

### 树概念

#### 树的定义

设G=<V, E>是无向图，若G 连通且无圈，则称为树，记为GT=<V, E>。

#### 定理

* 设 T 是树，则在 T 的任何两个不同顶点之间存在唯一的一条基本链。若在 T 的两个不相邻顶点之间加上一条边 e，则图 T+e 仅有一个圈
* 设树 T 是一个 (n, m) 图，则 m = n - 1
* 非平凡树 T 中至少存在两个次数为 1 的顶点
* 设 T 是非平凡 (n, m) 无向图，则下面五个命题等价
  * T 是树
  * T 连通且无圈
  * T 的每一对顶点之间有唯一的一条基本链
  * T 连通且 m = n - 1
  * T 无圈且 m = n - 1

#### 树的构建

```python
def graph2tree(V, E):
    E = sorted(E)
    (u, v) = E[0]
    Vt = {u, v}
    Et = {(u, v)}
    E = set(E)
    n = len(V)
    while (len(Et) < (n - 1)):
        m = len(Vt)
        for (u, v) in E:
            if ((u in Vt and v not in Vt)
                    or (u not in Vt and v in Vt)):
                Vt = Vt | {u, v}
                Et = Et | {(u, v)}
        E = E - Et
        if (m == len(Vt)):
            break
    return [Vt, Et]
```

#### 树的判断

```python
def istree(V, E):
    tv = True
    E = sorted(E)
    (u, v) = E[0]
    Vt = {u, v}
    E = set(E) - {(u, v)}
    Et = {(u, v)}
    while (E != set({})):
        for (u, v) in E:
            if (u in Vt) and (v in Vt):
                tv = False
                break
            if ((u in Vt) and (v not in Vt)) or ((u not in Vt) and (v in Vt)):
                Vt = Vt | {u, v}
                E = E - {(u, v)}
                Et = Et | {(u, v)}
        if (tv == False):
            break
    if ((len(Vt) - 1) != len(Et)):
        tv = False
    return tv
```

### 各类树

#### 根树

若一个**有向树**有一个引入次数为0的顶点，而所有其他顶点的引入次数都为1，则称该有向树为**根树**。引入次数为0的顶点称为**树根**。

#### m 元树

若根树中的每个顶点的引出次数都**小于或等于** m，则称这种根树为 m 元树。

##### 完全 m 元树

每个顶点的引出次数等于 m 或 0

##### 位置 m 元树

任何顶点的 m 个子结点都有位置

##### 二叉树

m = 2

#### 有序树

设T=<V,E>是根树，并且每个顶点的子顶点是**有序**的，则称为有序树。

<u,v,k>表示u的第k个子

### 二叉树

#### 定义

设T=<V,E>是有序树，并且每个顶点的最多有2个子顶点，则称为二叉树。

#### 递归定义

定义：设 $T = <V, E>$

1. $[\ ]$ 是二叉树。
2. $a\in V$，$[a]$是二叉树。
3. 若$a\in V$，$R_t$ 和 $L_t$ 是二叉树，则 $[a,L_t, R_t]$ 是二叉树。

#### 二叉树构建算法 (set, list 结构)

```python
def createbitree(nodes):
    nodes = set(nodes)
    if (len(nodes) == 0):
        return []
    if (len(nodes) == 1):
        return list(nodes)
    [a] = random.sample(nodes, 1)
    nodes = nodes - {a}
    k = random.randint(0, len(nodes) - 1)
    Lnodes = random.sample(nodes, k)
    Lnodes = set(Lnodes)
    Rnodes = nodes - Lnodes
    L = createbitree(Lnodes)
    R = createbitree(Rnodes)
    return [a, L, R]


def bitree(nodes):
    nodes = list(nodes)
    if (len(nodes) == 0):
        return []
    if (len(nodes) == 1):
        return nodes
    k = int(len(nodes) / 2)
    a = nodes.pop(k)
    Lnodes = nodes[0:k]
    Rnodes = nodes[k:]
    L = bitree(Lnodes)
    R = bitree(Rnodes)
    return [a, L, R]
```

#### 二叉树与图

##### 从树结构变换为图结构

1. 若`len(tree)=0`或`len(tree)=1`，则为空集。
2. tree有左子树，则`L=tree2graph(tree[1])`, `gtree=gtree | {(tree[0],tree[1][0])} | L`
3.  tree有右子树，则`R=tree2graph(tree[2])`, `gtree=gtree | {(tree[0],tree[2][0])} | R`

```python
def tree2graph(tree):
    gtree = set({})
    if len(tree) == 0 | len(tree) == 1:
        return gtree
    if len(tree[1]) != 0:
        L = tree2graph(tree[1])
        gtree = gtree | {(tree[0], tree[1][0])} | L
    if len(tree[2]) != 0:
        R = tree2graph(tree[2])
        gtree = gtree | {(tree[0], tree[2][0])} | R
    return gtree
```

#### 二叉树结构

从有序树生成二叉树

#### 遍历

设G=<E,V>是二叉树，访问G的每个顶点的过程称为遍历。

设 $G=<E,V>$ 是二叉树，$r$ 根顶点，若 $V={r}$ ，则 $r$ 是前序遍历，否则，若 $T_1$ 和 $T_2$ 是 $r$ 的左右子树，则先访问顶点 $r$，而后分别前序遍历子树 $T_1$ 和 $T_2$ 。

##### 前序遍历

```python
def preordertraversal(tree):
    if (len(tree) == 0):
        return []
    if (len(tree) == 1):
        return [tree[0]]
    else:
        return [tree[0]] + preordertraversal(tree[1]) + preordertraversal(tree[2])
```

##### 中序遍历

```python
def inordertraversal(tree):
    if (len(tree) == 0):
        return []
    if (len(tree) == 1):
        return [tree[0]]
    else:
        return preordertraversal(tree[1]) + [tree[0]] + preordertraversal(tree[2])
```

##### 后序遍历

```python
def postordertraversal(tree):
    if (len(tree) == 0):
        return []
    if (len(tree) == 1):
        return [tree[0]]
    else:
        return preordertraversal(tree[1]) + preordertraversal(tree[2]) + [tree[0]]
```

#### 表达式树

### 霍夫曼编码

#### 二元前缀码

用0-1字符串作为代码表示字符，要求**任何字符的代码都不能作为其他字符代码的前缀**；

对于位置二叉树，可以采用二元前缀编码，使得每一个顶点都与唯一的一个取自字符表 {0,1} 的字符串对应

1. 树根对应空串
2. 字符串为 a 的顶点，左子结点的为 0，右子节点为 1
3. 位置字符串的前缀编码 = {a|a 的树叶对应的字符串}

#### 霍夫曼编码

1. 假设顶点集合为 $S$, $u_1$ 和 $u_2$ 是权值最低的两个顶点
2. 构造一个新的顶点 $w$，令 $w$ 的左侧子结点为 $u_1$，右侧子结点为 $u_2$，权值为 $u_1$, $u_2$ 之和
3. $S\leftarrow(S-\{u_1,u_2\})\cup\{w\}$, 返回步骤 1，直至 $|S| = 1$

#### 霍夫曼树的构建与编码熵

```python
def codingentropy(W, C):
    # 求得编码熵
    r0 = 0
    r = 0
    W = sorted(W)
    C = sorted(C)
    for k in range(len(W)):
        r0 -= W[k][1] * math.log2(W[k][1])
        r += W[k][1] * len(C[k][1])
    r0 = math.floor((r0) * 1000) / 1000
    r = math.floor((r) * 1000) / 1000
    return [r0, r]


def Huffmantree(W):
    # 构建霍夫曼树
    W0 = []
    for [a, w] in W:
        W0 = W0 + [[w, a]]
    tree = sorted(W0)
    while len(tree) != 1:
        w = tree[0][0] + tree[1][0]
        w = math.floor(w * 10000) / 10000
        tree = [[w, tree[0], tree[1]]] + tree[2:]
        tree = sorted(tree)
    return tree[0]


def Huffmantree2graph(tree):
    gtree = set({})
    if len(tree) == 2:
        return gtree
    L = Huffmantree2graph(tree[1])
    R = Huffmantree2graph(tree[2])
    a = math.floor(tree[0] * 10000) / 10000
    l = math.floor(tree[1][0] * 10000) / 10000
    r = math.floor(tree[2][0] * 10000) / 10000
    gtree = {(a, l)} | {(a, r)} | L | R
    return gtree


def htree2graph(G):
    V = set({})
    for [u, v] in G:
        V = V | {u, v}
    V = sorted(V)
    V = V[::-1]
    E = set({})
    for [u, v] in G:
        E = E | {(V.index(u), V.index(v))}
    return [V, E]


def huffmancoding(subtree, code):
    if len(subtree) == 2:
        return [[subtree[1], code]]
    else:
        node0 = subtree[1]
        node1 = subtree[2]
        code = huffmancoding(node0, code + '0') + huffmancoding(node1, code + '1')
        return code


def HuffmanCoding(tree):
    code0 = huffmancoding(tree[1], '0')
    code1 = huffmancoding(tree[2], '1')
    return code0 + code1
```

### 生成树

#### 生成树 (Spanning Trees)

如果无向图 G 的一个生成子图 T 是树，则称 T 是图 G 的一个生成树

#### 最小生成树 (Minimum Spanning Trees, MST)

对于一个**带权无向连通**图，最小生成树是其生成树中边的权重之和最小的那个

**最小生成树可能不唯一**

#### 安全边 (save edge)

对于边的集合 $A\subseteq T$, 其中 $T$ 是一棵最小生成树，如果集合 $A\cup\{(u, v)\}$ 同样属于 $T$，则 $(u,v)$ 是集合 $A$ 的安全边

> 若每一步都向集合中增加一条安全边，就可以找到图 G 的最小生成树

#### 分割 (Cut)

表示为 $(S, V-S)$ 是图 $(V, E)$ 的一个划分，划分之和，一部分节点 $\in S$，其他节点 $\not\in S \Rightarrow \in \{V-S\}$

#### 横跨 (Cross)

如果边 $(u, v)\in E$ 的一个端点在集合 $S$ 中，另一个端点在集合 $(V-S)$ 中，则称边 $(u,v)$ 是分割的横跨边

#### 尊重 (Respect)

如果集合 A 中不存在横跨该分割的边，则称该分割 **尊重** 集合 $A$

#### 轻边 (Light Edge)

在横跨一个分割的所有边中，权重最小的边，称为 **轻边**

#### 定理（寻找安全边）

1. 对于 **带权无向连通图** $G(V, E, W)$
2. 假设边的集合 $A$ 为一棵最小生成树的子集
3. $(S, V-S)$ 为任意尊重 $A$ 的分割
4. $(u,v)$ 是横跨分割 $(S, V-S)$ 的轻边

那么，边 $(u, v)$ 是 $A$ 的一条安全边

#### Kruskal 算法

##### 算法思路

```python
将图看作离散的点和一堆边
dot = []
while num(边) < n - 1:
    从未选择的边中找到满足：<m ,n>
    1. 权重最小
    2. 两个端点不同时出现在已连接的点中
    dot.append(m)
    dot.append(n)
    将边从未选择中删除
    num(边) ++
```

![img](https://upload-images.jianshu.io/upload_images/3755117-2656ffcd5cdb097d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https://upload-images.jianshu.io/upload_images/3755117-8392698e3388fece.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

#### Prim 算法

![img](https://upload-images.jianshu.io/upload_images/3755117-4491cf0d977af08c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https://upload-images.jianshu.io/upload_images/3755117-ac654c5400c4a97e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### 平面图及着色

#### 平面图

所有边在顶点以外没有任何交叉的图称为 **平面图**

#### 非平面图

##### $K_{3,3}$

![image-20210619181532731](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619181532731.png)

* $k_{3,3}$ 是边数最少的非平面图

##### $k_{5}$

![image-20210619181621676](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619181621676.png)

* $k_5$ 是顶点数最少的非平面图

$k_{3,3}$ 和 $k_{5}$ 删除任意一条边就是平面图

#### 平面图的面

当把一个连通平面图 G 画在平面上，使它的边在顶点之外没有任何交叉时，G 的边把平面分割成许多区域，这样的一个区域称为图 G 的面。

##### 有限面

平面图内部的各个有限区域

##### 无限面

平面图外部的无限区域

#### 周界

围成一个面的各边构成的闭合链，称为 **周界**

#### 次数

一个平面图所有面的次数之和等于边的数量的两倍

#### ==欧拉公式==

具有 $n$ 个顶点，$m$ 条边， $k$ 个面的 $(m,n)$ 连通平面图 $G$，等式（欧拉公式）恒成立：$n-m+k=2$

#### 极大平面图

设图 G 是一个平面图，如果连接 G 的任意两个不邻接顶点 $u$ 和 $v$ ，都会使 $G+(u,v)$ 变成非平面图，则称图 $G$ 是极大平面图

##### 定理

设 G 是具有至少三个顶点的极大平面图，则 G 的任何一个面都是 $k_3$，$3k=2m$

##### 极大平面图的欧拉公式

* 若一个图的极大平面图，则 $m = 3n-6$
* 若一个图是平面图，则 $m\le 3n-6$，尚可加边

#### 同胚

反复插入和（或）除去次数为 2 的顶点，它们能变成同构的图

![image-20210619202100986](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619202100986.png)

![image-20210619202104794](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619202104794.png)

##### 库拉图斯基 (Kuratowski) 定理

图 G 是平面图，当且仅当它不包含同胚于 $k_{3,3}$ 或 $k_5$ 的子图

#### 着色问题

给定一个无向图 $G(V, E)$，其中 $V$ 为顶点集合，$E$ 为边集合。图着色问题为：将 $V$ 分成 $K$ 个颜色组，没有相邻顶点同色，取最小的 $K$ 值

#### 对偶图

设 $G$ 是一个平面图，有 $k$ 个面，记为 $F_1, F_2, ..., F_k$，其中包括无限面，按如下方式构造的图 $G^*$ 称为图 $G$ 的对偶图：

* 将每个面 $F_i$ 作为 $G^*$ 的一个顶点 $f_j$
* 对于两个面 $F_i$ 和 $F_j$ 的公共边，对应图 $G^*$ 中的边 $(f_i, f_j)$
* 对于仅属于一个面的边（如悬挂边和桥边），则对应图 $G^*$ 中的自环

*对偶图将地图的着色问题转换为一般图的顶点着色问题*

#### 平面图的着色

* 对给定的无向图 G 的顶点进行涂色，如果每个顶点只涂一种颜色并且任何两个邻接的顶点颜色不同，则称为图的一个正常着色。正常着色所需要的最少颜色数，称为图 G 的 **着色数**，记为 $\chi(G)$
* 对于不是零图的无向图 G，以下三个说法等价：
  * 图 G 的着色数 $\chi(G) = 2$
  * 图 G 是二分图
  * 图 G 的所有圈的长度都是偶数
* 对以下图的着色数容易计算
  * 对于 **零图**，$\chi(G) = 1$
  * 对于具有 n 个顶点的完全图 $K_N$，着色数 $\chi(G) = n$
  * 对于两个或两个以上节点的树 $T$，着色数 $\chi(G) = 2$

#### 韦尔奇-鲍威尔算法

* 将图 G 的顶点按次数递减的顺序排序
* 用一种颜色涂染序列中的第一个顶点，以及与该顶点不相邻的每一个顶点
* 余下的节点重新排序，按照上述方法重新着色

> 此算法不保证能用最少色数

#### 五色定理

对任意一个平面图 G，都有 $\chi(G) \le 5$ 

##### 证明

![image-20210619204950520](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619204950520.png)

![image-20210619204955910](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619204955910.png)

![image-20210619205224413](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619205224413.png)

![image-20210619205229157](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619205229157.png)![image-20210619205244631](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619205244631.png)![image-20210619205233370](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619205233370.png)

### 二分图与匹配

#### 二分图

设 $G = <V, E>$ 是一个无向图，如果可以把 $V$ 划分成两个子集 $X$ 和 $Y$，使得同一个子集中的任何两个顶点都不邻接，则称图 $G$ 为 **二分图**。$X$ 和 $Y$ 称为 $G$ 的互补顶点子集

![image-20210619205612678](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619205612678.png)

#### 完全二分图

$X$ 中的每个顶点都与 $Y$ 中的所有顶点邻接。若 $\#X=p, \#Y = q$，则将其记为 $K_{p,q}$

![image-20210619205800704](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619205800704.png)

#### 定理

* $G$ 是一个二分图，当且仅当非平凡无向图 $G$ 的所有圈的长度都是偶数

#### 二分图匹配

##### 匹配

设  $G = <V, E>$ 是一个二分图，如果 $E$ 的一个子集 $M$ 中的任何两条边都不相邻，则称 $M$ 为二分图 $G$ 的一个匹配

##### 饱和顶点 & 非饱和顶点

此时，$M$ 中的边所关联的顶点称为 $M$ 的 **饱和顶点**，而 $G$ 的其他顶点为 **非饱和顶点**

![image-20210619210401708](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619210401708.png)

##### 最大匹配

在二分图 $G$ 的所有匹配中， **边数** 最多的匹配称为 $G$ 的 **最大匹配**

##### 交错链

设 $M$ 是二分图 $G$ 的一个匹配。如果 $G$ 中有这样一条基本链， **链中任何相邻的两条边中恰好有一条属于 $M$**，则称这样的链为关于 $M$ 的 **交错链**

![image-20210619210708537](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619210708537.png)

##### 可扩充链

若一条交错链的两个端点是 $M$ 的非饱和顶点，则称该交错链为关于 $M$ 的 **可扩充链** (M-augmenting path)

![image-20210619210919683](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619210919683.png)

##### 最大匹配相关定理

设 $M$ 是二分图 $G$ 的一个匹配，当且仅当 $G$ 中不存在关于 $M$ 的可扩充链时，$M$ 是 $G$ 的最大匹配

##### 匈牙利算法

1. 任取一个匹配 $M$ （可以冲空集或一条边开始）
2. 令 $S$ 是非饱和顶点（尚未匹配的点）的集合
3. 若 $S=\empty$，则 $M$ 已经是最大匹配
4. 否则，从 $S$ 中取出一个非饱和顶点 $u_0$ 作为起点，从 $u_0$ 开始找到交错链 $P$
5. 如 $P$ 是可扩充链，则令 $M = (M\cup P)-(M\cap P)$
6. 如 $P$ 不是可扩充链，则去掉 $u_0$，回到第 3 步

##### 从 $X$ 到 $Y$ 的匹配

如有二分图 $G$ 的匹配 $M$，使得 $X$ 的每一个顶点都是饱和的，则称 $M$ 为从 $X$ 到 $Y$ 的匹配

* 若存在从 $X$ 到 $Y$ 的匹配，则应该满足 $\#X\le\#Y$
* 若 $M$ 为从 $X$ 到 $Y$ 的匹配，则 $M$ 是二分图 $G$ 的最大匹配

**判断从 $X$ 到 $Y$ 的匹配定理**

设 $G$ 是一个具有互补顶点子集 $X$ 和 $Y$ 的二分图。当且仅当对于 $X$ 的任意子集 $U$，$\#\Gamma(U)\ge\#U$ 被满足时，存在 $G$ 的从 $X$ 到 $Y$ 的匹配

其中，$U$ 表示 $X$ 的子集，$\Gamma(U)$ 表示 $U$ 的邻域，即与 $U$ 中顶点邻接的所有顶点的集合，$\Gamma(U)\subseteq Y$

###### 相异性条件

上述条件被称为 **相异性条件**

###### 从 $X$ 到 $Y$ 匹配的算法

![](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619213305111.png)

![](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619213331044.png)

![image-20210619213337469](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210619213337469.png)

###### t 条件

设 $G$ 是具有互补顶点子集 $X$ 和 $Y$ 的二分图，如果能找到一个正整数 t，使得对 $X$ 中的任意顶点 $x$ 有 $d(x)\ge t$，对 $Y$ 的任意顶点有 $d(y)\le t$，则图 $G$ 存在从 $X$ 到 $Y$ 的匹配。定理中的条件称为 "t 条件"

##### 二分图的稳定匹配问题

###### 问题定义 & 描述

###### Gale-Shapley 算法

# 代数系统

## 知识回顾

### 集合

#### 集合

集合是一些能够**明确区分**的对象构成的整体，集合一般用*大写英文字母*表示

   * 集合是一组无序对象
   * 在集合中没有重复出现的元素

#### 元素

如果对象a在集合A中，则称a是A的**元素**

#### 集合与元素的关系

   * $a属于A, 记为a\in A$
   * $a不属于A, 记为a\notin A$
   * 在数理逻辑中，属于关系$\in$看作是一个谓词：$a\in A, \neg(a\in A$)

#### 集合示例

   * $N=\{x|x是自然数\}$
   * $I=\{x|x是整数\}$
   * $Q=\{x|x是有理数\}$

#### 集合运算

| 运算名称   | 集合表示    | 内涵定义                       | 谓词逻辑表示                                                 | python文字描述            | python表示 | python 意义                      |
| ---------- | ----------- | ------------------------------ | ------------------------------------------------------------ | ------------------------- | ---------- | -------------------------------- |
| 并运算     | $A\cup B$   | $\{x|x\in A\vee x\in B\}$      | $\forall x(x\in A\cap B\leftrightarrow x\in A\wedge x\in B$) | A.union(B)                | A$|$B      | x in A$|$B==(x in A) or (x in B) |
| 交运算     | $A\cap B$   | $\{x|x\in A\wedge x\in B\}$    | $\forall x(x\in A\cup B\leftrightarrow x\in A\vee x\in B$)   | A.intersection(B)         | A&B        | x in A&B==x in A and x in B      |
| 差运算     | $A-B$       | $\{x|x\in A\wedge x\notin B\}$ | $\forall x(x\in A-B\leftrightarrow x\in A\wedge\neg x\in B$) | A.difference(B)           | A-B        | x in A-B==x in A and not(x in B) |
| 对称差运算 | $A\oplus B$ | $\{x|x\in A\oplus x\in B\}$    | $\forall x(x\in A\oplus B\leftrightarrow x\in A\oplus x\in B$) | A.symmetric_difference(B) | A^B        | x in A^B==x in A$\oplus$x in B   |
| 补运算     | ~$A$        | {$x|x\notin A$}                | $\forall x(x\in$~$A\leftrightarrow x\notin A)$               | U.differnce(A)            | U-A        | x in ~A==not x in A              |

#### 集合之间的关系

##### 包含关系（子集）

###### 定义

若集合A的元素都是集合B的元素，则称A是B的子集，也称A包含于B，或者B包含A，记为A$\subseteq$B

###### 谓词逻辑表示

* $A\subseteq B\Leftrightarrow\forall x(x\in A\rightarrow x\in B$)
* $A\subset B\Leftrightarrow\forall x(x\in A\rightarrow x\in B$)$\wedge\exists x(x\in B\wedge x\notin A$)
* $A\subset B\Leftrightarrow\forall x(x\in A\rightarrow x\in B$)$\wedge A\cap B\neq\varnothing$
* $A\not\subseteq B\Leftrightarrow\exists x(x\in A\wedge x\notin B$)

##### 相等关系

###### 定义(外延性公理)：

如果集合A和B含有相同的元素，则称A和B相等，记为A=B

###### 谓词逻辑表示

* $A=B\Leftrightarrow\forall x(x\in A\leftrightarrow x\in B$)
* $A=B\Leftrightarrow\forall x(x\in A\leftrightarrow x\in B$)$\wedge\forall x(x\in B\rightarrow x\in A$)

#### 集合运算性质

| 定律      | 描述1                                     | 描述2                                     |
| --------- | ----------------------------------------- | ----------------------------------------- |
| 结合律    | ($A\cup B)\cup C=A\cup(B\cup C$)          | ($A\cap B)\cap C=A\cap(B\cap C$)          |
| 交换律    | $A\cup B=B\cup A$                         | $A\cap B=B\cap A$                         |
| 分配律    | $A\cup(B\cap C$)=($A\cup B)\cap(A\cup C$) | $A\cap(B\cup C$)=($A\cap B)\cup(A\cap C$) |
| 德·摩根律 | $\neg(A\cup B)=$$(\neg A)\cap(\neg B)$    | $\neg (A\cap B)=$$(\neg A)\cup (\neg B)$  |
| 幂等律    | $A\cup A=A$                               | $A\cap A=A$                               |
| 吸收率    | $A\cup(A\cap B$)$=A$                      | $A\cap(A\cup B$)$=A$                      |
| 对合律    | ~~$A=A$                                   |                                           |

### 函数

#### 满射、单射和双射

$设函数f:X\rightarrow Y$

##### 满射

若对于每个 y$\in$Y，都存在 x$\in$X使得 f (x) = y，则称f为满射，即

   * $\forall y(y\in Y\rightarrow\exists x(x\in X\wedge f(x$$)=y))$

##### 单射

若对于任意 x1$\in$X, x2$\in$X， 若 f (x$_1$) = f (x$_2$)则x1=x2，称 f 为单射，即

   * $\forall x_1\forall x_2(x_1\in X\wedge x_2\in X\wedge f(x_1$$)=f(x_2$$)\rightarrow x_1=x_2$

##### 双射

$f$既是单射又是满射，则$f$是双射函数。

![image-20210628124603445](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210628124603445.png)

### 关系

#### 关系的定义

设X,Y是集合，若R$\subseteq$X × Y, 则称R为从X到Y的关系，简称为关系R

   * 将<x,y>$\in$R表示为x$R$y, 读作“x和y有关系R”
   * 将<x,y>$\not\in$R表示为x$\underline{R}$y, 读作“x和y没有关系R”

若R是从X到Y的关系，就是集合X中元素与集合Y中元素之间的对应

   * 从集合X中选择一个元素x, 对应地再集合Y中选择一个元素y
   * x和y组成一个有序偶(x,y)
   * 有序偶(x,y)$\in$R

#### 定义域和值域

##### 定义域

关系R中所有有序偶的第一元组成的集合称为R的定义域，记为**dom(R)**

dom(R)={x|$\exists$y(<x,y$\in$R>)}

##### 值域

关系R中所有有序偶的第二元组成的集合称为R的值域，记为**ran(R)**

ran(R)={y|$\exists$x(<x,y>$\in$R)}

#### 特殊关系

##### 空关系$\varnothing$

##### 恒等关系

$I_x=\{<x,x>|x\in X\}$

##### 全域关系

$U_x=\{<x,y>|x\in X\wedge y\in X\}$

$\varnothing\subseteq I_x\subseteq U_x$

#### 关系的集合运算

设关系$R$和$S$是从$X$到$Y$的两个关系，则运算为$R\cap S,R\cup S,R-S,R$^$S,$~$R$

   * $R\cap S=\{(x,y)|(x,y)\in R\wedge<x,y>\in S\}$
   * $R\cup S=\{(x,y)|(x,y)\in R\vee<x,y>\in S\}$
   * $R-S=\{(x,y)|(x,y)\in R\wedge<x,y>\not\in S\}$
   * $R$^$S=\{(x,y)|(x,y)\in R\oplus<x,y>\in S\}$
   * ~$R=\{(x,y)|(x,y)\not\in R\}$


   * python运算符
     * & 交
     * | 并
     * $-$ 差
     * ^ 对称差

#### 复合运算

关系$R\subseteq X×Y$和关系$S\subseteq Y×Z$的复合

$R\circ S=\{<x,z>| \exists y(<x,y>\in R\wedge<y,z>\in S$$)\}$

#### 逆运算

定义$R\subseteq X×Y$的逆关系

$R^-$$^1=\{<y,x>|<x,y>\in R\}$

将R中每个有序偶的第一元与第二元对调就得到$R$的逆关系$R^-$$^1$

#### 幂运算

设$R$是$X$上关系，$n$是自然数，$R^n$定义如下:

   * $R^0=I_x\qquad\qquad\ |X|=m$
   * $R^n$$^+$$^1=R^n\circ R\qquad R^m$$^+$$^1=?$

#### 关系运算性质

若R和S都是从X到Y的关系，则$R\cap S,R\cup S,R-S,R+S,$~$R$仍然是从X到Y的关系

   * $R\subseteq X×Y,\ S\subseteq X×Y\vDash R\cap\ S\subset X×Y$
   * $R\subseteq X×Y,\ S\subseteq X×Y\vDash R\cup\ S\subset X×Y$
   * $R\subseteq X×Y,\ S\subseteq X×Y\vDash R-S \subset X×Y$
   * $R\subseteq X×Y,\ S\subseteq X×Y\vDash R+S \subset X×Y$
   * $R\subseteq X×Y\vDash$~$R\subseteq X×Y$

#### 复合性质

若$R$是从$X$到$Y$的关系，$S$是从$Y$到$Z$的关系，则$R\circ S\subseteq X×Z$

即$R\subseteq X×Y,\ S\subseteq Y×Z \vDash R\circ S\subseteq X×Z$

   * $(R_1\circ R_2$$)\circ R_3=R_1\circ(R_2\circ R_3$)
   * $R\circ I_A=I_A\circ R=R$
   * $(R_1\cap R_2$$)\circ R_3\subseteq R_1\circ R_3\cap R_2\circ R_3$
   * $R_1\circ($$R_2\cap R_3)\subseteq R_1\circ R_2\cap R_1\circ R_3$
   * $(R_1\cup R_2$$)\circ R_3\subseteq R_1\circ R_3\cup R_2\circ R_3$
   * $R_1\circ($$R_2\cup R_3)\subseteq R_1\circ R_2\cup R_1\circ R_3$
   * <font color=red>注意：一般不满足交换律</font>

#### 逆运算性质

   * ($R_1\cup R_2$)$^-$$^1$$=R_1$$^-$$^1$$\cup\ R_2$$^-$$^1$
   * ($R_1\cap R_2$)$^-$$^1$$=R_1$$^-$$^1$$\cap\ R_2$$^-$$^1$
   * ($R_1-R_2$)$^-$$^1$$=R_1$$^-$$^1$$-R_2$$^-$$^1$
   * (~$\ R_1$)$^-$$^1$$=$$\ $~$\ R_1$$^-$$^1$
   * <font color=red>($R_1\circ R_2$)$^-$$^1$$=R_2$$^-$$^1$$\circ\ R_1$$^-$$^1$</font>

#### 幂运算性质

   * $R^mR^n=R^m$$^+$$^n$
   * ($R^m$)$^n=R^m$$^n$

#### 关系特性

* 集合X上的有一些重要的关系R
  * 自反的($reflexive$)
    * 矩阵的主对角线均为1，关系图中每个节点都有自环
  * 反自反的($irreflexive$)
    * 矩阵的主对角线均为0，关系图中每个节点都没有自环
  * 对称的($symmetric$)
    * 矩阵的所有元素关于主对角线对称，关系图中任意两个节点间或者没有边或者是双向边
  * 反对称的($antisymmetric$)
    * 矩阵关于主对角线对称位置至多有一个为1，关系图中任意两个节点间或者没有边或者单边。
  * 传递的($transitive$)
    * 任意两个节点间有路径，节点间一定有边。
* 构建特性关系
  * 自反关系
  * 对称关系
  * 传递关系

#### 自反

   * 定义设R是集合X上的关系
   * 若对于每个x$\in X$都有$<x,x>\in R$, 则称R为自然的，即
   * $\forall x(x\in X\rightarrow<x,x>\in R$)

#### 反自反

   * 定义设R是集合X上的关系
   * 若对于每个$x\in X$都有$<x,x>\not\in R$, 则称R为反自反的，即
   * $\forall x($$x\in X\rightarrow<x,x>\not\in R)$
   * $\forall x($$x\in X\rightarrow\neg<x,x>\in R)$

#### 对称

   * 定义设$R$是集合$X$上的关系
   * 若对于$x,y\in X$, 以及($x,y$)$\in R$, 就称R是对称的, 即
   * $\forall x\forall y(x\in X\wedge y\in X\wedge(x,y$)$\in\rightarrow(y,x$)$\in R)$

#### 反对称

   * 定义设R是集合X上的关系
   * 若对于每个$x,y\in X$, 以及$(x,y)\in R$, 且$(y,x)\in R$, 则有$x=y$, 就称$R$是反对称的，即
   * $\forall x\forall y(x\in X\wedge y\in X\wedge (x,y$$)\in R\wedge(y,x$$)\in R\rightarrow x=y$

#### 传递

   * 定义设$R是集合X$上的关系
   * 若对于每个$x,y,x\in X$, 以及$(x,y)\in R$且$<y,z>\in R$, 则有$<x,z>\in R$, 就称$R$是传递的，即
   * $\forall x\forall y\forall z(x\in X\wedge y\in X\wedge z\in X\wedge(x,y)\in R\wedge<y,z>\in R\rightarrow<x,z>\in R)$

#### 构造特征关系

   * 自反
     * 设 R是集合 X 上的关系，则 R' 是自反的，其中 $R'=R\cup I_x$
   * 反自反
     * 设 R是集合 X 上的关系，则 R' 是反自反的，其中 $R'=R-I_x$
   * 对称
     * 设 R是集合 X 上的关系，则 R' 是对称的，其中 $R'=R\cup R^-$$^1$
   * 反对称
     * 设 R是集合 X 上的关系，则 R'是反对称的，其中 $R'=(R-(R\cap R^-$$^1))\cup R\cap I_x$
   * 传递
     * 设 R是集合 X 上的关系，|X|=m，则 R'是传递的，其中，$R'=\cup_n$$_=$$_1$$_,$$_m$$R^n$

#### 关系特性的性质

##### 定理：

设R是集合X上的关系

   * $R是自反的当且仅当R=I_x\cup R$
   * $R的反自反的当且仅当I_x\cap R=\varnothing$
   * $R是对称的当且仅当R^-$$^1=R$
   * $R是反对称的当且仅当R\cap R^-$$^1\subseteq I_x$
   * $R是传递的当且仅当R\circ R\subseteq R$

##### 关系特性与集合运算的性质探究

设$R_1, R_2\subseteq X×X$, 若$R_1, R_2$是自反的, 则$R_1\cup R_2$是自反的

| \        | $R_1\cup R_2$ | $R_1\cap R_2$ | $R_1-$$R_2$ | ~$R_1$  | $R_1\circ R_2$ | $R_1^-$$^1$ |
| -------- | ------------- | ------------- | ----------- | ------- | -------------- | ----------- |
| 自反性   | $\surd$       | $\surd$       | -           | -       | $\surd$        | $\surd$     |
| 反自反性 | $\surd$       | $\surd$       | $\surd$     | -       | -              | $\surd$     |
| 对称性   | $\surd$       | $\surd$       | $\surd$     | $\surd$ | -              | $\surd$     |
| 反对称性 | -             | $\surd$       | $\surd$     | -       | -              | $\surd$     |
| 传递性   | -             | $\surd$       | -           | -       | -              | $\surd$     |

## 代数系统

### 代数运算

#### 概念

设 $X$ 是一个集合，$\psi$ 是 $X\times X\rightarrow X$ 的映射，则称 $\psi$ 是 $X$ 的一个 **代数运算**

在集合 $X$ 中，任意两个有次序的元素 $a$ 与 $b$，依据一个法则，都有**唯一确定**的元素 $d$ 与它们对应，则称这个法则是集合 $X$ 的一个代数运算，称集合 $X$ 关于运算 $\psi$ 是**封闭的**。可记为 $a\circ b=d$

**特点**

* 唯一性
* 封闭性

#### 代数运算实例

$\N,\ \Z,\ \Q,\ \R$ 分别为自然数、整数、有理数、实数集

$M_n(\R)$ 为 $n$ 阶实矩阵（实数矩阵）集合，$n\ge 2$

$P(B)$ 为幂集

$X^X$ 为从 $X$ 到 $X$ 的函数集, $|X|\ge 2$

| 集合                | 运算                               |
| ------------------- | ---------------------------------- |
| $\N,\ \Z,\ \Q,\ \R$ | $+,\ \times$                       |
| $M_n(\R)$           | $+$(矩阵加法)$,\ \times$(矩阵乘法) |
| $P(B)$              | $\cup,\ \cap$                      |
| $\{0,1\}$           | $\and,\ \or$                       |
| $X^X$               | $\circ$(函数复合)                  |

$N_n$ 是有穷自然数集合

* $<N_n, \oplus>$
  * $N_n = \{0, 1,...,n-1\}$
  * $x\oplus y = (x+y) \mod{n}$
* $<N_n,\otimes>$
  * $N_n = \{1, ...,n\}$
  * $x\otimes y = (x\times y)\mod{n}$

#### 运算表

表示有穷集上的一元运算和二元运算

| $\circ$ | $a_1$          | $a_2$          | ...  | $a_n$          |
| ------- | -------------- | -------------- | ---- | -------------- |
| $a_1$   | $a_1\circ a_1$ | $a_1\circ a_2$ | ...  | $a_1\circ a_n$ |
| $a_2$   | $a_2\circ a_1$ | $a_2\circ a_2$ | ...  | $a_2\circ a_n$ |
| ...     |                | ...            | ...  | ...            |
| $a_n$   | $a_n\circ a_1$ | $a_n\circ a_2$ | ...  | $a_n\circ a_n$ |

### 结合律、交换律、分配律

#### 结合律

设 $X$ 是集合，$X$ 上有代数运算 $\circ$，如果对 $X$ 中任意元素 $x,y,z$ 都有 $x\circ(y\circ z)=(x\circ y)\circ z$

#### 交换律

$x\circ y=y\circ x$

#### 分配律

$x\circ (y\oplus z) = (x\circ y)\oplus (x\circ z)$

### 幂等律、吸收律

#### ==幂等律==

设X是集合，$X$上有代数运算$\circ$，$e$是$X$上的单位元，如果对$X$中任意元素$x$都有 $x\circ x = e$，则称运算在$X$上满足**幂等律**

#### ==吸收律==

设$X$是集合，$X$上有代数运算$\circ$，如果对$X$中任意元素$x$都有 $x\circ x=x$，则称运算在$X$上满足**吸收律**

| 集合           | 运算                 | 交换律   | 结合律  | 幂等律   |
| -------------- | -------------------- | -------- | ------- | -------- |
| $\N, \Z,\Q,\R$ | $+$                  | $\surd$  | $\surd$ | $\times$ |
|                | $\times$             | $\surd$  | $\surd$ | $\times$ |
| $M_n(R)$       | $+$(矩阵加法)        | $\surd$  | $\surd$ | $\times$ |
|                | $\times$(矩阵乘法)   | $\times$ | $\surd$ | $\times$ |
| $P(S)$         | $\cup$               | $\surd$  | $\surd$ | $\surd$  |
|                | $\cap$               | $\surd$  | $\surd$ | $\surd$  |
| $\{0,1\}$      | $\and$               | $\surd$  | $\surd$ | $\surd$  |
|                | $\or$                | $\surd$  | $\surd$ | $\surd$  |
| $X^X$          | $\circ$ （函数复合） | $\times$ | $\surd$ | $\times$ |

### 克莱恩 (Klein) 四元集合及运算

| $\circ$ | e    | a    | b    | c    |
| ------- | ---- | ---- | ---- | ---- |
| e       | e    | a    | b    | c    |
| a       | a    | e    | c    | b    |
| b       | b    | c    | e    | a    |
| c       | c    | b    | a    | e    |

### 特殊元

#### 单位元

设$\circ$为$X$上的二元运算,如果存在$e\in X$，使得对任意 $x\in X$ 都有 $e\circ x = x$，则称 $e$ 是 $X$ 中关于$\circ$运算的左单位元。（同理定义右单位元）

>  $e$ 既为左单位元又为右单位元 $\Rightarrow$ $e$ 为 $X$ 上关于 $\circ$ 运算的单位元

#### 零元

设$\circ$为$X$上的二元运算,如果存在$0\in X$，使得对任意 $x\in X$ 都有 $0\circ x = 0$，则称 $0$ 是 $X$ 中关于$\circ$运算的左零元。（同理定义右零元）

>  $0$ 既为左零元又为右零元 $\Rightarrow$ $0$ 为 $X$ 上关于 $\circ$ 运算的单位元

#### 逆元

关于 $\circ$ 运算，若 $y\in X$ 既是 $x$ 的左逆元又是 $x$ 的右逆元则称 $y$ 为 $x$ 的逆元，表示为 $x^{-1}$ 。

> 如果 $x$ 的逆元存在，就称 $x$ 是可逆的。

#### 幂等元

设 $\circ$ 为X上的二元运算,若 $x\in X$ 且 $x \circ x=x$，则称 $x$ 是 $X$ 中关于运算 $\circ$ 的幂等元。

**特殊元实例**

| 集合                | 运算                | 单位元   | 零元     | 逆元                       |
| ------------------- | ------------------- | -------- | -------- | -------------------------- |
| $\N,\ \Z,\ \Q,\ \R$ | $+$                 | $0$      | $\times$ | $-x$                       |
|                     | $\times$            | $1$      | $0$      | $x^{-1}$                   |
| $M_n(\R)$           | $+$ (矩阵加法)      | $0$ 矩阵 | $\times$ | $-X$                       |
|                     | $\times$ (矩阵乘法) | 单位矩阵 | $0$ 矩阵 | $X^{-1}$                   |
| $P(S)$              | $\cup$              | $\empty$ | $S$      | $\empty$ 的逆元为 $\empty$ |
|                     | $\cap$              | $S$      | $\empty$ | $S$ 的逆元为 $S$           |

## 代数系统

设 $X$ 是非空集合，$X$上有 $m$ 个代数运算 $\psi_1, \psi_2, …, \psi_m$，集合 $X$ 及其代数运算组成的系统称为一个代数系统，简称代数，记为 $<X, {\psi_1,\psi_2,…,\psi_m}>$，其中，$m>0$。

在代数中，对象是抽象的而不是具体的，对象上的运算也是抽象的，其含义由一组给定公理规定。

### 公理

代数运算所具有的性质作为公理，并用方程的形式表示出来。如：结合律、交换律

### 代数常量

特殊元素：如单位元、零元等等

### 代数系统实例

<对象集合、运算集合、常量集合>

<$\N$，+，*，0，1>

$\forall a,b,c\in \N$

* (a+b)+c = a+(b+c), (a\*b)\*c = a\*(b\*c)
* a+b =b +a , a\*b=b\*a 
* a+0 =0+a =a, a+(-a)=0, a\*1 =1\*a =a
* (a+b)\*c = a*c+b\*c

### 同态

#### 定义

设集合 $X$ 与 $Y$ 各有代数运算 $\circ$ 及 $\bullet$ ，且 $\psi$ 是 $X$ 到 $Y$ 的一个映射，如果 $\psi$ 满足以下条件：在 $\psi$ 之下：$\psi(a\circ b)=\psi(a)\bullet\psi(b)$，称 $\psi$ 为代数系统 $<X,\circ>$ 与 $<Y,\bullet>$ **同态**。记为 $<X,\circ>\sim<Y,\bullet>$

若 $\psi$ 是满射，则 $\psi$ 称为代数系统 $<X,\circ>$ 到 $<Y,\bullet>$ 的同态满射。简称 $<X,\circ>$ 与 $<Y,\bullet>$ **满同态**

#### 性质

* 若代数系统 $<X,\circ>$ 与 $<Y,\bullet>$ 满同态，则对代数运算 $\circ$ 及 $\bullet$
  * 若 $\circ$ 满足结合律，则 $\bullet$ 也满足结合律
  * 若 $\circ$ 满足交换律，则 $\bullet$ 也满足交换律
* 若代数系统 $<X, \{\circ,\oplus \}>$，$<Y, \{\bullet,\otimes \}>$ 满同态，若代数运算 $\circ, \oplus$ 满足分配律，则 $\bullet,\otimes$ 也满足分配律

### 同构

#### 定义

设集合 $X$ 与 $Y$ 各有代数运算 $\circ$ 及 $\bullet$，且 $\psi$ 是 $X$ 到 $Y$ 的一个一一映射，如果 $\psi$ 满足以下条件：$\psi(a\circ b)=\psi(a)\bullet\psi(b)$，称 $\psi$ 为代数系统 $<X, \circ>$ 到 $<Y,\bullet>$ 的一个 **同构映射**，简称 $<X, \circ>$ 与 $<Y,\bullet>$ **同构**。记为  $<X, \circ>\cong<Y,\bullet>$ 

#### 性质

* 保持运算的所有性质
* 从抽象角度，两个代数系统完全相同

### 子代数

#### 定义

设 $A=<X,\bullet>$ 是一个代数系统，若 $Y\not=\empty$ , $Y\in X$，并且对于代数 $A$ 的运算 $\bullet$ 封闭，则称代数系统 $A_s=<Y,\bullet>$ 是代数系统 $A$ 的子代数

#### 性质

子代数保留原来代数系统中相关运算性质

### 商代数

#### 定义

 设 $R$ 是 $X$ 上的等价关系，$A=<X,\bullet>$ 是一代数系统，则 $A_q=<X/\R,\circ>$ 是一代数系统，称为 $A$ 的**商代数**。其中，$X/R=\{[x]_\R|x\in X\}$ ，$\circ$ 定义为对任意的 $[x]_\R,\ [y]_\R\in X/\R,\ [x]_\R\circ[y]_\R=[x\bullet y]_\R$

> 等价关系定义为：设R是非空集合A上的二元关系，若R是自反的、对称的、传递的，则称R是A上的等价关系。

#### 性质

* 一个代数系统 $<X, \bullet>$ 与其商代数 $<Y, \circ>$ 满同态。
* 商代数保留原代数系统的运算性质

### 积代数

#### 定义

代数系统 $<X, \bullet>$ , $<X, \circ>$ 的积代数定义为 $<X\times Y,\otimes>$ ，其中 $\times$ 为笛卡尔积，运算 $\otimes$ 定义为：

对任意 $(x_1, y_1)$, $(x_2, y_2)\in X\times Y,\ (x_1,y_1)\otimes(x_2,y_2)=((x_1\bullet x_2),\ (y_1\circ y_2))$ 

#### 性质

积代数保持两个代数运算共有的运算性质

## 半群

### 半群

#### 定义

对代数系统 $<S,\circ>$ ，若对任意 $a,b,c\in S$，有 $(a\circ b)\circ c=a\circ(b\circ c)$，则称 $<S,\circ>$ 为**半群**

#### 性质

* $<S, \circ>$ 为半群，若 $S$ 有限，则必有 $a\in S$ 使得 $a\circ a=a$
* **半群导出定理**：设 $<S,*>$ 是半群，$<T,\odot>$ 是一个代数系统，若有一同态满射 $\psi:S\rightarrow T$，则 $<T,\odot>$ 也是半群
* 设 $<S,*>$ , $<T,\odot>$ 是半群满同态，若 $<S,*>$ 是幺半群，则 $<T,\odot>$ 也是幺半群，满同态映射作用于 $S$ 的单位元的象是 $T$ 的单位元
* 设 $\psi$ 是半群 $(S,\circ)$ 到 $(T,\odot)$ 的同态，$\mu$ 是半群 $(T,\odot)$ 到 $(U,*)$ 的同态，则 $\mu$ 到 $\psi$ 的复合映射 $\mu\cdot\psi$ 是一个 $(S,\circ)$ 到 $(T,\odot)$ 的同态

### 交换半群

对代数系统 $<S,\circ>$ ，若对任意 $a,b,c\in S$，有 $(a\circ b)\circ = a\circ (b\circ c)$ 且 $a\circ b=b\circ a$ ，则称 $<S,\circ>$ 为**交换半群**

### 独异点 | 幺半群

#### 定义

若 $<S, \circ>$ 为半群，并且存在一个单位元 $e\in S$，对任意 $a\in S$ ，有 $a\circ e=e\circ a$ ，则 $<S,\circ,e>$ 为 **独异点** 或称 **幺半群**

#### 定理

* $<S,\circ, e>$ 为独异点或幺半群，则关于 $\circ$ 运算的运算表中，任何两行或两列都不相同
* $<S,\circ, e>$ 为独异点或幺半群，若对任意 $a,b\in S$ ，都有逆元，则
  * $(a^{-1})^{-1}=a$
  * $a\circ b$ 有逆元且 $(a\circ b)^{-1}=b^{-1}\circ a^{-1}$

### 循环独异点 | 循环幺半群

#### 定义

一个独异点 $<S,\circ, e>$ 称为 **循环独异点** 或 **循环幺半群**，若存在一个元素 $a\in S$ ，使得 $S$ 中每个元素 $b$ 都可以表示为：$a=a\circ a\circ a...\circ a=a^n,\ a^0=e$

也称 $<S,\circ>$ 是由元素 $a$ 生成的，$a$ 称为 $<S,\circ, e>$ 的生成元

#### 性质

对循环独异点 $<S,\circ,e>,\ <S,\circ>$ 为交换半群

### 子半群

#### 定义

设 $<S,\circ>$ 为一个半群，若

* $S^{'}\subseteq S$
* $<S^{'},\circ>$ 是半群

则称 $<S^{'},\circ>$ 是 $<S,\circ$ 的 **子半群**

#### 定理

$<S,\circ>$ 是一个半群，若 $S^{'}\subseteq S$，且 $S^{'}$ 关于 $\circ$ 运算封闭，则 $<S^{'},\circ>$ 是 $<S,\circ>$ 的子半群

### 半群同态和同构

#### 定义

设 $<S,*>$, $<T,\odot>$ 是半群，若存在一个映射 $\Psi:S\rightarrow T$，对任意 $a,b\in S$，都有 $\Psi(a*b)=\Psi(a)\odot\Psi(b)$，则称 $\Psi$ 为 **半群同态**

若 $\Psi$ 是满射，则称 $\Psi$ 是 **半群满同态**

若 $\Psi$ 是一一映射，则称 $\Psi$ 是 **半群同构**

若 $S$, $T$ 相同，则称 $\Psi$ 为 **自同态/自同构**

### 商半群

#### 定义

对半群 $<S,*>$，若 $R$ 是 $S$ 上的等价关系，则称 $<S/R,\oplus>$  为 $<S,*>$ 的商半群，运算 $\oplus$ 为 $*/R$ 

#### 定理

半群和它的商半群满同态

### 半群直积

#### 定义

设 $<S,*>$ , $<T,\odot>$ 是两个半群，其上的代数系统直积 $<S\times T,\otimes>$，其中 $\times$ 为 $S, T$ 的笛卡尔积，$\otimes$ 定义为对任意 $A_1,a_2\in S$, $b_1,b_2\in T$, $(a_1, b_1)\otimes (a_2,b_2)=((a_1*a_2),(b_1\odot b_2))$ 称为半群直积

#### 定理

* 半群直积也称为半群
* 交换半群的直积也是交换半群
* 幺半群的直积也是幺半群，并且其独异点（单位元） 是两个半群单位元的笛卡尔积
* 有零元的半群直积的零元是两个半群零元的笛卡尔积
* 有逆元的半群直积的逆元是两个半群逆元的笛卡尔积

## 群

### 群

#### 定义

设 $G$ 是一个非空集合， $\times$ 是儿园代数运算，如果满足以下条件：

* 代数运算 $\times$ 满足结合律，即对 $G$ 中的任意元素 $a,b,c$ 都有 $(a\times b)\times c=a\times(b\times c)$
* $G$ 中有 **左** 单位元 $e$，对于 $G$ 中的每个元素 $a$ 都有 $e\times a=a\times e=a$
* 对 $G$ 中每个元素 $a$，都有 **左** 逆元 $a^{-1}$，使得 $a^{-1}\times a=a\times a^{-1}=e$

则称 $G$ 对代数运算 $\times$ 作成一个 **群**

> 群 $\subseteq$ 独异点 $\subseteq$ 半群 $\subseteq$ 代数系统

#### 公理

群理论可以采用数理逻辑方法表示，它有三条公理

* 满足结合律

  $\forall x\in G,y\in G,z\in G$，有 $\forall x\forall y\forall z ((x\times y)\times z=x\times (y\times z))$

* 有单位元

  $\forall x\in G$，有 $\forall x(e\times x=x\times e = x)$

* 有逆元

  $\forall x\in G$，存在元素 $y\in G$ 使得 $\forall x\exists y(y\times x=x\times y=e)$

#### 性质

* 群 $<G,\times>$ 的左单位元也是有单位元，单位元唯一
* 群 $<G,\times>$ 中元素的左逆元也是右逆元，逆元唯一
* 群 $<G,\times>$ 无零元
* **消去律**：群 $<G,\times>$ 的运算 $\times$ 满足
  * 如果 $a\times x=a\times x^{'}$，则 $x=x^{'}$
  * 如果 $x\times a=x^{'}\times a$，则 $a=x^{'}$
* **唯一解**： 群 $<G,\times>$ ，$\forall a,b\in G$，必存在唯一的一个 $x\in G$，使得 $a\times x=b$，$x\times a=b$
* 群 $<G,\times>$ 中只有单位元是幂等元
* 群中运算满足消去律
  * $a\circ b=a\circ c\rightarrow b=c$
  * $a\circ c=b\circ c\rightarrow a=b$
* 群的每一行每一列都是群元素的双射
* 群 $G$，对任意 $a,b\in G$，都有：
  * $(a^{-1})^{-1}=a$
  * $(ab)^{-1}=b^{-1}a^{-1}$

#### 元素幂的性质

设 $G$ 为群，对于任意 $a,b\in G，有

* $a^ma^n=a^{m+n}$
* $(a^m)^n=a^{mn}$

#### 元素的阶

$|a|=k$，使得 $a^k=e$ 成立的最小正整数 $k$

有限群元素的阶一定是有限阶，且是群的阶的因子

#### 群的阶

一般情况下，群 $G$ 的阶 = 其因子的个数

当群 $G$ 为循环群时，群 $G$ 的阶为其生成元的阶

设有限群 $G$ 的阶是 $|G|$，任意元素 $a\in G$，$a$ 的阶是 $|a|$，则 $|a|$ 整除 $|G|$，即 $|a|\ |\ |G|$

### 特殊群

#### 有限群

给定群 $<G,\times>$ ，若 $G$ 是有限集，则称 $<G,\times>$ 是 **有限群**。元素个数称为群的阶，表示为 $|G|$

#### 无穷群

若集合 $G$ 是无穷的，则称 $<G,\times>$ 为 **无穷群**；阶无穷大

#### 平凡群

只含单位元的群称为 **平凡群**

#### 可交换群 (Abel 群)

给定群 $<G,\times>$ ，若 $\times$ 满足交换律，则称 $<G,\times>$ 是 **可交换群** 或 **Abel 群**

#### 克莱恩 (Klein) 四元集

| **$\circ$** |  e   |  a   |  b   |  c   |
| :---------: | :--: | :--: | :--: | :--: |
|    **e**    |  e   |  a   |  b   |  c   |
|    **a**    |  a   |  e   |  c   |  b   |
|    **b**    |  b   |  c   |  e   |  a   |
|    **c**    |  c   |  b   |  a   |  e   |

#### 模 N 集

$<I_N,\oplus>$ 称为模 N 整数加群，其中，$x\oplus y=(x+y)\mod{N}$

#### 交换群 (Abel 群)

##### 定义

若群 $G$ 的运算满足交换律，则称群 $G$ 为**交换群**，又称**阿贝尔群**

##### 定理

* 群 $G$ 是阿贝尔群 $\Leftrightarrow$ 对任意元素 $a,b\in G$，有 $(ab)^2=a^2b^2$

#### 循环群

##### 定义

群 $G$，存在一个 $a\in G$，使得群中任意元素可以表示为 $a$ 的幂，则群 $G$ 称为 **循环群**，$a$ 称为 $G$ 的生成元，记为 $G=(a)$

##### 循环群的阶

* 生成元的阶是无限的，则 $G$ 是无限循环群
* 生成元的阶是 $n$，则 $G$ 为 $n$ 阶群

##### 性质

* 循环群的生成元不一定是唯一的
* 有限群 $G=(a)$，若 $|G|=n$，则 $a^n=e$ 且 $G={a,a^2,...,a^n=e}$，其中 $e$ 是群 $G$ 的单位元

##### 定理

* 任何一个循环群必是交换群
* 群 $G=(a)$
  * 若 $G$ 是无限循环群，则 $G$ 的生成元是 $a$ 或 $a^{-1}$
  * 若 $|G|=n$，则 $G$ 有 $\phi(n)$ 个生成元，$\phi(n)$ 为 **欧拉函数**
* 在群 $G$ 中，$|a|=n$，则 $|a^k|=\frac{n}{(n,k)}$
* 任何一个循环群的子群也是循环群
* 无限循环群 $G$ 的子群除 $\{e\}$ 之外也是无限的
* 有限循环群 $G$，其子群的阶是群 $G$ 的因子，且 $G$ 有且仅有一个子群，其阶为该因子

#### 变换群

##### 定义

$A$ 为非空集合，$F(A)=\{f|f:A\rightarrow A 上的双射\}$，关于函数复合构成的群称为 **变换群**

##### 性质

任何群 $G$ 都和一个变换群 $S$ 同构

#### 置换群

##### 定义

有限群的变换群称为 **置换群**

##### 定理

每一个有限群都和一个置换群同构

### 置换与变换

#### 置换

##### 定义

置换 $\sigma$ 把 $a_1$ 变成 $\sigma(a_1)$，把 $a_2$ 变成 $\sigma(a_2)$，...，是一个一对一映射

##### 表示方式

$\begin{pmatrix}1 & 2&...&n\\ \sigma(1)&\sigma(2)&...&\sigma(n)\end{pmatrix}$

#### 轮换

置换 $\begin{pmatrix}i_1 & i_2&...&i_{k-1}&i_k&i_{k+1}&...&i_n\\ i_2&i_3&...&i_k&i_1&i_{k+1}&...&i_n\end{pmatrix}$ 称为 **k-轮换**

#### 恒等变换

k = 1 的 k-轮换称为 **恒等变换**

#### 对换

k = 2 的 k-轮换称为 **对换**

#### 定理

* 有限置换可表示为不相交的轮换积
* 不相交轮换可以表示为对换积

### 陪集

#### 定义

设 $H$ 是群 $G$ 的子群，$a\in G$，有 $aH=\{ax|x\in G\},\ Ha=\{xa|x\in G\}$ 则 $aH,Ha$ 分别称为群 $G$ 关于子群 $H$ 的一个左陪集，右陪集

#### 性质

* $a\in aH$
* $a\in H\Leftrightarrow aH=H$
* $b\in aH\Leftrightarrow aH=bH$
* $aH=bH\Leftrightarrow a^{-1}b\in H$ 或 $b^{-1}a\in H$
* $b\in aH\Leftrightarrow aH=bH\Leftrightarrow a^{-1}b\in H$ 或 $b^{-1}a\in H$
* 若 $aH\cap bH\not=\empty$，则 $aH=bH$
* 群 $G$ 上全体不同的左陪集构成群 $G$ 的元素分类，对任意 $a,b$ 若同属一类，当且仅当 $a^{-1}b\in H$ （或 $b^{-1}a\in H$）
* 对于右陪集，上条同理

#### 定理

* $H$ 是 $G$ 的子群，则 $R=\{<a,b>|a\in G,b\in G 且 a^{-1}b\in H\}$ 是 $G$ 中一个等价关系，对于 $a\in G$，记为 $[a]_R=\{x|x\in G 且 <a,x>\in R\}$，则有 $[a]_R=aH$

* 一个子群 $H$ 的所有陪集都与 $H$ 等势

  > 等势是数学术语，集合里的等势是指两个集合之间一一对应。

* **拉格朗日定理**：$H$ 是 $G$ 的子群，若 $G$ 是有限群，$|G|=n,\ |H|=m,\ (G:H)=k$，则 $n=km$

  > 群 $G$ 的阶为质数，则群 $G$ 无非平凡子群 / 真子群

* 群 $|G|=n$ ，对于任意 $a\in G,|a|$ 是 $n$ 的因子，且必有 $a$，$a^n=e$。若 $n$ 是质数，则群 $G$ 是循环群。

#### 指数

群 $G$ 的子群 $H$ 的所有不同左陪集（右陪集）的个数称为 $H$ 在 $G$ 里的指数，可表示为 $(G:H)$

### 不变子群

#### 定义

$G$ 的子群 $N$，若对于任意 $a\in G$，都有 $aN=Na$，则称 $N$ 为**不变子群**或正规子群。记为 $N\trianglelefteq G$，若 $N\not= G$，则记为 $N\triangleleft G$

#### 定理

若对任意 $a\in G$，$a^{-1}Na=aNa^{-1}=N$，则 $N\trianglelefteq G$

### 半群与群

* 对半群 $G$，若对任意 $a,b\in G$， 方程 $a x = b，x a = b$ 在 $G$ 中有解，则 $G$ 为群。
* 对有限半群 $G$ 无零元，若满足消去律，则 $G$ 为群

### 子群

#### 定义

$<S,\times>$ 称为群 $<G,\times>$ 的子群，若

* $S\subseteq G$
* $<S,\times>$ 是一个群

记为 $S\le G$

#### 性质

* 若 $|G|\gt1$ ，则群 $<G,\times>$ 至少有两个子群：

  * $<\{e\},\times>$
  * $<G,\times>$

  称为 $G$ 的 **平凡子群**，其他子群称为 **真子群 / 非平凡子群**，记为 $S\lt G$

* $<S, \times>$ 是群 $<G, \times >$ 的非空子群，则群 $S$ 的单位元就是群 $G$ 的单位元，$S$ 中任意元素 $a$ 的逆元也是 $a$ 在群 $G$ 中的逆元。

#### 定理

* 群 $G$ 的非空子集 $S$ 构成子群的充要条件是
  * 若 $a,b\in S$，则 $ab\in S$
  * 若 $a\in S$，则 $a$ 在 $G$ 中逆元 $a^{-1}\in G$
* 群 $G$ 的非空子集 $S$ 构成子群的充要条件是
  * 若 $a,b\in S$，则 $ab^{-1}\in S$
* 群 $G$ 的非空 **有限** 子集 $S$ 构成子群的充要条件是
  * 若 $a,b\in S$，则 $ab\in S$

