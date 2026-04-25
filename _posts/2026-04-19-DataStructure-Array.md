---
title: "DataStructure, Array"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Array]

permalink: /DataStructure

toc: true
toc_sticky: true

date: 2026-04-19
last_modified_at: 2026-04-19
---

# DataStructure(Array)
***
   
## Array
   
기초 자료구조입니다.
   
논리 데이터 순서와 물리 메모리 순서가 일치하는 연속 메모리 공간(Contiguous Memory Allocation)이며 
동일 데이터 타입(Homogeneous Data Type)의 요소들을 순차 저장합니다.
   
int는 int끼리 줄세우기 하고 float는 float끼리 줄지어놓고 들어간 0(GND, 0V), 1(VCC, 약 4.5~5.5V)의 2진수 비트(Bit, Binary Digit) 읽을때는 int식으로 읽게 시키고 float방식대로 읽게 시키고 그렇게 씁니다.
   
주소 계산식은 Address = Base Address + (Index x sizeof(Type))입니다. CPU 기준 단일 명령어 처리가 가능해서 인덱스 접근 시간복잡도는 O(1)입니다.
   
int, float와 같은 포인터 타입이 2진수 비트의 나열 묶음을 디코딩하는 방식입니다.
   
포인터 타입은 컴파일러에 역참조(*)시 두 가지 지시를 전달합니다.
   
1) 몇 바이트를 읽어올 것인가(Stride)
   
- int*(정수 포인터)로 읽을 경우
   
컴파일러는 이 주소의 데이터를 범용 레지스터로 로드하고 산술 논리 장치(ALU)를 통해
2의 보수(Two's Complement)체계로 해석합니다.

2) float*(실수 포인터)로 읽을 경우
   
컴파일러는 동일한 메모리 주소의 데이터를 부동소수점 레지스터(XMM/YMM)로 로드하고
부동 소수점 처리 장치(FPU)를 통해 IEEE 754 단정밀도(Single Precision) 규격으로 분해, 해석합니다.
   
예시로 16진수 0x40000000라는 32비트(4바이트) 데이터 해석 시
   
int* 해석은 1,073,741,824가 float* 해석은 2.0f가 도출됩니다.
   
배열의 전체 시간 복잡도는 다음과 같습니다.
   
- 접근(Access)
   
O(1)
   
- 탐색(Search)
   
순차 탐색 시 O(N), 데이터 정렬로 이진 탐색 시 O(logN)
   
- 삽입 및 삭제(Insertion/Deletion)
   
배열의 맨 끝에 데이터 추가, 삭제는 O(1)이나 처음이나 중간에 삽입/삭제 시 
대상 인덱스 이후 모든 데이터를 앞이나 뒤로 이동(Shift)해야해서 O(N)이 됩니다.
   
CPU가 RAM에서 데이터를 읽어올 때 요청 데이터 주변의 연속 메모리 블록, 캐시라인(Cache Line, 보통 64Byte)를 
L1/L2 캐시에 프리패치(Prefetch)합니다. 배열은 데이터가 연속적이라서 캐시에 로드 후 순차 탐색 시 
캐시 메모리에서 바로 데이터를 가져올 수 있어 RAM을 읽거나 하는 일을 줄일 수 있습니다. 캐시 히트율을 높히는 효과입니다.
   
- 정적 배열
   
컴파일 타임에 크기 결정, 스택 메모리에 할당되며 크기 변경은 안됩니다.
   
- 동적 배열
   
런타임 크기 결정, 힙 메모리에 할당되며 사전 할당 용량(Capacity)을 모두 사용 시 
보통 1.5~2배의 용량을 재할당(Reallocation)받고 기존 데이터 복사, 이전 메모리 해제의 동작이 발생합니다.
   
재할당, 복사, 이전 메모리 해제 일련의 동작 발생 시 비용을 분할 상환 분석(Amortized Analysis) 계산 시 
평균 비용은 O(1)입니다.
   
배열 구현 예시입니다.
   
```C++
#include <iostream>
#include <algorithm>

template <typename T>
class TRawDynamicArray
{

private:
	T* Data;
	size_t Capacity;
	size_t Size;

	void Reallocate()
	{
		Capacity = (Capacity == 0) ? 2 : Capacity * 2;
		T* NewData = new T[Capacity];

		for (size_t i = 0; i < Size; ++i)
		{
			NewData[i] = Data[i];
		}

		delete[] Data;
		Data = NewData;
	}

public:
	TRawDynamicArray() : Data(nullptr), Capacity(0), Size(0) {}
	~TRawDynamicArray() { delete[] Data; }

	void PushBack(const T& Element)
	{
		if (Size >= Capacity)
		{
			Reallocate();
		}
		Data[Size++] = Element;
	}
	
	T& operator[](size_t Index) {return Data[Index]; }
	size_t GetSize() const { return Size; }
};
```
   
클라 기준 구현 예시입니다.
   
```C++
#include <vector>
#include <string>

struct FGameEntity
{
	uint32_t EntityID;
	float PositionX;
	float PositionY;

	FGameEntity(uint32_t ID, float X, float Y)
		: EntityID(ID), PositionX(X), PositionY(Y) {}
};

class FEntityManager
{

private:
	std::vector<FGameEntity> EntityPool;

public:
	void InitializePool(size_t ExpectedCount)
	{
		// 로딩 단계에서 사전 필요 용량 예약, 재할당으로 인한 프레임 스파이크 방지
		EntityPool.reserve(ExpectedCount);
	}

	void SpawnEntity(uint32_t ID, float X, float Y)
	{
		// push_back의 경우 임시 객체 생성 -> 복사/이동
		// emplace_back은 배열이 할당된 메모리 공간에 직접 객체 생성하도록 함, 단계를 줄여서 오버헤드 줄임
		EntityPool.emplace_back(ID, X, Y);
	}

	void UpdateTransforms()
	{
		for (auto& Entity : EntityPool)
		{
			Entity.PositionX += 1.0f;
		}
	}
}
```