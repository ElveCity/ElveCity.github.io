---
title: "DataStructure, Graph"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Graph]

permalink: /DataStructure/Graph01

toc: true
toc_sticky: true

date: 2026-05-07
last_modified_at: 2026-05-07
---

# DataStructure(Grpah)
***
   
## Graph
   
데이터 객체들 사이의 다대다(N:N) 관계의 표현을 위한 비선형 자료구조입니다.
   
트리와 다르게 그래프는 순환, 양방향 관계를 허용함으로써 네트워크 구조나 
객체 간 유기적 연결 상태 모델링에 적합한 형태입니다.
   
그래프는 수리적으로 G = (V, E)로 정의합니다.
   
> V(Vertices/Nodes) : 그래프 구성 정점 유한 집합
   
> E(Edges/Links) : 정점 쌍을 연결하는 간선의 집합
   
그래프는 세 가지 정도로 분류할 수 있습니다.
   
- 방향성에 따른 분류
   
간선의 방향 유무에 따라 무방향 그래프(Undirected Graph)와 방향 그래프(Directed Graph)로 
나눌 수 있습니다. 방향 그래프에서 간선은 (u, v)로 표현되며 이는 정점 u에서 정점 v로의 단방향 
연결을 나타냅니다.
   
- 가중치 그래프(Weighted Graph)
   
간선에 비용, 거리, 시간 등의 가중치 함수가 매핑된 그래프입니다.
   
- 순환성(Cyclic Properties)
   
정점 v0에서 시작해서 다시 v0로 돌아오는 경로가 존재할 경우 순환 그래프(Cyclic Graph), 
존재하지 않으면 비순환 그래프(Acyclic Graph)라 합니다.
   
방향성을 가지면서 순환이 없는 그래프는 DAG(Directed Acyclic Graph)라 합니다.
   
그래프로 표현되는 구조는 크게 인접 행렬과 인접 리스트 두 가지가 있습니다.
   
- 인접 행렬(Adjacency Matrix)
   
V x V 크기의 2차원 배열 A를 사용하여 표현합니다.
   
정점 i와 j가 연결되어 있으면 A[i][j] = 1(또는 가중치로 표현), 연결되어 있지 않으면 0으로 나타냅니다.
   
시간복잡도의 경우 두 정점의 연결 여부 확인은 O(1)로 수행되나, 특정 정점의 모든 인접 정점을 
탐색하는 데 O(|V|)가 소요됩니다. 
   
공간 복잡도는 O(|V|^2)입니다.
   
데이터 밀도가 높은 밀집 그래프에 사용하기 좋습니다.
   
- 인접 리스트(Adjacency List)
   
각 정점이 인접한 정점들의 리스트에 대한 포인터나 배열을 유지하는 방식입니다.
   
시간복잡도의 경우 정점의 연결 확인은 정점의 차수(Degree)만큼 소요되나, 모든 인접 정점을 
순회하는데 좋습니다. 
   
공간 복잡도는 O(|V| + |E|)입니다.
   
대다수의 간선이 존재하지 않는 희소 그래프 구조에 사용하기 좋습니다.
   
그래프 탐색 알고리즘은 DFS, BFS 두 가지입니다.
   
- DFS(Depth-First Search)
   
스택 메모리나 재귀 호출을 사용해서 특정 경로의 끝까지 우선 탐색하는 구조입니다.
   
시간 복잡도는 인접 리스트 기준으로 O(|V| + |E|)입니다.
   
사이클 검출이나 위상 정렬 시 필수적으로 사용합니다.
   
- BFS(Breadth-First Search)
   
큐를 써서 시작 정점으로부터 인접 노드들을 계층적으로 탐색하는 구조입니다.
   
비가중치 그래프에서 최단 거리 산출 시 좋습니다.
   
사용 예시는 다음과 같습니다.
   
- 네트워크 라우팅(Network Routing)
   
패킷이 송신지에서 수신지까지 도달하기 위한 최적 경로 계산에 씁니다.
   
라우터는 정점으로, 네트워크 대역폭이나 지연 시간은 간선의 가중치로 모델링되며 
OSPF 프로토콜 중에서 다익스트라 알고리즘 형태로 사용됩니다.
   
- 소셜 네트워크 서비스
   
사용자 간 팔로우(방향 간선)나 친구 관계(무방향 간선)를 모델링해서 클러스터링 
알고리즘을 구동, 상호 친구 추천을 수행합니다.
   
