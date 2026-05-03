---
title: "DataStructure, B-Tree, B+Tree"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, B-Tree, B+Tree]

permalink: /DataStructure/B-Tree_B+Tree

toc: true
toc_sticky: true

date: 2026-05-03
last_modified_at: 2026-05-03
---

# DataStructure(B-Tree, B+Tree)
***
   
## B-Tree, B+Tree
   
대용량 데이터 탐색, 삽입, 삭제를 위한 자가 균형 탐색 트리(Self-balance search tree)입니다.
   
### B-Tree
   
노드 하나에 여러 개의 키와 자식 포인터를 가질 수 있는 탐색 드리입니다.
   
이진 트리는 자식을 최대 2개만 가지지만 B-트리는 차수(Degree, m)을 정의해서 
노드당 최대 m개의 자식을 가질 수 있습니다.
   
구조적 특성은 다음의 네 가지 정도로 볼 수 있습니다.
   
1) 루트 노드를 제외한 모든 내부 노드는 최소 m/2개의 자식을 가진다.
   
2) 모든 리프 노드는 트리의 동일한 레벨(Depth)에 위치해서 완벽한 높이 균형을 이룬다.
   
3) 각 노드에는 정렬된 상태의 키가 존재하고 키의 수는 자식 포인터의 개수보다 항상 1개가 적다.
   
4) 탐색 연산의 시간 복잡도는 최악의 경우에도 O(logN)는 나온다.
  
B-트리는 모든 노드(내부 노드 및 리프 노드)에 실제 데이터(또는 데이터에 대한 포인터)를 
저장합니다. 탐색 중 대상 키를 내부 노드에서 발견 시 리프 노드까지 내려가지 않고 즉시 탐색을 종료할 수 있습니다.
   
그러나 범위 탐색 수행 시 트리의 노드들을 중위 순회(In-order Traversal)해야 하기에 
트리의 여러 브랜치들을 오르내리는 과정에서 디스크 I/O나 메모리 랜덤 액세스가 
빈번하게 나서 성능이 저하되는 한계가 있습니다.
   
Linux의 Ext4, Windows의 NTFS, Apple의 APFS등은 디스크 내 파일의 메타데이터 블록 및 
디렉터리 계층 구조를 매핑 시 B-트리 또는 그 변형인 B*트리 등을 활용해서 파일 탐색 시간을 줄입니다.
   
구현 예시는 다음과 같습니다. 포인터 배열 기반 B-트리 노드 구조와 이진 탐색 결합 형태입니다.
   
```C++
#include <stream>
#include <vector>
#include <algorithm>

template <typename KeyType, typename ValueType, int Degree>
struct BTreeNode
{
	int num_key = ;
	bool is_leaf = true;
	KeyType keys[Degree - 1];
	ValueType values[Degree - 1];
	BTreeNode* children[Degree] = {nullptr};

	ValueType* Search(const KeyType& target_key)
	{
		int i = 0;

		while (i < num_keys && target_key > keys[i])
		{
			i++;
		}

		if (i < num_keys && keys[i] == target_key)
		{
			return &values[i];
		}

		if (is_leaf) return nullptr;

		return children[i]->Search(target_key);
	}
};
```

클라 기준 사용 예시는 다음과 같습니다.
   
- 로컬 내장형 데이터베이스(Embedded DB)
   
모바일 및 데스크톱 클라이언트에서 오프라인 데이터를 캐싱하거나 세이브 데이터를 관리하기 위해 
SQLite를 광범위하게 사용합니다.
   
SQLite의 내부 코어 페이저(Pager) 및 B-트리 모듈은 C언어로 작성된 B-트리/B+트리의 혼합 구현체입니다.
   
로컬 데이터 무결성을 위해 이 B-트리 기반 아키텍처를 직간접적으로 다룹니다.
   
- CPU 캐시 친화적 메모리 최적화
   
CPU의 L1/L2/L3 캐시 미스를 줄이는 것이 성능 최적화의 가장 중요한 부분입니다.
   
