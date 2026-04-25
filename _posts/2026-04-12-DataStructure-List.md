---
title: "DataStructure, List"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, List]

permalink: /DataStructure/List

toc: true
toc_sticky: true

date: 2026-04-12
last_modified_at: 2026-04-12
---

# DataStructure(List)
***
   
## List
   
물리적으로 연속된 메모리 공간을 할당받아 데이터를 저장하는 자료구조입니다. 선형적입니다.
   
C++에서는 std::vector가 이에 해당하는데 vector는 동적 배열이긴 합니다.
   
- 시간복잡도
   
최적 O(1), 최악 O(N)의 시간복잡도를 가집니다.
   
인덱스 접근 Random Access의 경우 O(1)의 복잡도가 나오고 중간에 데이터 삽입, 삭제가 있는 경우에는 
삽입 삭제된 곳 기준으로 뒷 데이터들 전부 시프트 연산해야해서 O(N)이 나옵니다.
   
push_back()을 사용하는 경우 O(1)이 일반적인데 할당 용량 넘어가면 메모리 더 큰거 하나 재할당(Reallocation)하고 
기존 데이터를 복사/이동합니다. 이 때도 O(N)이 됩니다.
   
리스트의 경우 데이터가 연속적으로 있어서 CPU L1/L2 캐시 적중률이 좋습니다.
   
CPU가 메모리 페치 시 64바이트 단위 캐시 라인을 가져오기 때문에 순차 순회 시 
성능이 자료구조 중 상당히 좋은 편에 속합니다.
   
데이터 개수가 가변적, 삽입/삭제보다 조회 및 순차가 잦은 데이터 집합에 쓰는데
   
문자열 버퍼, 로그 데이터 수집, 인덱스를 통한 정렬 알고리즘의 대상 컨테이너로 쓰입니다.
   
클라 기준에서 사용되는 케이스는 다음과 같습니다.
   
- 렌더링 파이프라인
   
DX11에서 GPU로 데이터를 넘기는 경우(Vertex Buffer, Index Buffer 등..) 메모리가 연속되어야 하므로 
벡터 기반 리스트가 사용됩니다. D3D11_MAP_WRITE_DISCARD를 통해 GPU 메모리로 복사 전 CPU에서 데이터 취합 시 사용합니다.
   
- IMGUI
   
IMGUI의 드로우 커맨드(ImDrawCmd)나 버텍스 배열(ImDrawVert)은 동적 배열 리스트 기반으로 구성되어있습니다. 
   
- 엔티티 관리
   
게임 루프 내에서 공간 지역성을 활용한 캐시 친화적 설계를 위해 객체 풀링(Object Pooling) 컨테이너로 사용합니다.
   
프레임마다 업데이트 해야하는 몬스터, 파티클, 투사체 등이 그 컨테이너 안에 들어갑니다.
   
> 객체 풀링(Object Pooling)
   
생셩/소멸 대신 재사용한다는 방식입니다. 파티클 기준으로 보겠습니다.
   
수백, 수천 개의 파티클이 나타났다가 사라지는데 매번 new와 delete를 할 수 없습니다.
가능은 하지만 매번 new를 쓰면 매번 빈 메모리 공간을 잡아서 할당해줘야하고 함수 호출 비용도 들어가고 
함수 호출하는데 오버헤드도 자연스럽게 따라옵니다. 동기화 처리도 해야하고 문제가 많습니다.
   
할당하고 호출해다가 쓴다고 해도 그건 그거대로 문제가 더 있습니다. 할당/해제 잘 하는거 아닌가 싶다가도 
메모리 할당이 앞에부터 차곡차곡 되는게 아니라서 메모리 파편화(Fragmentation)가 발생합니다. 중간중간 쪼개진 공간이 
생기다보니까 밖에서 보면 메모리량이 충분해보이는데 큰 메모리 할당은 불가능한 현상입니다. 
   
