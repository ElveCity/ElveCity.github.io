---
title: "DataStructure, Hash Table"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Hash Table]

permalink: /DataStructure/Hash Table

toc: true
toc_sticky: true

date: 2026-04-19
last_modified_at: 2026-04-19
---

# DataStructure(Hash Table)
***
   
## Hash Table
   
키(Key)를 특정 해시 함수(Hash Function)로 해시 값(Hash Value)으로 변환하며 
이를 배열의 인덱스로 만들어 값을 저장하는 연관 배열(Associative Array) 형태의 자료구조입니다.
   
시간복잡도는 삽입, 삭제, 탐색 모두 O(1)입니다.
   
- 해시 함수(Hash Function)
   
임의 길이를 가진 키 데이터를 고정 길이의 해시 값(정수)로 매핑하는 알고리즘입니다.
   
좋은 해시 함수는 작은 변화에도 해시 값이 크게 변하게 됩니다(Avalanche Effect).
   
물론 속도도 빨라야 합니다.
   
- 버킷(Bucket) / 슬롯(Slot)
   
데이터가 실제 메모리상에 저장되는 배열의 각 공간입니다.
   
계산된 해시 값은 버킷 배열의 크기(Capacity)로 모듈러 연산되어 최종 메모리 주소(Index)로 변환됩니다.
   
> index = hash(key) % capacity
   
capacity가 2의 거듭제곱인 경우 비트 AND 연산으로 연산 주기 단축이 가능합니다
   
> hash(key) & (capacity - 1)
   
- 해시 충돌(Hash Collision), 해결법
   
무한한 키 공간을 유한한 버킷 공간으로 매핑 시 서로 다른 두 키가 동일한 해시 값(index)을 갖는 
충돌이 발생합니다. 탐색 시간 복잡도는 O(N)까지 떨어질 수 있습니다.
   
해결법은 두 가지가 있습니다.
   
1) 분리 연결법(Separate Chaining)
   
각 버킷을 연결 리스트의 헤드 포인터로 하고 충돌 발생 시 해당 버킷의 리스트에 노드를 동적 할당해서 
체인처럼 엮어 추가합니다. 구현이 직관적이고 버킷이 다 차도 확장이 잘 되긴 하는데 
노드가 메모리 파편화를 발생시켜서 CPU L1/L2 캐시의 공간 지역성이 망가집니다.
   
잦은 포인터 추적으로 캐시 미스 확률이 너무 올라갑니다.
   
2) 개방 주소법(Open Addressing)
   
연결 리스트 배제, 1차원 배열 자체의 빈 공간 탐색으로 데이터를 삽입하는 형태입니다.
   
- 선형 조사법(Linaer Probing)
   
충돌 시 인접 후 다음 인덱스(+1)를 순차 탐색합니다.
   
메모리가 연속적이라 캐시라인 적중률이 많이 올라가긴 하는데 데이터가 특정 구간에 
군집되는 1차 클러스터링(Primary Clustering)현상이 발생합니다.
   
- 이차 조사법(Quadratic Probing)
   
충돌 시 탐색 폭을 제곱수로 넓혀 클러스터링을 완화합니다.
   
- 이중 해싱(Double Hashing)
   
충돌 시 완전히 다른 제 2의 해시 함수를 적용, 탐색 보폭을 결정합니다.
   
클러스터링 억제에 좀 더 효과적입니다.
   
> 적재율(Load Factor), 리해싱(Rehashing)
   
적재율 x = n / m (n = 저장된 원소 수, m = 버킷의 총 크기)은 해시 테이블의 성능 주요 지표입니다.
   
적재율이 임계치(C++ std::unordered_map 기준 1.0) 초과 시 버킷 배열의 크기를 2배로 확장하고 
모든 기존 데이터를 재해싱해서 메모리를 재배치합니다.
   
해당 과정은 O(N)의 오버헤드를 발생시키기에 데이터 최대 규모를 예측해서 reserve()로 
초기 메모리를 할당하는 방식으로 구현합니다. 최적화라고 볼 수 있겠습니다.
   
해시 테이블의 사용 예시는 다음과 같습니다.
   
- 데이터베이스 인덱싱(Database Indexing)
   
관계형 데이터베이스에서 레코드 접근 시 사용됩니다.
   
B-Tree 자료구조와 함께 해시 인덱스를 제공하는데 범위 탐색이 아닌 정확한 값의 일치 쿼리에 대해서 
탐색 속도가 좋습니다.
   
- 캐시 시스템(Cache System)
   
Memcached나 Redis와 같은 인메모리(In Memory) 캐시 데이터베이스의 자료구조입니다.
   
메모리 용량이 한정적일 시 데이터에 대한 최상위 접근 속도를 위해서 키-값 스토어를 해시 기반으로 관리합니다.
   
- 컴파일러 심볼 테이블(Compiler Symbol Table)
   
소스 코드를 컴파일하는 과정에서 변수명, 함수명 등의 식별자(Indentifier)를 메모리 주소나 
데이터 타입 정보와 매핑 시 사용합니다.
   
