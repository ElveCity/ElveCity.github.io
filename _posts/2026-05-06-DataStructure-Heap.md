---
title: "DataStructure, Heap"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Heap]

permalink: /DataStructure/Heap

toc: true
toc_sticky: true

date: 2026-05-06
last_modified_at: 2026-05-06
---

# DataStructure(Heap)
***
   
## Heap
   
특정 순서 규칙을 보장하는 완전 이진 트리 기반 자료구조입니다.
   
최댓값이나 최솟값을 O(1)로 빠르게 찾기 위해서 나왔고 삽입과 삭제 연산은 
O(logN)의 시간 복잡도를 가집니다.
   
포인터 기반 레드 블랙 트리와 다르게 연속적인 1차원 배열 메모리 레이아웃에 
매핑이 가능합니다.
   
완전 이진 트리 구조이기에 배열 내 잦은 메모리 단편화 없이 
데이터 노드를 순차적으로 적재 가느합니다.
   
0_based 인덱싱 기준 임의의 노드 인덱스 i에 대해서 다음과 같은 산술 접근이 가능합니다.
   
- 부모 노드 인덱스
   
(i - 1) / 2
   
- 왼쪽 자식 노드 인덱스
   
2 * i + 1
   
- 오른쪽 자식 노드 인덱스
   
2 * i + 2
   
이러한 접근은 별도의 메모리 주소 역참조 없이 가능하기에 포인터 캐시 미스를 
방지하고 파이프라인 스톨을 줄일 수 있습니다.
   
루트 노드부터 시작해서 순차적으로 배열에 데이터를 채워나가기 때문에 
공간 복잡도는 데이터 개수 N에 대해서 O(N)을 유지하고 
추가적인 메타데이터(포인터 등)으로 인한 메모리 패딩 오버헤드가 줄어듭니다.
   
힙은 모든 부모 노드는 자식 노드보다 크거나 같아야 한다를 속성으로 가지는 최대 힙
   
모든 부모 노드는 자식 노드보다 작거나 같아야 한다를 속성으로 가지는 최소 힙 두 가지가 있습니다.
   
모든 힙은 연산 시 다음과 같은 특징을 지니게 됩니다.
   
- 삽입(Push/Up-Heap)
   
새로운 요소는 항상 배열의 마지막 슬롯(완전 이진 트리의 최하단 우측 노드)에 
삽입됩니다.
   
이후 삽입된 요소는 잣니의 부모 노드와 값을 비교하며 힙 속성을 만족할때까지 
위로 상승(Bubble-up 또는 Sift-up)합니다.
   
트리의 높이가 logN이므로 최대 비교 횟수는 logN으로 제한됩니다.
   
- 삭제(Pop/Down-Heap)
   
힙에서의 삭제는 오직 루트 노드(최댓값 혹은 최솟값)에서만 발생합니다.
   
루트 노드를 제거한 후, 트리의 가장 마지막 요소를 루트 노드로 이동시킵니다.
   
새로운 루트 노드는 자신의 자식 노드들과 비교하며 아래로 하강(Bubble-down 또는 Sift-down)해서 
다시 힙 속성을 복원합니다.
   
- 힙 생성(Heapify)
   
무작위의 배열을 힙 구조로 재배열할 시, 상향식(Bottom-up)방식을 사용하면
O(NlogN)이 아닌 O(N)의 시간 복잡도로 힙 구축이 가능합니다.
   
힙은 배열 기반이기에 공간적 지역성이 우수한게 맞지만 트리 높이가 깊어질수록 
자식 노드를 탐색하기 위한 인덱스 도약 폭(2 * i)가 크게 증가합니다.
   
대용량 데이터 셋에서 L1/L2 캐시 라인을 벗어나는 메모리 접근이 생길 수 있는데 
좀 더 최적화 하기 위해서는 B-트리 기반 구조나 d-ary Heap을 쓰기도 합니다.
   
힙 사용 예시는 다음과 같습니다.
   
- 힙 정렬(Heap Sort)
   
주어진 배열을 제자리(In-place)에서 O(NlogN)의 복잡도로 정렬하는 알고리즘입니다.
   
퀵 정렬과 달리 최악의 경우에도 O(NlogN)이 나옵니다.
   
추가적인 배열 메모리 할당이 필요하지 않다는 장점이 있습니다.
   
- 운영체제 스케줄러
   
프로세스나 스레드의 실행 순서 관리 시 각 태스크의 우선순위(Priority)를 
키 값으로 하는 최대 힙을 구성합니다.
   
가장 높은 우선순위를 가진 프로세스를 O(1)로 찾아 CPU를 할당하고 
O(logN)으로 새 태스크를 큐에 삽입합니다.
   
- 네트워크 패킷 처리 시스템
   
대역폭 제한이나 서비스 품질 관리를 위해 중요도가 높은 네트워크 패킷을 
먼저 라우팅하는 우선순위 큐 매커니즘의 기반 구조로 사용합니다.
   
- 다익스트라(Dijkstra), 프림(Prim) 알고리즘
   
그래프 이론에서 최단 경로나 최소 신장 트리 탐색 시 아직 방문하지 않은 
정점 중 가장 가중치가 낮은 정점을 선택하는 과정에 최소 힙이 들어갑니다.
   
선형 탐색 O(V)를 힙을 통해서 O(logV)로 단축시킵니다.
   
- 연속된 스트림 데이터 중앙값 관리
   
데이터가 지속적으로 유입되는 환경에서 중앙값의 추적을 위해 
최대 힙과 최소 힙 두 개를 연결하여 사용할 수 있습니다.
   
구현 예시는 다음과 같습니다. 벡터 래핑 최소 힙 구조체입니다.
   