메모리 파편화는 외부 단편화(External Fragmentation)와 내부 단편화(Internal Fragmentation) 두 가지가 있습니다.
   
- 내부 단편화(Internal Fragmentation)
   
메모리 할당 시 프로세스나 객체가 실제 필요로 하는 메모리 크기보다 더 큰 메모리 블록이 할당되는 경우입니다.
   
할당된 블록 내부에 공간 낭비가 생깁니다. 주로 운영체제 페이징(Paging)이나 
고정 크기 메모리 풀(Fixed size Memory Pool)사용 시 생깁니다. 둘 다 관리 쉽게 하려고 
동일 크기 페이지나 블록으로 메모리를 쪼개놔서 그렇습니다.
   
- 외부 단편화(External Fragmentation)
   
메모리의 빈 공간(Free Space) 총합은 새 프로세스, 객체 할당에 충분하게 표시되나 공간들이 
연속적이지 않고 여러 곳에 조각나 있어(Scattered) 큰 연속 메모리 블록을 할당하지 못하는 경우입니다.
   
운영체제 세그멘테이션(Segmentation)이나 동적 메모리 할당(new, malloc) 사용 시 발생합니다.
요청 메모리가 가변적이라 그렇습니다.
   
게임 플레이 시 게임 루프 내에서 가변적 크기 문자열(std::string)이나 동적 배열(std::vector)을 지속적으로 할당, 해제 시 
운영체제 힙 영역이 많이 쪼개집니다. 게임을 장시간 켜두었을 때 메모리 여유가 있다 나오는데 크래시가 나서 꺼진다거나 
메모리 재할당 비용이 너무 커져서 프레임이 불안정해지는 경우에 이 원인인 경우가 많습니다.
   
다시 돌아가서 객체 풀링을 위해서 게임 시작이나 로딩 시 필요한 파티클 객체를 미리 큰 배열인 컨테이너에 넣어둡니다.
   
사용했을 시 new 대신 해당 풀에서 비활성 상태인 객체를 찾아서 활성 상태로 만듭니다.
   
사용이 끝난 후 delete 대신 해당 객체를 비활성 상태로 만들고 풀에 반납합니다.
   
List 구현 예시는 다음과 같습니다.
   
```C++
template <typename T>
class SimpleArrayList{

private:
	T* data;
	int capacity;
	int size;

	void reallocate(){
		capacity = (capacity == 0) ? 2 : capacity * 2;
		T* newData = new T[capacity];
		for (int i = 0; i < size; ++I)
		{
			newData[i] = data[i];
		}
		delete[] data;
		data = newData;
	}

public:
	SimpleArrayList() : data(nullptr), capacity(0), size(0) {}
	~SimpleArrayList() { delete[] data; }

	void push_back(const T& value){
		if (size == capacity) reallocate();
		data[size++] = value;
	}

	T& operator[](int index) { return data[index]; }
	int getSize() const { return size; }
};
```
   
클라 기준 예시입니다.
   
```C++
#include <iostream>

class Entity { /* ... */ }

class ClientEntityManager{

private:
	// 클라 객체들은 캐시 지역성을 위해 포인터 배열이 아닌 값 배열 형태 지향하는 편
	// 데이터 지향 설계(DOD)를 위해 컴포넌트를 분리, 연속 메모리에 배치
	std::vector<Entity> entityList;
}

public:
	ClientEntityManager(size_t initialCapacity){
		// 클라 초기화 시 병목 방지를 위해 메모리 선할당(Reserve)
		entityList.reserve(initialCapacity);
	}

	void SpawnEntity(const Entity& newEntity){
		// 복사 생성 비용 감소를 위해 emplace_back을 보통 사용
		entityList.emplace_back(newEntity);
	}

	void UpdateAllEntities(float deltaTime){
		// 메모리 연속배치로 CPU 데이터 프리페치(Prefetch)가 잘 작동
		for (auto& entity : entityList){
			// entity.Update(deltaTime);
		}
	}
};
```