---
title: "DataStructure, Linked List"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Linked List]

permalink: /DataStructure/Linked List

toc: true
toc_sticky: true

date: 2026-04-16
last_modified_at: 2026-04-16
---

# DataStructure(Linked List)
***
   
## Linked List
   
데이터를 들고있는 노드(Ndoe)끼리 포인터를 통해 서로 연결된 선형 자료구조입니다.
   
각 노드는 저장할 데이터, 다음 노드의 주소를 가리키는 포인터로 구성됩니다.
   
배열이 메모리상 연속적으로 배치되는 것과 다르게 링크드리스트는 메모리의 여러 곳에 
흩어져서 배치되게 됩니다. 포인터에 의해서 논리적 순서를 유지하는 형태입니다.
   
링크드리스트는 단일, 이중, 원형 3가지가 있습니다.
   
### 단일 링크드리스트(Singly Linked List)
   
가장 기본적인 링크드리스트입니다. 각 노드가 데이터와 다음 노드를 가리키는 하나의 포인터(Next)만을 가집니다. 
   
데이터의 흐름이 단방향으로만 진행되며 마지막 노드의 포인터는 NULL을 가리켜 리스트의 끝임을 확인할 수 있도록 합니다.
   
사용 예시는 다음과 같습니다.
   
- 스택 자료구조의 연결 리스트 구현
- 단방향 데이터 스트리밍 처리
- 단순 선형 데이터 저장 및 순차 처리
   
구현 예시는 다음과 같습니다.
   
```C++
struct SNode
{
	int data;
	SNode* next;
	SNode(int v) : data(v), next(nullptr) {}
};

class SinglyLinkedList
{
	SNode* head = nullptr;

public:
	void Add(int v)
	{
		SNode* n = new SNode(v);
		n->next = head;
		head = n;
	}
	~SinglyLinkedList()
	{
		while (head)
		{
			SNode* t = head;
			head = head->next;
			delete t;
		}
	}
};
```

클라 기준으로는 엔진 내에서 이벤트 리스너(Event Listener) 목록을 관리할 때 사용합니다.
   
이벤트 발생 시 등록 객체들에 순차적으로 알림을 넣는 구조에서 역방향 탐색이 필요하지 않아서 메모리 효율을 
위해서 단일 링크드리스트를 쓰곤합니다.
   
클라 구현 예시는 다음과 같습니다.
   
```C++
struct SElement
{
	int value;
	int nextIndex = -1;
};

class ClientSLL
{
	std::vector<SElement> pool;
	int head = -1;

public:
	void push(int v)
	{
		SElement elem { v, head };
		pool.push_back(elem);
		head = (int)pool.size() -1;
	}
};
```
   
- 이중 링크드리스트(Doubly Linked List)
   
이중 링크드리스트에서는 각 노드가 다음 노드(Next) 및 이전 노드(Prev)를 가리키는 포인터를 각각 가집니다.
   
양방향으로 자유롭게 이동할 수 있기에 삽입과 삭제 연산 시 이전 노드의 주소를 찾기위한 추가 순회가 요구되지 않습니다.
   
사용 예시는 다음과 같습니다.
   
- 웹 브라우저 앞/뒤 페이지 이동 기록
- 텍스트 에디터 커서 이동 및 수정 이력
- LRU(Least Recently Used) 캐시 알고리즘
   
구현 예시는 다음과 같습니다.
   
```C++
struct DNode
{
	int data;
	DNode *prev, *next;
	DNode(int v) : data(v), prev(nullptr), next(nullptr) {}
};

class DoublyLinkedList
{
	DNode *herad = nullptr, *tail = nullptr;

pulbic:
	void Append(int v)
	{
		DNode* n = new DNode(v);
		if (!head)
		{
			head = tail = n;
		}
		else
		{
			tail->next = n;
			n->prev = tail;
			tail = n;
		}
	}
}
```
   
클라 기준으로는 씬 그래프 내의 형제 노드 관리나 대규모 오브젝트 매니저에서 삭제가 빈번하게 나올 때 사용됩니다.
std::List가 더블 링크드리스트 구조로 되어있습니다.
   
특정 요소를 반복자(Iterator)를 통해 즉시 삭제해야 하는 경우 O(1)의 성능으로 사용가능합니다.
   
클라 구현 예시는 다음과 같습니다
   
```C++
class ListNode // 객체 자체가 노드가 되어 추가 할당을 없애는 방식
{
public:
	ListNode *prev = nullptr, *next = nullptr;
	virtual ~ListNode() = default;
};

clas RenderObject : public ListNode
{
public:
	void Render() { /* 렌더링 로직 */ }
};
   
class RenderQueue
{
	ListNode* head = nullptr;

public:
	void Add(ListNode* obj) // 동적할당 없이 포인터 연결
	{
		if (head) { obj->next= head; head->prev = obj; }
		head = obj;
	}
};
```
   
- 원형 링크드리스트(Circular Linked List)
   
