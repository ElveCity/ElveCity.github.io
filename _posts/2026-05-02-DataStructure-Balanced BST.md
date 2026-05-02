---
title: "DataStructure, Balanced BST"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Balanced BST]

permalink: /DataStructure/Balanced BST

toc: true
toc_sticky: true

date: 2026-05-02
last_modified_at: 2026-05-02
---

# DataStructure(Balanced BST)
***
   
## Balanced BST
   
이진 탐색 트리는 좌우 서브트리의 균형이 맞지 않을 경우 최악의 시간 복잡도 O(N)을 가집니다.
   
데이터가 정렬된 상태로 삽입될 때, 트리가 한쪽으로 편향외어 연결리스트와 동일한 형태가 되어서입니다.
   
이걸 막으려고 나온게 균형 이진 탐색 트리(Balanced Binary Search Tree)입니다.
   
시간 복잡도는 탐색, 삽입, 삭제에서 O(logN)을 가집니다.
   
균형 이진 탐색 트리는 두 가지가 있습니다.
   
### 1) AVL Tree(Adelson-Velsky and Landis Tree)
   
가장 먼저 나온 스스로 균형을 잡는 이진 탐색 트리입니다. 주요 제한 조건은 
모든 노드에 왼쪽 서브트리와 높이 차이(Balance Factor, BF)가 최대 1을 넘지 않아야 한다는겁니다.
   
이 BF가 2이상일 시 회전 연산이 발생합니다.
   
회전 연산 케이스는 네 가지가 있습니다.
   
1) LL(Left-Left) Case
   
부모 노드의 왼쪽 자식 노드의 왼쪽 서브트리에 데이터가 추가된 상황입니다.
   
불균형이 발생한 기준 노드를 우회전시켜 균형을 복구합니다.
   
2) RR(Right-Right) Case
   
오른쪽 자식 노드의 오른쪽 서브트리에 데이터가 추가된 상황입니다.
   
기준 노드를 좌회전 시킵니다.
   
3) LR(Left-Right) Case
   
왼쪽 자식의 오른쪽 서브트리에 데이터가 추가된 상황입니다.
   
자식 노드를 기준으로 먼저 좌회전 수행으로 LL Case 구조로 변환 후에 부모 노드를 기준으로 우회전하는 이중 회전을 수행합니다.
   
4) RL(Right-Left) Case
   
오른쪽 자식의 왼쪽 서브트리에 데이터가 추가된 상황입니다.
   
자식 노드 우회전 후 부모 노드 좌회전 수행합니다.
   
AVL Tree는 구조적 균형이 좋아서 탐색 성능이 정말 좋습니다.
   
데이터 삽입이나 삭제 시 루트 노드까지 백트래킹하면서 다수의 회전 연산이 나올 수 있어서 
쓰기보다는 읽기 연산 비율이 높은 시스템에 씁니다.
   
### 2) Red-Black Tree
   
AVL 트리 조건을 좀 완화해서 잦은 삽입 및 삭제 시 발생하는 회전 연산의 오버헤드 감소에 
O(logN)의 성능을 낼 수 있도록 만든 자료구조입니다.
   
Red-Black Tree 요구 조건이 다섯 가지 있습니다.
   
1) 모든 노드는 Red나 Black이다
   
2) 트리의 루트노드는 반드시 Black이다
   
3) 모든 리프(Leaf)노드(보통 데이터 넣지 않는 NIL노드로 구현, 더미 노드)는 Black이다
   
4) Red노드의 자식 노드는 반드시 Black이다. 따라서 Red노드는 경로상 연속으로 나타날 수 없다(Double Red 금지)
   
5) 임의의 노드에서 그 노드의 하위 리프 노드(NIL)들까지 이어지는 모든 단순 경로는 동일한 개수의 Black 노드를 포함해야 한다.
   
위의 4, 5 조건으로 루트에서 가장 먼 리프 노드까지의 경로는 Red노드와 Black노드가 번갈아 나오는 경우이고 
가장 짧은 경로는 Black노드로만 이루어진 경우입니다.
   
따라서 최장 경로의 길이는 최단 경로의 길이의 2배를 초과할 수 없습니다.
   
> h <= 2log_2(N+1)
   
삽입되는 새로운 노드는 기본적으로 Red색상을 가지며 Double Red 금지 조건이 위반될 수 있습니다.
   
따라서 Uncle노드의 색상에 따라 두 가지 방식으로 해결합니다.
   
1) Recoloring(Uncle이 Red인 경우)
   