- 컴파일러 및 빌드 시스템
   
소스 코드나 패키지 간 의존성을 방향성 비순환 그래프(DAG)로 구성합니다.
   
위상 정렬(Topological Sort)을 통해서 교착 상태(Deadlock)없이 
빌드 오더를 산출하도록 구현합니다.
   
구현 예시는 다음과 같습니다. 인접 리스트 형태의 그래프입니다.
   
```C++
#include <iostream>
#include <vector>

class GraphTheory
{

private:
	int numVertices;

	vector<vector<int>> adjList;

public:
	GraphTheory(int vertices) : numVertices(vertices)
	{
		adjList.resize(vertices);
	}

	void addEdge(int src, int dest)
	{
		adjList[src].push_back(dest);
		adjList[dest].push_back(src);
	}

	void printGraph() const
	{
		for (int i = 0; i < numVertices; i++)
		{
			cout << "Vertex " << i << ":";
			for (int neighbor : adjList[i])
			{
				cout <<  " -> " << neighbor;
			}
			cout << "\n";
		}
	}
};

```
   
클라 기준 사용 예시는 다음과 같습니다.
   
- 네비게이션 메시 및 런타임 경로 탐색
   
3D 월드에서 캐릭터가 이동 가능한 평면 기하학을 다수의 볼록 다각형(Convex Polygon) 형태 
노드로 분할합니다.
   
인접한 다각형 간의 변(Edge)을 간선으로 삼아서 공간 그래프를 형성합니다.
   
런타임에서 AI 틱이 발생할 때마다 이 그래프 위에서 A-Star 휴리스틱 알고리즘이나 
JPS(Jump Point Search)를 써서 실시간 최적 이동 경로 벡터를 구성합니다.
   
- 상태 머신(FSM) 및 애니메이션 그래프
   
캐릭터의 동작(Idle, Walk, Run, Jump)을 각 노드로, 동작 간 전이 조건(Transition Rules)을 
간선으로 정의하는 방향 그래프 시스템입니다.
   
런타임에 이 그래프를 순회하며 각 애니메이션 노드의 본(Bone) 트랜스폼 데이터를 추출하고 
간선의 가중치에 따라 행렬 보간(Matrix Blending) 연산을 수행해서 부드러운 모션 렌더링을 구현합니다.
   
- 렌더링 파이프라인의 프레임 그래프
   
DirectX12나 Vulkan과 같은 모던 그래픽스 API환경에서 복수의 렌더 패스 간 
텍스처 버퍼 참조 의존성을 DAG 형태로 구성합니다.
   
렌더러는 씬을 그리기 전 이 그래프를 분석해서 리소스의 할당 및 해제 시점, 
메모리 배리어의 삽입 시점을 자동화해서 GPU 메모리 대역폭 낭비를 막고 
비동기 연산 효율을 높입니다.
   
- 에셋 종속성 그래프(Asset Dependency Graph)
   
오픈 월드 클라이언트 구동 시 특정 레벨이 참조하는 메쉬, 텍스처, 셰이더 간 
포함 관계를 그래프로 구성합니다.
   
백그라운드 스레드는 이 그래프를 기반으로 I/O 큐에 데이터를 비동기적으로 
적재할 순서를 스케줄링하고 메모리 임계치 도달 시 
역참조 그래프를 순회해서 안전하게 해제할 수 있는 에셋 노드를 선별합니다.
   
클라 기준 구현 예시는 다음과 같습니다. 
   
```C++
#include <vector>
#include <cstdint>

using NodeHandle = int32_t;

struct Edge
{
	NodeHandle targetNode;
	float weight;
};

class GraphClientStandard
{

private:
	vector<Node> nodes;
	vector<Edge> edges;

public:
	void Reserve(size_t maxNodes, size_t maxEdges)
	{
		nodes.reserve(maxNodes);
		edges.reserve(maxEdges);
	}

	NodeHandle AddNode()
	{
		Node newNode = { static_cast<int32_t>(edges.size()), 0 };
		nodes.push_back(newNode);
		return static_cast<NodeHandle>(nodes.size() - 1);
	}

	void AddDirectedEdge(NodeHandle src, NodeHandle dest, float weight)
	{
		edges.push_back({dest, weight});
		nodes[src].edgeCount++;
	}
};
```