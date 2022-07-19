---

title: "linked list"
categories: data-structure
excerpt: linked list
toc: true
toc_sticky: false
author_profile: false
date: 2022-07-19
---

## why 연결리스트?
![image](https://user-images.githubusercontent.com/96481582/179653975-f6cd2b03-13fb-48a7-b679-a22b218af095.png)

다음과 같은 배열이 있다고 할 때, $3$과 $4$사이에 $6$이라는 새로운 원소를 삽입한다고 했을 때, 배열을 복사하고 밀어서 붙여 넣는 것은 비효율적이다.  

$3$과 $4$ 사이의 관계를 끊어주고($3$옆에 $4$) $3$ 옆에 $6$, $6$ 옆에 $4$의 관계로 바꿔 주면 우리가 원하는 것을 만들 수 있다. 

## 연결 리스트의 구성과 종류

연결리스트는 Node로 이루어져 있는데, node는 value와 link라는 두 부분으로 구성 되어 있다.  

![image](https://user-images.githubusercontent.com/96481582/179655168-4c55fc25-aa05-46f3-a1a3-88c6e643ca12.png)

이때 첫 번째 노드를 가리키는 포인터가 있는데 이를 head라고 한다.   
또한 node들의 연결 방식에 따라 __단일 연결 리스트, 이중 연결 리스트, 원형 연결 리스트, 이중 원형 연결 리스트__ 로나눌 수 있다.  

### 단일 연결 리스트(singly linked list)

우리가 생각하는 일반의 연결 리스트이다.  

![image](https://user-images.githubusercontent.com/96481582/179655931-483455ff-dbed-4252-9350-9a3167bc0544.png)

```c
typedef struct _Node
{
    int value;
    struct _Node *next;
}Node;
```

### 이중 연결 리스트 (doubly linked list)
이중 연결 리스트의 노드에는 다음 노드 뿐만 아니라, 이전 노드를 가리키는 포인터가 있다. 

![image](https://user-images.githubusercontent.com/96481582/179674944-43ebf10c-3955-44bb-a11f-e8434da946ff.png)
```c
typedef struct _Node
{
    int value;
    struct _Node *next;
    struct _Node *prev;
}Node;
```

### 원형 연결 리스트 (circular linked list)

원형 연결리스트는 단일 연결 리스트에서 마지막 노드의 포인터가 첫번째 노드를 가리킨다.  

![image](https://user-images.githubusercontent.com/96481582/179658413-2fca7431-6385-4325-a144-fcac21e53815.png)

```c
typedef struct _Node
{
    int value;
    struct _Node *next;
}Node;
```

### 이중 원형 연결 리스트

이중 원형 연결 리스트는 이중 연결 리스트에서 첫번째 노드의 `prev` 는 마지막 노드를 가리키고 마지막 노드의 `next`는 첫번째 노드를 가리킨다. 
![image](https://user-images.githubusercontent.com/96481582/179659354-c5f9f18a-79d1-433c-b73d-2d6c00cf1ba6.png)

```c
typedef struct _Node
{
    int value;
    struct _Node *next;
    struct _Node *prev;
}Node;
```

## 단일 연결 리스트 구현

### 시작에 원소 삽입하기 

```c
void insert(Node **PtrHead, int value)
{
    Node *tempnode = (Node *)malloc(sizeof(Node));
    tempnode->value = value;
    tempnode->next = *PtrHead;
    *PtrHead = tempnode;
    return;
}
```
```c
void print(Node *head)
{
    while (head)
    {
        printf("%d\n", head->value);
        head = head->next;
    }
}
```
```c
int main()
{
    Node *head = NULL;
    insert(&head, 1);
    insert(&head, 2);
    print(head);
}
```

`insert()` 를 그림으로 보면 

```c
insert(&head, 1);
```
`main`함수의 `head` 가 `insert()`로 넘어옴(call by reference)

```c
Node *tempnode = (Node *)malloc(sizeof(Node));
    tempnode->value = value;
```

<center><img src = https://user-images.githubusercontent.com/96481582/179668065-bd3abc8b-fa68-445a-929e-4e59d040f97c.png width=80% height=80%></center>

```c
tempnode->next = *PtrHead;
```

<center><img src=https://user-images.githubusercontent.com/96481582/179669548-60211977-9033-4e71-ab7d-baf88c4aa097.png width=80% height=80%></center>

```c
*PtrHead = tempnode;
```
![image](https://user-images.githubusercontent.com/96481582/179669704-67d86aa6-5008-46cf-a2b7-573fdbb51e0d.png)

### 끝에 원소 삽입하기 
`head`를 움직여서 `head`의 다음이 `NULL`을 가리킬때까지 움직인다.  

```c
void insertEnd(Node **PtrHead, int value)
{
    Node *head = *PtrHead;
    Node *tempnode = (Node *)malloc(sizeof(Node));
    tempnode->value = value;
    tempnode->next =NULL;

    if(head == NULL){
        tempnode->next = *PtrHead;
        *PtrHead = tempnode;
    }
    while(head->next != NULL){
        head = head->next;
    }
    tempnode->next = head->next;
    head->next = tempnode;
}
```

### 특정 값을 갖고 있는 노드 삭제하기

```c
void deleteNode(Node **PtrHead, int delValue){
    Node *curr = *PtrHead;
    Node *nextNode;
    if(curr &&curr->value == delValue){
        *PtrHead = curr->next;
        free(curr);
        return;
    }
    while(curr != NULL){
        nextNode = curr->next;
        if(nextNode && nextNode->value == delValue){
            curr->next = nextNode->next;
            free(nextNode);
            return;
        }
        else{
            curr = nextNode;  
        }
    }
}
```
![image](https://user-images.githubusercontent.com/96481582/179673569-162fb02d-2f77-49a4-bbe2-e8c5876b829c.png)

다음과 같은 상황에서 
```c
deletenode(&head, 3);
```
을 생각해보자.  

`nextNode`의 `value`가 $3$이 될 때까지, `curr`과 `nextNode`가 움직인다. 일치하게 된다면 $3$인 노드를 삭제하기 전에 $2$와 $4$를 연결 해주어야 한다.  

```c
curr->next = nextNode->next;
```

![image](https://user-images.githubusercontent.com/96481582/179674423-98f3969f-6977-40c1-a3ea-7c9d3a6cb882.png)

이제 $3$인 노드를 삭제 해주면 된다.  

```c
free(nextNode)
```