부모 노드와 삼촌 노드를 Black으로, 조부모 노드를 Red로 합니다.
   
이 연산은 트리의 구조적 변경(포인터 수정)없이 색상 비트만 변경하기에 오버헤드가 적으나 
상위 노드로 Double Red위반 전파의 가능성이 있습니다.
   
2) Restructuring(Uncle이 Black이거나 없는 경우)
   
AVL 트리의 회전(LL, RR, LR, RL)과 같은 방식으로 포인터 회전을 1~2회 수행한 뒤, 부모 노드와 조부모 노드의 색을 교체합니다.
   
이 경우 위반 사항이 상위로 전파되지 않습니다.
   
균형 이진 탐색 트리의 사용 예시는 다음과 같습니다.
   
1) C++ STL
   
std::map, std::multimap, std::set, std::multiset 컨테이너의 내부 구현은 모두 Red-Black 트리입니다.
   
사용자가 키를 입력할 때마다 바로 정렬을 수행하며 반복자(Iterator)를 통한 중위 순회(Inorder Traversal)시 
오름차순 데이터 접근을 O(1)의 다음 노드 탐색 비용으로 제공할 수 있습니다.
   
2) 운영체제 프로세스 스케줄링(Linux CFS)
   
리눅스 커널의 기본 스케줄러인 Completely Fair Scheduler는 실행 대기중인 프로세스 관리용으로 Red-Black 트리를 사용합니다.
   
프로세스의 누적 실행 시간(vruntime)을 키로 써서 트리에 배치합니다.
   
스케줄러는 트리의 가장 왼쪽 하단 노드(가장 실행 시간이 적은 프로세스)를 O(1)나 O(logN)의 비용으로 추출, CPU 할당 공정성 유지를 수행합니다.
   
3) 리눅스 가상 메모리 관리(VMA)
   
리눅스 커널은 프로세스가 사용하는 메모리 영역(Virtual Memory Area, vm_area_struct)의 시작 주소와 끝 범위 
관리를 위해서 Red-Black 트리를 사용합니다.
   
페이지 폴트 발생 시 해당 주소가 유효 메모리 영역에 속하는지 검색할 때 O(logN)속도로 최적화합니다.
   
4) 데이터베이스 인메모리 인덱싱
   
디스크 I/O 최소화를 위해 다진 트리(B-Tree, B+Tree)를 사용하는 디스크 기반 DB와 다르게 
메모리 내부에서만 동작하는 일부 특수 목적의 DB나 이전 시스템에서는 AVL 트리를 인덱스 구조로 사용하기도 했습니다.

균형 이진 탐색 트리 구현 예시는 다음과 같습니다.
   
```C++
#include <algorithm>

template <typename T>
struct AVLNode
{
	T Data;
	int Height;		// 균형 인수(BF) 계산을 위한 서브트리 높이 정보
	AVLNode* Left;
	AVLNode* Right;

	AVLNode(T InData) : Data(InData), Height(1), Left(nullptr), Right(nullptr) {} 
};

// nullptr 예외 처리 포함 높이 반환 인라인 함수
template <typename T>
inline int GetHeight(AVLNode<T>* Node)
{
	return Node ? Node->Height : 0;
}

// LL Case 발생 시 수행 우회전 알고리즘
// 회전 축이 되는 최상단 노드 Y를 매개변수로 받아서 회전 후 새로운 루트 노드 X 반환
template <typename T>
AVLNode<T>& RightRotate(AVLNode<T>* Y)
{
	AVLNode<T>* X = Y->Left;		// Y의 왼쪽 자식 노드를 새로운 루트 X로 설정
	AVLNode<T>* T2 = X->Right;		// 백업해둔 T2는 Y의 왼쪽 자식으로 편입

	// 포인터 회전 연산(링크 교체)
	X->Right = Y;		// 기존 루트 Y는 X의 오른쪽 자식으로 강등
	Y->Left = T2;		// 백업해둔 T2는 Y의 왼쪽 자식으로 편입

	// 회전 완료 후 트리 높이 갱신(자식 노드 높이 먼저 갱신 후 계산되어야 함)
	Y->Height = std::max(GetHeight(Y->Left), GetHeight(Y->Right)) + 1;
	x->Height = std::max(GetHeight(X->Left), GetHeight(X->Right)) + 1;

	// 회전 후 새로운 서브트리의 루트 노드가 된 X 포인터 반환
	return X;
}
```
   