노드가 메모리 파편화를 일으키는 이진 트리 대신 하나의 노드가 연속된 메모리 배열을 가지는
B-트리 형태(ex. Google Abseil 라이브러리의 absl::btree_map)를 클라이언트의 
초당 프레임(FPS)방어가 중요한 게임의 오브젝트 관리자나 에셋 레지스트리(Asset Registry)에 
도입해서 탐색 속도를 올리는 방법을 사용합니다.
   
- 가상 파일 시스템(VFS - Virtual File System)
   
용량이 큰 게임은 수십 GB의 리소스를 단일 압축 파일(Pak, MPQ..)로 묶어 배포하는데 
파일 내부에서 특정 텍스처나 모델링 파일의 오프셋을 찾기 위해서 클라이언트 엔진 내부에 
B-트리 기반의 인덱싱 구조를 구현합니다.
   
클라 기반 구현 예시는 다음과 같습니다. 캐시 친화적 B-트리 예시입니다.
   
```C++
#include <cstdint>
#include <array>

constexpr size_t CACHE_LINE_SIZE = 64;
constexpr int MAX_KEYS = 5;

alignas(CACHE_LINE_SIZE) struct ClientBTreeNode
{
	uint16_t numkeys;
	bool is_leaf;

	uint32_t keys[MAX_KEYS];
	uint32_t data_indices[MAX_KEYS];
	uint32_t child_indices[MAX_KEYS + 1];
};

class ClientAssetBtree
{

private:
	std::array<ClientBTreeNode, 10000> node_pool;
	uint32_t root_index = 0;
	uint32_t next_free_node = 1;
}

public:
	uint32_t FindAssetDataIndex(uint32_t node_idx, uint32_t target_key)
	{
		if (node_idx == static_cast<uint32_t>(-1)) return -1;

		const auto& node = node_pool[node_idx];
		int i = 0;
		while (i < node.num_keys && target_key > node.keys[i]) i++;

		if (i < node.num_keys && node.keys[i] == target_key)
		{
			return node.data_indices[i];
		}

		if (node.is_leaf) return -1;

		return FindAssetDataIndex(node.child_indices[i], target_key);
	}
};
```
   
### B+Tree
   
B+트리는 B-트리의 구조를 개선해서 데이터베이스의 인덱싱 및 
순차 접근 성능을 올린 변형 자료구조입니다.
   
구조적 특성은 두 가지 정도로 볼 수 있습니다.
   
1) B+트리는 트리를 인덱스 세트(내부 노드)와 시퀀스 세트(리프 노드)로 분리한다.
   
2) 내부 노드(라우팅 노드)에는 탐색을 위한 키값만 저장하고 실제 데이터 레코드나 데이터 포인터는 오직 리프 노드에만 저장한다.
   
B+트리에서는 모든 리프 노드는 형제 노드를 가리키는 포인터(Next Pointer)를 
가지고 하나의 링크드리스트를 형성하게 됩니다.
   
내부 노드에 데이터를 저장하지 않기에 하나의 디스크 블록(또는 메모리 페이지)에서 
더 많은 라우팅 키를 적재할 수 있습니다.
   
이는 트리의 차수(m)를 크게 늘리고 트리의 전체 높이를 낮춰서(보통 3~4단계로 유지) 
탐색 시 I/O 횟수를 줄입니다.
   
그리고 범위 탐색 시 리프 노드의 링크드리스트를 타고 순차접근만 수행하면 되기에 
범위 질의(Range Query)성능이 좋습니다.
   
범위 기반 데이터 집계 시 B-트리와 다른 B+트리의 강점이 드러나는데
   
```
SELECT * FROM Orders WHERE Date BETWEEN '2026-05-01' AND '2026-05-03'
```
   
위와 같은 쿼리 처리 시 B-트리는 트리를 여러 번 오르내리며 중위 순회를 해야하나 
B+트리는 시작점인 '2026-05-01'을 O(logN)으로 찾은 후에 리프 노드의 
next포인터를 타고 디스크의 연속된 블록을 O(k, 조건 충족 최종 반환 데이터 수)의 속도로 
스캔합니다. 읽기 성능이 좋습니다.
   
