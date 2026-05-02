---
title: "DataStructure, Binary Tree"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Binary Tree]

permalink: /DataStructure/Binary Tree

toc: true
toc_sticky: true

date: 2026-05-02
last_modified_at: 2026-05-02
---

# DataStructure(Binary Tree)
***
   
## Binary Tree
   
모든 노드가 최대 두 개의 자식 노드(왼쪽 자식, 오른쪽 자식)를 갖는 계층적 자료구조입니다.
   
싸이클이 없는 연결 그래프(Connected Graph)로 노드의 개수가 N개일 때 간선(Edge)의 개수는 항상 N-1 개가 됩니다.
   
이진 트리는 트리의 높이가 h일 때 최대 노드의 수는 2\^h - 1개이며 레벨 l에 존재할 수 있는 최대 노드 수는 2^l개 입니다.
   
이진 트리는 세 가지 정도로 분류가 됩니다.
   
1) 포화 이진 트리(Perfect Binary Tree)
   
모든 단말 노드(Leaf Node)가 동일한 깊이를 가지며 모든 내부 노드가 2개의 자식을 갖는 상태
   
2) 완전 이진 트리(Complete Binary Tree)
   
마지막 레벨을 제외한 모든 레벨이 노드로 채워져 있고 마지막 레벨의 노드는 왼쪽부터 차례대로 채워진 트리, 메모리상에서 1차원 배열 표현에 최적화
   
3) 편향 트리(Skewed Binary Tree)
   
모든 노드가 한 쪽 방향(왼쪽 또는 오른쪽)으로만 자식을 갖는 트리, 링크드리스트와 동일한 선형 구조
   
### Binary Search Tree, BST
   
이진 탐색 트리는 이진 트리의 구조에 탐색을 위한 정렬 규칙(Invariant)를 추가한 자료구조입니다.
   
해당 트리의 경우 임의의 노드 x에 대해서 왼쪽 서브트리에 있는 모든 노드의 키 값은 x의 키 값보다 작고 
오른쪽 서브트리에 있는 모든 노드의 키 값은 x의 키 값보다 큰 형태입니다.
   
이진 탐색 트리는 트리 구조가 균형을 이룰 경우 탐색, 삽입, 삭제 연산의 시간복잡도는 O(logN)이며 
편향 이진 탐색 트리의 경우 깊이가 N일 때 O(N)의 시간 복잡도를 가집니다.
   
시간 복잡도가 너무 나빠지지 않게 하기 위해서 AVL 트리나 Red Black 트리를 쓰기도 합니다.
   
이진 트리의 사용 예시는 다음과 같습니다.
   
1) 수식 트리(Expression Tree)
   
컴파일러가 소스 코드를 파싱할 때 연산자와 피연산자의 우선순위 처리를 위해서 이진 트리 구조인 수식 트리를 생성합니다.
   
전위/중위/후외 순회를 통해 수학 수식을 여러 표기법으로 치환, 어셈블리 명령어 변환의 기초로 이용합니다.
   
2) 데이터 압축(Huffman Coding)
   
허프만 코딩에 빈도수 기반 이진 트리가 사용됩니다.
   
허프만 코딩은 빈도수가 높은 문자일수록 짧은 코드를 부여하는 무손실 데이터 압축 알고리즘입니다.
   
3) 언어 내장 컨테이너 자료구조
   
std::map, std::set은 내부구조는 이진 탐색 트리의 보완 형태인 Red Black Tree로 구현되어 있습니다.
   
삽입, 삭제, 탐색 시 O(logN)의 시간 복잡도를 가집니다.
   
4) 데이터베이스 인덱싱
   
대규모 데이터 저장 RDBMS의 인덱스는 이진 탐색 트리 개념을 디스크 I/O 블록 사이즈에 맞춰 N-ary(다항) 트리 형태로 확장한
   
B-Tree 및 B+Tree 구조 기반으로 동작합니다.
   
클라 기준 사용 예시는 다음과 같습니다.
   
1) BSP(Binary Space Partitioning)트리 구조 기반 렌더링, 충돌 최적화
   
3D 공간을 임의 평면(Plane)기준으로 앞과 뒤 두 공간으로 재귀 분할하는 BSP 트리를 사용합니다.
   