클라 기준에서는 트리, 포인터 기반의 트리 구조 사용을 잘 안씁니다.
   
포인터 기반의 이진 트리는 노드가 힙 메모리의 무작위 주소에 산발적으로 할당되기에 
CPU가 캐시 라인(보통 64Bytes)단위로 메모리를 읽어올 때 흩어진 노드들을 포인터 체이싱으로 탐색 시 
L1/L2 캐시 적중률이 너무 낮아져서 Memory Latency 병목이 나옵니다. 
   
렌더링 루프나 물리 엔진쪽에서는 연속 배열 기반 자료구조(ECS 패턴 등)가 일반적입니다.
   
프레임 단위의 순차 탐색 성능이 요구되지 않으면서 빈번한 동적 삽입/삭제, 지속적인 데이터 정렬 상태 보장이 필수적인 
비동기적, 이벤트 주도형 시스템에서는 사용됩니다. 
   
- AI 어그로 리스트 및 타겟팅 시스템
   
몬스터가 다수의 플레이어 중 누구를 공격할 지 결정하는 AI로직에서 사용합니다.
   
단순히 가장 위협이 높은 1명을 찾는 Priority Queue 메커니즘을 넘어 
특정 범위 내의 어그로 순위 변동을 관리하거나 중간 순위의 플레이어를 임의로 타겟팅하는 기믹(Range Query)구현 시 
Red-Black 트리 기반 Map 구조를 사용합니다.
   
- 타임라인 보간 및 이벤트 스케줄러
   
클라엔진에서 발생하는 비동기 이벤트, 스킬의 지속시간 만료 이벤트, 네트워크 지연 보상(Dead Reckoning)을 위한 
과거 상태(Snapshot)의 타임스탬프 로깅 등에서 데이터를 시간 순서대로 정렬, 보관하고 만료된 노드를 
잘라내는(Pruning) 작업에 사용합니다.
   
- UI 시스템 동적 Z-Order 관리
   
렌더링 우선순위를 결정하는 Z축 깊이 값이 런타임에 동적으로 빈번하게 변하는 
복잡한 위젯 계층 구조에서 투명도 소팅을 위한 드로우 콜 제출 전 렌더링 큐 관리 시 백엔드 자료구조로 활용합니다.
   
- 경계 볼륨 계층(BVH) 구축 전 단계의 공간 분할 보조
   
충돌 처리를 위해 Octree나 완전한 3D BVH 구조 구성 전 특정 1차원 축(Axis)으로 투영된 
오브젝트들의 AABB(Axis-Aligned Bounding Box)시작 및 끝 좌표를 정렬된 상태로 유지하면서 겹침을 검사하는 
Sweep and Prune(SAP) Broad-phase 알고리즘의 동적 리스트 관리에 균형 트리를 사용할 수 있습니다.
   
클라 기준 구현 예시는 다음과 같습니다.
   
```C++
#include <cstdint>

enum class ERBColor : uint8_t { Black = 0, Red = 1 };

template <typename T>
struct alignas(16) FClientRBTNode
{
	T Data;
	int32_t LeftIndex;		// 메모리 풀 배열 내 왼쪽 자식 인덱스(포인터 대체, -1은 NIL 노드)
	int32_t RightIndex;		
	ERBColor Color;			// 노드 색상(1바이트, 뒤쪽 남은건 패딩으로 16바이트 정렬됨)

	// 기본 생성자, 초기화 시 자식이 없는 상태(-1)와 Black 색상 지정
	FClientRBTNode() : LeftIndex(-1), RightIndex(-1), Color(ERBColor::Black) {}
};

template <typename T, size_t MaxNodes = 4096>	// 최대 노드 수를 컴파일 타임에 결정
class FFixedPoolRedBlackTree
{
	
private:
	FClientRBTNode<T> NodePool[MaxNodes];
	int32_t RootIndex = -1;
	int32_t FreeHeadIndex = 0;

public:
	...

	int32_t insert(const T& Value)
	{
		// 배열 풀이 가득 찼을 시 Out Of Memory 방어
		if (FreeHeadIndex == -1) return -1;

		int32_t NewNodeIndex = FreeHeadIndex;

		...

		NodePool[NewNodeIndex].Data = Value;
		NodePool[NewNodeIndex].Color = ERBColor::Red; // Red-Black 트리 삽입 기본 원칙
		NodePool[NewNodeIndex].LeftIndex = -1;
		NodePool[NewNodeIndex].RightIndex = -1;

		...

		return NewNodeIndex;
	}
};
```