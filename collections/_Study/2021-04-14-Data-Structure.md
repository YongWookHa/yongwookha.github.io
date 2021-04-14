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

---

## Stack  
![](https://www.dropbox.com/s/196z8tpwsomjxz1/stack.png?raw=1)  
_First-In-Last-Out_ 특성이 있는 자료구조입니다. push와 pop Method를 이용하여 데이터를 입력하고 출력합니다.  Python에서는 `list`를 이용하여 Stack을 간단히 만들 수 있습니다. 

```Python
# Stack 구현 
class Stack:
    def __init__(self):
        self.stack = list()

    def push(self, data):
        self.stack.append(data)

    def pop(self):
        if self.stack:
            return self.stack.pop(len(self)-1)

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

---

## Queue

![](https://www.dropbox.com/s/ztar5b1ctv4r7oj/queue.png?raw=1)
Stack과 한 쌍을 이루는 데이터 구조입니다. _First-In-First-Out_ 특성이 있습니다. Enqueue와 Dequeue Method를 이용하여 데이터를 입력하고 출력합니다. 여기서 별도로 다루지는 않지만, Python의 강력한 내장함수 `list`를 이용하면 Stack, Queue 뿐만 아니라 Deque(양쪽에서 넣기, 빼기 가능한 구조)의 구현도 쉽게할 수 있습니다. 특히 Queue는 별도의 class로 구현할 필요 없이 Enqueue는 `append`, Dequeue는 `pop` method를 사용하면 됩니다.

```python
> s = list()  # queue와 거의 같음
> s
[]
> s.append(1)
> s.append(2)
> s.append(3)
> s.append(4)
> s
[1, 2, 3, 4]
> s.pop()
1
> s.append(s.pop())
> s
[3, 4, 2]
> s[0]  # front
3
> s[-1]  # back
2
```

## Linked List

각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식입니다. 시작 노드는 `Head`, 끝 노드는 `Tail`으로 부릅니다. 또한 노드들이 일방향으로 연결되어 있는지, 양방향으로 연결되어 있는지에 따라 `Single Linked List` 또는 `Double Linked List`로 나눌 수 있습니다.

### Single Linked List
![](https://www.dropbox.com/s/6oeedhpc45vycog/singly-linked-list.png?raw=1)  
단방향으로 연결된 Linked List입니다. `Head`에서 `Tail`로 Traverse는 가능하지만, 반대로 `Tail`에서는 `Head`를 찾아갈 수 없습니다. 따라서 `Head` Node를 알고 있어야 연결된 모든 Node에 접근이 가능합니다.

```Python
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

```Python
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

# Non-linear Data Structure  
__Comming Soon__