```C++
#include <vector>
#include <iostream>

template <typename T>
class TSimpleMinHeap
{

private:
	std::vector<T> Data;

	void BubbleUp(int Index)
	{
		while (Index > 0)
		{
			int ParentIndex = (Index - 1) / 2;
			if (Data[ParentIndex] <= Data[Index]) break;
			std::swap(Data[ParentIndex], Data[Index]);
			Index = ParentIndex;
		}
	}

	void BubbleDown(int Index)
	{
		int Size = static_cast<int>(Data.size());
		while (Index * 2 + 1 < Size)
		{
			int Left = Index * 2 + 1;
			int Right = Index * 2 + 2;
			int MinIndex = Index;

			if (Left < Size && Data[Left] < Data[MinIndex]) MinIndex = Left;
			if (Right < Size && Data[Right] < Data[MinIndex]) MinIndex = Right;

			if (MinIndex == Index) break;
			std::swap(Data[Index], Data[MinIndex]);
			Index = MinIndex;
		}
	}

public:
	void Push(const T& Value)
	{
		Data.push_back(Value);
		BubbleUp(Data.size() - 1);
	}

	void Pop()
	{
		if (Data.empty()) return;
		Data[0] = Data.back();
		Data.pop_back();
		BubbleDown(0);
	}

	T Peek() const { return Data.front(); }
	bool IsEmpty() const {return Data.empty(); }
};
```
   
클라에서는 프레임 방어, 로직 최적화에 주로 쓰입니다. 많은 동적 엔티티 상호작용이 실시간으로 요구되는 경우 더 그렇습니다.
   
클라 사용 예시는 다음과 같습니다.
   
- A-star 길찾기 알고리즘 Open List 관리
   
플레이어의 소환수나 적 AI가 월드 맵 이동 시 휴리스틱 가중치가 포함된 비용 함수인 
F-Cost(F = G + H)를 계산합니다. 경로 탐색 과정에서 수십, 수백의 노드(그리드 
또는 메쉬 폴리곤)를 순회하며 평가하게 되는데 이 중 다음에 탐색할 노드(가장 낮은 
F-Cost를 가진 노드)를 담아두는 버퍼를 Open List라고 합니다.
   
이 Open List를 최소 힙으로 구현 시 적절한 경로를 매번 O(logN)에 추출해서 
탐색 성능을 높일 수 있습니다.
   
- 지연 발생 이벤트 및 타이머 관리
   
특정 스킬 발동 후 X초 뒤에 타격 판정 박스 활성화, 디버프 지속시간 종료 후 상태 복구 
이런 프레임 단위가 아닌 절대 시간 단위의 지연 처리 예약을 게임에서 본 적 있을겁니다.
   
매 프레임(Tick)마다 수백 개의 예약된 이벤트를 전부 순회하며 시간이 되었는지 
검사하는 것은 CPU 낭비이기에 이를 최소 힙 기반의 우선순위 큐로 설계 시 
힙의 루트(가장 먼저 만료될 이벤트)의 시간과 현재 게임 월드 시간만 단 한 번 O(1)로 
비교하면 됩니다. 조건이 만족될 때만 Pop연산을 수행하므로 
불필요한 캐시 라인 오염을 줄일 수 있습니다.
   
- LOD(Level of Detail) 및 렌더링 최적화 큐
   
오픈월드 클라이언트에서는 카메라부터의 거리나 화면에서 차지하는 면적에 따라 
모델링의 폴리곤 해상도를 동적으로 스왑하는 LOD 시스템을 사용합니다.
   
수백 개의 액터가 동시에 시야에 들어올 때 메모리 로드/언로드의 병목을 막기 위해 
모든 액터를 일시에 업데이트 하지 않고 우선순위(카메라에 가까운 순서나 크기가 큰 순서)를 
넣어 최대 힙 큐에 넣습니다. 이후 프레임당 할당된 시간 예산내에서 힙에서 요소를 하나씩 꺼내 
순차적으로 LOD 레벨을 조절하는 비동기 처리 작업에 사용합니다.
   
- 파티클 및 이펙트 정렬
   
알파 블렌딩(Alpha Blending)이 적용되는 반투명 파티클을 렌더링할 때는 깊이 버퍼(Z-Buffer)에 
의한 가려짐 판정이 제대로 작동하지 않기에 카메라에서 먼 객체부터 순서대로 그려야 합니다(Back-to-Front)
   
보통 기수 정렬(Radix Sort)등을 사용하나 프레임 내내 지속적으로 생성되고 소멸되는 
동적 이펙트의 경우 거리값을 기준으로 하는 최대 힙을 유지하며 렌더링 파이프라인으로 
스트리밍 하는 방식으로 메모리 대역폭을 절약하는 엔진 구조도 존재합니다.
   
클라 기준 구현 예시는 다음과 같습니다. 지연 실행 타이머 이벤트 관리를 위한 구조체입니다.
   
```C++
#include <queue>
#include <vector>
#include <functional>

struct FTimerEvent
{
	float ExecutionTime;
	int EventID;

	bool operator>(const FTimerEvent& Other) const
	{
		return ExecutionTime > Other.ExecutionTime;
	}
};

class FClientTimerManager
{

private:
	std::priority_queue<FTimerEvent, std::vector<FTimerEvent>, std::greater<FTimerEvent>> EventHeap;

public:
	void ScheduleEvent(float CurrentTime, float Delay, int ID)
	{
		EventHeap.push({ CurrentTime + Delay, ID });
	}

	void Tick(float CurrentAbsoluteTime)
	{
		while (!EventHeap.empty() && EventHeap.top().ExecutionTime <= CurrentAbsoluteTime)
		{
			FTimerEvent ReadyEvent = EventHeap.top();
			EventHeap.pop();
		}
	}
};
```