마지막 노드가 다시 첫 번째 노드(head)를 가리키는 구조입니다. 리스트의 끝이 없고 어느 노드에서 시작해도 
모든 노드에 도달 가능합니다. 단일, 이중 링크드리스트 구조에 모두 적용 가능합니다.
   
사용 예시는 다음과 같습니다.
   
- Round-Robin CPU 스케줄링
- 멀티플레이어 게임 내 플레이어 턴 관리
- 지속적으로 순환하는 버퍼(Circular Buffer)
   
구현 예시는 다음과 같습니다.
   
```C++
struct CNode
{
	int data;
	CNode = next;
	CNode(int v) : data(v), next(nullptr) {}
};

class CircularList
{
	CNode* tail = nullptr; // 마지막 노드가 head를 가리키므로 tail만 관리하면 됨

public:
	void Add(int v)
	{
		CNode* n = new CNode(v);
		if (!tail)
		{
			tail = n;
			n->next = n;
		}
		else
		{
			n->next - tail->next;
			tail->next = n;
			tail = n;
		}
	}
};
```
   
클라 기준으로는 UI 시스템에서 루프 형태의 인벤토리 슬롯 이동이나, 무한 스크롤 뷰를 구현 시 사용할 수 있습니다.
   
게임 내 턴제 전투 시스템에서 마지막 유저의 턴이 끝나고 다시 첫 번째 유저로 돌아가는 로직을 포인터 연산으로 구현 가능합니다.
   
클라 구현 예시는 다음과 같습니다.
   
```C++
class Player; // 전방 선언

struct TurnNode
{
	Player* player;
	TurnNode* next = nullptr;
};

class TurnSystem
{
	TurnNode* currentTurn = nullptr;

public:
	void NextTurn()
	{
		if (currentTurn)
		{
			currentTurn = currentTurn->next;
		}
	}

	Player* GetCurrentPlayer()
	{
		return currentTurn->player;
	}
}
```
   
>생성자 멤버 초기화 리스트
   
이중 링크드리스트의 코드 기준 해석입니다.
   
```C++
public:
	ListNode *prev = nullptr, *next = nullptr;
```
   
- DNode(int v)
   
생성자의 선언부에 해당합니다. 
외부에서 int 타입 인자 v를 전달받습니다.
   
- 콜론(:)
   
초기화 리스트 시작을 표시하는 구분자입니다.
   
- data(v)
   
멤버 변수 data를 매개변수 v의 값으로 초기화 합니다.
   
- prev(nullptr), next(nullptr)
   
포인터 멤버 변수들을 nullptr로 초기화합니다.
   
- {}
   
생성자의 본문(body)에 해당합니다. 초기화 리스트에서 전체 초기화가 완료되었고 추가 로직이 없어 비워둡니다.
   
   
멤버 초기화 리스트의 연산 순서는 초기화 리스트에 적힌 순서와 관계없이 클래스/구조체 선언부에서 
멤버 변수가 선언된 순서에 따라서 초기화를 진행합니다.
   
```C++
struct DNode
{
	DNode* prev;
	int data;
	DNode* next;
};
```
   
위의 구현 순서를 따릅니다. prev -> data -> next 순서대로 CPU에서 처리됩니다.
객체를 없애는 소멸자 호출 시 역순으로 없애야 하는데 이 기준 오프셋으로 클래스/구조체 선언부 내 선언 순서대로 가는 방식이빈다.
   
생성자 본문인 {} 내부의 경우 객체 생성 후의 행동입니다. 초기화리스트는 객체 생성 시의 형태입니다.
      
```C++
DNode(int v)
{
	data = v;
	prev = nullptr;
	next = nullptr;
}
```
   
위와 같은 형태의 코드가 생성자 멤버 초기화 리스트와 동일하다고 볼 수 있습니다.
   
위 코드는 생성자 내 대입 방식이며 이 경우 
   
1) 멤버 변수 기본값 초기화(Default Initialization)
   
2) 생성자 본문에서 값 대입(Assignment)
   
두 번의 연산을 수행합니다.
   
생성자 멤버 초기화 리스트 사용 시 멤버 변수가 메모리에 배치되는 때 바로 값을 할당하는 
직접 초기화(Direct Initialization) 방식입니다. 기본 값 할당이 생략되어 성능상 이득입니다.
   
초기화 리스트가 필수적인 케이스가 세 가지 정도 있습니다.

- cosnt 멤버 변수
   
선언과 동시에 초기화되어야 합니다. 생성된 이후 값 변경이 불가능합니다. 
생성자 본문 { }에서의 대입이 불가능한 형태입니다.
   
- 참조(Reference) 멤버 변수
   
void UDamageSystem::ProcessDamage(const FDamageContext& Context) 같은 참조자는 생성 시점에 
대상이 정해져야 합니다.
   
- 기본 생성자가 없는 클래스 타입 멤버
   
멤버 변수가 다른 클래스 객체인데 그 클래스에 인자 없는 생성자가 존재한다면 
초기화 리스트에서 명시적으로 인자를 전달해야 합니다.