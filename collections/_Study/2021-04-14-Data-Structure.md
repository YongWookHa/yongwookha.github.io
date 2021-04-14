---
layout: post
title: 자료 구조 되돌아보기
subtitle: Data Structure with Python3
tags: [STUDY, DEVELOP]
cover-img: /assets/img/datastructure_banner.jpg
comments: true
---

실력있는 요리사는 항상 칼을 날카롭게 유지합니다. 실력있는 개발자도 항상 기본기가 무뎌지지 않게 노력해야겠죠? 취업하고 나서는 따로 시간내서 살펴보지 않았던 **자료구조**를 곱씹어보며 포스팅을 통해 간단히 정리해봅니다.

# 자료구조  
> 자료구조(資料構造, 영어: data structure)는 컴퓨터 과학에서 효율적인 접근 및 수정을 가능케 하는 자료의 조직, 관리, 저장을 의미한다. 더 정확히 말해, 자료 구조는 데이터 값의 모임, 또 데이터 간의 관계, 그리고 데이터에 적용할 수 있는 함수나 명령을 의미한다. 신중히 선택한 자료구조는 보다 효율적인 알고리즘을 사용할 수 있게 한다. 이러한 자료구조의 선택문제는 대개 추상 자료형의 선택으로부터 시작하는 경우가 많다. 효과적으로 설계된 자료구조는 실행시간 혹은 메모리 용량과 같은 자원을 최소한으로 사용하면서 연산을 수행하도록 해준다.  
> -wikipedia

자료구조는 Linearity에 따라 다음과 같이 분류될 수 있습니다.  