수만 줄의 코드에서 특정 식별자의 선언 여부를 빠르게 판단해야 하는 때 해시 테이블 구조는 강제에 가깝습니다.
   
- 중복 검출 및 세트(Duplicate Detection / Set)
   
특정 데이터 스트림이나 대용량 파일에서 중복 값 존재 확인 시 사용할 수 있스빈다.
   
C++에서는 std::unordered_set이며 값을 키로 사용, 존재 여부 플래그를 상수 시간으로 검증 가능합니다.
   
클라 기준의 사용 예시는 다음과 같습니다.
   
- 리소스 및 에셋 매니저(Asset Manager)
   
클라이언트가 디스크에서 VRAM 혹은 RAM으로 텍스처, 메쉬, 사운드 파일 등을 로드 시 동일 에셋 
중복 로딩 방지를 위해 캐싱을 만드는데 여기서 파일 경로 문자열이나 에셋 고유 GUID를 Key로 삼고 
메모리에 할당된 포인터를 Value로 하는 해시 테이블을 씁니다.
   
문자열 해싱 오버헤드를 줄이려고 컴파일 타임에 문자열을 해시 정수로 변환하는 
StringId 최적화 기법을 씁니다.
   
- 엔티티 컴포넌트 시스템(ECS), 오브젝트 풀링(Object Pooling)
   
게임 내 몬스터, NPC, 투사체 뭐 이런것들은 각각 고유 정수형 EntityID를 가집니다.
   
네트워크를 통해 서버로부터 특정 EntityID의 상태 변경 패킷 도달 시 
클라이언트가 수만 개의 오브젝트 중 해당 ID를 가진 실제 메모리 상의 오브젝트나 컴포넌트 포인터를 
색인하기 위해서 맵핑용 해시 테이블 구조를 사용합니다.
   
- 공간 해싱 활용을 통한 충돌 처리(Spatial Hashing)
   
게임 월드를 일정 크기 그리드로 분할 후 각 개체의 좌표를 해시 키로 변환합니다.
   
많은 탄환과 몬스터가 교차 시 모든 객체끼리 충돌 연산을 하지 않고 동일 해시 값(그리드 셀)을 
가진 인접 객체들끼리만 충돌을 검사하는 Broad Phase 연산에 사용됩니다.
   
- 이벤트 디스패처/델리게이트 시스템(Event Dispatcher)
   
클라이언트 내부의 시스템 간 결합도를 낮추기 위해 옵저버 패턴 사용 시, 특정 EventID를 Key로 
이벤트 발생 시 호출해야되는 콜백 함수 포인터들의 배열을 Value로 묶어 관리하는 
중앙 통제 테이블 구성에 사용됩니다.
   
해시 테이블 구현 예시입니다. 분리 연결법 - 체이닝 형태입니다.
   
```C++
#include <iostream>
#include <vector>
#include <list>
#include <string>

class ChainingHashTable
{

private:
	int capacity;
	// 버킷 배열, 각 버킷은 Key(int)와 Value(string)을 쌍으로 가지는 연결 리스트
	 std::vector<std::list<std::pair<int, std::string>>> table;

	 int hashFunction(int key) const
	 {
		return key % capacity;
	 }

public:
	ChainingHashTable(int size) : capacity(size)
	{
		table.resize(capacity);
	}

	void insert(int key, const std::string& value)
	{
		int index = hashFunction(key);
		for (auto& pair : table[index])
		{
			if (pair.first == key)
			{
				pair.second = value;
				return;
			}
		}
		table[index].emplace_back(key, value);
	}

	std::string search(int key) const
	{
		int index = hashFunction(key);
		for (const auto& pair : table[index])
		{
			if (pair.first == key)
			{
				return pair.second;
			}
		}
		return "Not Found";
	}
};
```
   
클라 기반 구현 예시입니다.
   
```C++
#include <iostream>
#include <unordered_map>
#include <cstdint>

struct GridCoord
{
	int32_t x, y, z;

	// 해시 테이블 검색 동치 연산자 오버로딩
	bool operator==(const GridCoord& other) const
	{
		return x == other.x && y == other.y && z == other.z;
	}
};

struct GridHash
{
	std:size_t operator()(const GridCoord& coord) const noexcept
	{
		std::size_t h1 = std::hash<int32_t>{}(coord.x);
		std::size_t h2 = std::hash<int32_t>{}(coord.y);
		std::size_t h3 = std::hash<int32_t>{}(coord.z);

		// 비트 시프트를 통해 1차 클러스터링 현상 방지
		return h1 ^ (h2 << 1) ^ (h3 << 2);
	}
};

class SpatialPartitionManager
{
private:
	std::unordered_map<GridCoord, uint32_t, GridHash> spatialMap;

public:
	void RegisterEntity(const GridCoord& coord, uint32_t entityID)
	{
		spatialMap(coord) = entityID; // 공간 맵에 엔티티 ID 등록
	}

	uint32_t GetEntityInGrid(const GridCoord& coord) const
	{
		auto it = spatialMap.find(coord);
		return (it != spatialMap.end()) ? it->second : 0xFFFFFFFF; // 없으면 Invalid ID 반환
	}
}
```