구현 예시는 다음과 같습니다. 리프 노드 간 순차 접근용 포인터(next_leaf) 형성 중점 구현입니다.
   
```C++
#include <iostream>

template <typename KeyType, typename ValueType, int Degree>
struct BPlusNode
{
	bool is_leaf = true;
	int num_keys = 0;
	KeyType keys[Degree - 1];

	BPlusNode* children[Degree] = {nullptr};
	ValueType values[Degree - 1];
	BPlusNode* next_leaf = nullptr;

	void RangeSearch(const KeyType& start_key const KeyType& end_key)
	{
		BPlusNode* current = this;

		while (!current->is_leaf)
		{
			int i = 0;
			while (i < current->num_keys && start_key => current->keys[i]) i++;
			current = current->children[i];
		}

		while (current != nullptr)
		{
			for (int i = 0; i < current->num_keys; i++)
			{
				if (current->keys[i] >= start_key && current->keys[i] <= end_key>)
				{
					std::cout << "Found Data: " << current->values[i] << '\n';
				}
				if (current->keys[i] > end_key) return;
			}
			current = current->next_leaf;
		}
	}
};
```
   
클라 사용 예시는 다음과 같습니다.
   
- 공간 분할(Spatial Partitioning) 및 범위 기반 에셋 스트리밍
   
대규모 심리스 오픈월드 게임 클라이언트에서는 플레이어의 현재 좌표를 기준으로 반경 N미터
내의 텍스처, 사운드, 모델링 에셋만을 메모리에 적재해야합니다.
   
2D/3D 공간 좌표를 1차원 정렬 키(ex. Geohash, Z-order curve)로 변환해서 B+트리에 
인덱싱하게 되는 경우 플레이어 이동에 따른 특정 범위의 에셋 ID들을 
리프 노드의 링크드리스트를 통해 빠르게 일괄 추출, 백그라운드 로딩 스레드에 전달할 수 있습니다.
   
- 시계열 기반 클라이언트 로그 및 리플레이 시스템
   
게임 클라이언트의 성능 프로파일러나 프레임 단위의 리플레이 시스템에서는 많은 
이벤트 로그를 메모리에 기록하는데 프레임 드랍이 발생한 특정 타임스탬프 구간의 
로그만 렌더링하기 위해서 이벤트 발생 시간을 키로 하는 메모리 기반 B+트리를 구축, 
범위 검색 병목을 줄입니다.
   
클라 기준 구현 예시는 다음과 같습니다. 메모리 풀 기반 B+트리입니다.
   
```C++
#include <cstdint>
#include <vector>

constexpr size_t CACHE_LINE = 64;
constexpr int M = 7;

alignas(CACHE_SIZE)	struct DODBPlusNode
{
	uint16_t num_keys;
	bool is_leaf;
	uint32_t keys[M - 1];

	union
	{
		uint32_t children_idx[M];
		struct
		{
			uint32_t data_idx[M - 1];
			uint32_t next_leaf_idx;
		} leaf;
	};
};

class ClientAssetBPlusTree
{
	
private:
	void GetAssetsInRange(uint32_t start_id, uint32_t end_id, std::vector<uint32_t>& out_assets)
	{
		if (node_pool.empty()) return;
		uint32_t curr = root_idx;

		while (!node_pool[curr].is_leaf)
		{
			int i = 0;
			while (i < node_pool[curr].num_keys && start_id >= node_pool[curr].keys[i]) i++;
			curr = node_pool[curr].children_idx[i];
		}

		while (curr != static_cast<uint32_t>(-1))
		{
			const auto& node = node_pool[curr];
			for (int i = 0; i < node.num_keys; i++)
			{
				if (node.keys[i] > end_id) return;
				if (node.keys[i] >= start_id)
				{
					out_assets.push_back(node.leaf.data_idx[i]);
				}
			}
			curr = node.leaf.next_leaf_idx;
		}
	}
};
```