![](https://www.dropbox.com/s/r6iykcm5uyc17k1/dataStructure.png?raw=1)  

위 그림의 각 leaf node들을 순서대로 확인하고, Python으로 직접 구현해보면서 가물가물했던 개념들을 되짚어봅시다.

---

# Linear Data Structure   
## Array  

배열. 프로그래머라면 생활과 같이 쓰는 자료구조이므로 길게 설명하지 않고 생략합니다.  Python에서는 `list` 내장함수를 이용합니다.  

![](https://www.dropbox.com/s/ahkyt3orqvvges9/array.jpg?raw=1)  

```python
> x = [1, 2, 3, 4, 5]  # list
> x.append(6)
> x
[1, 2, 3, 4, 5, 6]
> x.pop(0)
1
> x
[2, 3, 4, 5, 6]
> [0, 1] + x
[0, 1, 2, 3, 4, 5, 6]
> x[1:4]
[3, 4, 5]
> x[1:-1]
[3, 4, 5]
> sorted(x, reverse=True)
[6, 5, 4, 3, 2]
```

## Stack  

![](https://www.dropbox.com/s/196z8tpwsomjxz1/stack.png?raw=1)  

_First-In-Last-Out_ 특성이 있는 자료구조입니다. push와 pop Method를 이용하여 데이터를 입력하고 출력합니다.  Python에서는 `list`를 이용하여 Stack을 간단히 만들 수 있습니다. 

```python
# Stack 구현 
class Stack:
    def __init__(self):
        self.stack = list()

    def push(self, data):
        self.stack.append(data)

    def pop(self):
        if self.stack:
            return self.stack.pop()

    def __repr__(self):
        return str(self.stack)

    def __len__(self):
        return len(self.stack)
```  

```python
> s = Stack()
> s
[]
> s.push(1)
> s.push(2)
> s.push(3)
> s.push(4)
> s
[1, 2, 3, 4]
> s.pop()
4
> s.push(s.pop())
> s
[1, 2, 3]
```  


## Queue

![](https://www.dropbox.com/s/ztar5b1ctv4r7oj/queue.png?raw=1)  

Stack과 한 쌍을 이루는 데이터 구조입니다. _First-In-First-Out_ 특성이 있습니다. Enqueue와 Dequeue Method를 이용하여 데이터를 입력하고 출력합니다. Stack과 마찬가지로 Python의 강력한 내장함수 `list`를 이용하면 Queue를 쉽게 구현할 수 있습니다.

```python
# Queue 구현 
class Queue:
    def __init__(self):
        self.q = list()

    def Enqueue(self, data):
        self.q.append(data)

    def Dequeue(self):
        if self.q:
            return self.q.pop(0)

    def __repr__(self):
        return str(self.q)

    def __len__(self):
        return len(self.q)
```  

```python
> s = Queue()
> s
[]
> s.Enqueue(1)
> s.Enqueue(2)
> s.Enqueue(3)
> s.Enqueue(4)
> s
[1, 2, 3, 4]
> s.Dequeue()
1
> s.Enqueue(s.Dequeue())
> s
[3, 4, 2]
```

## Linked List

각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식입니다. 시작 노드는 `Head`, 끝 노드는 `Tail`으로 부릅니다. 또한 노드들이 일방향으로 연결되어 있는지, 양방향으로 연결되어 있는지에 따라 `Single Linked List` 또는 `Double Linked List`로 나눌 수 있습니다.

### Single Linked List  

![](https://www.dropbox.com/s/6oeedhpc45vycog/singly-linked-list.png?raw=1)  

단방향으로 연결된 Linked List입니다. `Head`에서 `Tail`로 Traverse는 가능하지만, 반대로 `Tail`에서는 `Head`를 찾아갈 수 없습니다. 따라서 `Head` Node를 알고 있어야 연결된 모든 Node에 접근이 가능합니다.  

```python  
class SingleLinkedListNode:
    def __init__(self, data, next):
        self.data = data
        self.next = next

    def __repr__(self):
        data = [str(self.data)]
        n = self.next
        while n:
            data.append(str(n.data))
            n = n.next
        return '->'.join(data)
```  

```python
> s = SingleLinkedListNode(0, None)  # Tail Node
> for i in range(1, 6):
>   s = SingleLinkedListNode(i, s)
>
> s  # Head Node
5->4->3->2->1->0
```  

### Dobule Linked List
![](https://www.dropbox.com/s/dumumzrz5cd3zhy/doubly-linked-list.png?raw=1)  

양방향으로 연결된 Linked List입니다. `Head`에서 `Tail`으로, `Tail`에서 `Head`로도 Traverse 가능합니다. 하나의 Node만 알고 있으면 연결된 모든 Node에 접근 가능합니다.

```python  
class DoubleLinkedListNode:
    def __init__(self, data, prev, next):
        self.data = data
        self.prev = prev
        self.next = next

    def __repr__(self):
        data = ['*'+str(self.data)+'*']
        n = self.next
        while n:
            data.append(str(n.data))
            n = n.next
        p = self.prev
        while p:
            data = [str(p.data)] + data
            p = p.prev

        return '<->'.join(data)
```  

```python
> t = DoubleLinkedListNode(0, None, None)  # Tail Node
> for i in range(1, 6):
>   p = DoubleLinkedListNode(i, None, t)
>   t.prev = p
>   t = p
>
> t  # Head Node
*5*<->4<->3<->2<->1<->0
> t.next
5<->*4*<->3<->2<->1<->0
> t.next.next.prev
5<->*4*<->3<->2<->1<->0
```  

---

# Non-linear Data Structure   
## Graphs

![](https://www.dropbox.com/s/3kccg4d5hk48qjx/undirectedgraph.png?raw=1)

`Edge`와 `Vertice`로 이루어진 데이터 구조입니다. `Edge`에 방향이 있는지 여부에 따라 `Directed` Graph와 `Undirected` Graph로 나뉘어집니다. 아래에서는 Edge에 값이 없는 단순한 `Undirected` Graph를 구현해봅니다. 

```python
class Vertice:
    def __init__(self, name):
        self.name = name
        self.edges = []
    
    def connect(self, v):
        self.edges.append(v)

class Graph:
    def __init__(self):
        self.vertices = dict()
    
    def add_vertice(self, name):
        if name not in self.vertices:
            self.vertices[name] = Vertice(name)
        else:
            return "input name already exists in the graph"

    def add_edge(self, n1, n2):
        if n1 not in self.vertices or n2 not in self.vertices:
            return "input name not exists in the graph"
        else:
            self.vertices[n1].connect(self.vertices[n2])
            self.vertices[n2].connect(self.vertices[n1])

    def repr_vertices(self):
        return {k:[x.name for x in v.edges] for k, v in self.vertices.items()}

```

```python
> g= Graph()
> g.add_vertice('a')
> g.add_vertice('b')
> g.add_vertice('c')
> g.add_vertice('d')
> g.add_edge('a', 'b')
> g.add_edge('a', 'c')
> g.add_edge('b', 'c')
> g.add_edge('a', 'd')
>
> g.repr_vertices()
{'a': ['b', 'c', 'd'], 'b': ['a', 'c'], 'c': ['a', 'b'], 'd': ['a']}
```

Graph를 이용하면 실제 삶에서 발생하는 문제들을 컴퓨터로 가져올 수 있습니다. 가장 대중적인 예시는 세일즈맨이 도시를 순회하며 영업을 할 때, 가장 효율적인 경로를 찾는 문제 등입니다. TSP(Travelling Salesman Problem)라고 알려진 이 문제는 간단한듯하지만 _NP-Problem_ 으로 매우 복잡한 문제입니다. 이 문제에 접근할 때도 Edge에 값(도시간 거리)이 있는 Graph를 주로 이용하게 됩니다. 

이 이외에도 Network Routing Algorithm 등의 매우 많은 실생활 문제와 닿아있는 Graph이기에 **Graph Theory** 라는 Graph의 성질을 연구하고, Graph위에서 생기는 문제들을 조금 더 심오하게 탐구하는 별도의 학문으로 확장되기도 합니다.

참고 : [그래프 이론 기초 - 데이터 사이언스 스쿨](https://datascienceschool.net/03%20machine%20learning/17.01%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EC%9D%B4%EB%A1%A0%20%EA%B8%B0%EC%B4%88.html)  

## Trees

![](https://www.dropbox.com/s/5ii05s8cv22cn47/tree.png?raw=1)  

Tree는 계층 구조를 가지는 특징이 있는 Graph의 일종입니다. `Node`와 `Edge`로 이뤄져있으며, 계층 가장 상위에 위치한 Node는 Root라고 하며, 가장 하위에 있는 Node는 Leaf라고 합니다. Edge로 연결된 두 Node는 `Parent`와 `Child`로 관계를 가집니다. Root Node 이외의 모든 Node는 단 하나의 Parent Node가 있습니다.

Binary Tree는 Child Node가 최대 2개로 구성된 Tree입니다. 그리고 Binary Search Tree는 Binary Tree의 한 종류입니다. Binary Search Tree는 Node의 값을 기준으로 Child의 위치가 정해집니다. 어떤 Node에 연결된 두 Child 중, 왼쪽 Child는 더 작은 값, 오른쪽 Child는 더 큰 값을 가집니다. 아래는 Binary Search Tree의 구현 예시입니다.

```python
class Node:
    def __init__(self, item, parent):
        self.item = item
        self.parent = parent
        self.lchild = None
        self.rchild = None

class BinarySearchTree:
    def __init__(self, item):
        self.root = Node(item, None)
        self.length = 0
    
    def add_item(self, item):
        p = self.root
        while True:
            d = p.item
            if item <= d:
                if p.lchild:
                    p = p.lchild
                else:
                    p.lchild = Node(item, p)
                    break
            else:
                if p.rchild:
                    p = p.rchild
                else:
                    p.rchild = Node(item, p)
                    break
        self.length += 1
    
    def search_item(self, item):
        res = []
        p = self.root
        for _ in range(self.length+1):
            d = p.item
            if d == item:
                return res
            elif item < d:
                p = p.lchild
                res.append('left')
            else:
                p = p.rchild
                res.append('right')
```  

![](https://www.dropbox.com/s/lvi0pmwpkbkamh0/binaryTreeExample.png?raw=1)  

위의 그림과 같은 Binary Tree를 만들어봅니다. `search_item` Method는 Root에서 출발하여 입력 item이 있는 Node까지 도달하는 방향을 출력합니다.

```python
> tree = BinarySearchTree(7)
> tree.add_item(3)
> tree.add_item(1)
> tree.add_item(5)
> tree.add_item(8)
> tree.add_item(10)
>
> tree.search_item(5)
['left', 'right']
> tree.search_item(1)
['left', 'left']
> tree.search_item(8)
['right']
```