특정 카메라 뷰포트가 주어진 때 BSP 트리를 중위 순회의 변형으로 탐색 시 폴리곤을 카메라와의 거리에 따라 정렬해서 그릴 수 있습니다.
   
이를 통해 투명도(Alpha Blending) 렌더링 문제와 Z-Buffer 연산 부하를 감소시킬 수 있습니다.
   
충돌 감지(Collision Detection)시 씬 내의 모든 오브젝트를 순회하지 않고 플레이어 위치 공간과 교차하는 BSP 노드에서만 
충돌 검사를 수행하도록 해서 연산량을 O(N)에서 O(logN)정도로 줄일 수 있습니다.
   
2) LCRS(Left-Child Right-Sibling) 계층 구조 변환
   
게임 내 Transform 객체(Scene Graph)들은 하나의 부모 오브젝트 아래 수십 개의 자식 오브젝트가 
붙을 수 있는 N-ary(다항) 트리 형태입니다.
   
동적으로 크기가 변하는 자식 포인터 배열을 유지하는 식으로 하면 캐시 관리가 안돼서 클라이언트 엔진 코어에서는 
N-ary 트리를 첫 번째 자식(Left-Child)과 바로 옆 형제(Right-Sibling)만을 가리키는 이진 트리 형태로 매핑, 메모리 구조를 고정 레이아웃으로 최적화 진행합니다.
   
3) AI Behavior Tree
   
NPC 행동 패턴 정의 Behavior Tree의 Selector 노드와 Sequence 노드의 하부 평가 로직은 
이진 결정 트리(Binary Decision Tree)의 확장 형태입니다.
   
Success/Failure의 이진 분기를 통해 상태 머신(FSM)이 갖는 전이 복잡도를 계층적 단순화합니다.
   
4) 메모리 관리 체계(Buddy Memory Allocation)
   
클라용 커스텀 메모리 할당자를 구현 시 메모리 블록을 절반으로 계속 분할해서 관리하는 버디 할당기(Buddy System)는 
내부적으로 완전 이진 트리 구조로 메모리의 병합(Coalescing)과 분할(Splitting)상태를 비트맵으로 추적합니다.
   
할당 및 해제 속도를 올리고 외부 단편화 제어에 이용합니다.
   
구현 예시는 다음과 같습니다.
   
```C++
#include <iostream>

struct BasicNode
{
	int key;
	BasicNode* left;
	BasicNode* right;
	BasicNode(int k) : key(k), left(nullptr), right(nullptr) {}
};

class TheoreticalBST
{
	
private:
	BasicNode* root = nullptr;

	BasicNode* insertRecursive(BasicNode* node, int key)
	{
		if (node == nullptr) return new BasicNode(key);

		if (key < node->key)
		{
			node->left = insertRecursive(node->left, key);
		}
		else if (key > node->key)
		{
			node->right = insertRecursive(node->right, key);
		}
		return node;
	}

public:
	void insert(int key)
	{
		root = insertRecursive(root, key);
	}
	...
}
```
   
클라 구현 예시는 다음과 같습니다.
   
```C++
#include <vector>
#include <cstdint>

struct alignas(16) CacheFriendlyNode
{
	int32_t key;
	int32_t leftIndex;
	int32_t rightIndex;
	
	CacheFriendlyNode(int k) : key(k), leftIndex(-1), rightIndex(-1) {}
};

class ClientOptimizedBST
{

private:
	std::vector<CacheFridendlyNode> nodePool;
	int32_t rootIndex = -1;

public:
	ClientOptimizedBST(size_t reserveSize = 1024)
	{
		nodePool.reserve(reserveSize);
	}

	void insert(int32_t key)
	{
		int32_t newNodeIdx = static_cast<int32_t>(nodePool.size());
		nodePool.emplace_back(key);

		if (rootIndex == -1)
		{
			rootIndex = newNodeIdx;
			return;
		}

		int32_t current = rootIndex;
		while (true)
		{
			if (key < nodePool[current].key)
			{
				if (nodePool[current].leftIndex == 1)
				{
					nodePool[current].leftIndex = newNodeIdx;
					break;
				}
				current = nodePool[current].leftIndex;
			}
			else
			{
				if (nodePool[current].rightIndex == -1)
				{
					nodePool[current].rightIndex = newNodeIdx;
					break;
				}
				current = nodePool[current].rightIndex;
			}
		}
	}
};
```