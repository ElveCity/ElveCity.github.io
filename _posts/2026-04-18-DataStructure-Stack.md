---
title: "DataStructure, Stack"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Stack]

permalink: /DataStructure

toc: true
toc_sticky: true

date: 2026-04-18
last_modified_at: 2026-04-18
---

# DataStructure(Stack)
***
   
## Stack
   
한 쪽 끝에서만 데이터의 삽입과 삭제가 일어나는 선형 자료구조입니다.
   
가장 나중에 삽입된 데이터가 가장 먼저 삭제되는 LIFO(Last in First Out, 후입선출)의 형태를 가집니다.
   
스택은 5가지의 연산을 가집니다.
   
1) Push
   
스택의 최상단(Top)에 새 데이터를 삽입합니다. 시간 복잡도는 O(1)입니다.
   
2) Pop
   
스택의 최상단에 위치한 데이터를 제거, 반환합니다. 시간 복잡도는 O(1)입니다.
   
3) Top(Peek)
   
최상단 데이터를 제거하지 않고 그 값만을 참조합니다.
   
4) IsEmpty
   
현재 스택이 비어 있는지 확인합니다.
   
5) IsFull
   
정적 배열 구현 시 스택이 가득찼는지 확인할 수 있습니다.
   
스택은 논리적으로 수직으로 쌓인 구조를 가집니다.
   
메모리상으로는 연속된 메모리 공간(Array based)나 불연속적 노드 연결(Linked list based)방식으로 구현됩니다.
   
CPU에서는 스택 포인터(Stack Pointer, SP) 레지스터를 통해 프로세스의 콜스택(Call Stack)을 관리하는 주요 방식입니다.
   
스택은 역순 데이터 복구, 중첩 구조 처리 시 사용 가능합니다.
   
예시는 4가지 정도 볼 수 있습니다.

1) 함수 호출 관리(Call Stack)
   
서브루틴의 복귀 주소와 지역 변수를 저장, 함수 실행 종료 후 이전 지점으로 돌아갑니다.
   
2) 수식 계산, 파싱
   
중위 표기법(Infix)을 후위 표기법(Postfix)로 변환하거나 연산자 우선순위 판별 시 사용합니다.
   
3) 깊이 우선 탐색(DFS)
   
그래프나 트리 구조에서 한 경로를 끝까지 탐색 후 돌아오는 과정에 사용합니다.
   
4) 역순 문자열 생성
   
데이터의 순서 반전에 쓰기 좋습니다.
   
고정 배열 방식의 스택 기본 구현은 다음과 같습니다.

```C++
#include <iostream>

const int MAX_SIZE = 10;

class ArrayStack
{

private:
	int data[MAX_SIZE];
	int topIndex;

public:
	ArrayStack() : topIndex(-1) {}

	bool push(int value)
	{
		if (topIndex >= MAX_SIZE - 1) return false;
		data[++topIndex] = value;
		return true;
	}

	bool pop()
	{
		if (isEmpty()) return false;
		--topIndex;
		return true;
	}

	int top() const
	{
		if (isEmpty()) return -1;
		return data[topIndex];
	}

	bool isEmpty() const
	{
		return topIndex == -1;
	}
}
```
   
클라에서는 상태(State) 관리, 흐름 제어에 주로 사용합니다.
   
예시는 4가지 정도 볼 수 있겠습니다.
   
1) UI 네비게이션 스택(Screen Stack)
   
UI 시스템은 보통 스택을 사용하여 구현됩니다.
   
> 메인 로비 -> 설정 -> 그래픽 설정
   
위와 같은 UI 창 진입 시 뒤로 가기 버튼을 누를 경우 가장 최근에 열린 창부터 
닫히는 로직이 스택으로 구현되는 형태입니다.
   
2) Undo/Redo 시스템(Command Pattern)
   
맵 에디터나 캐릭터 커스터마이징 툴에서 사용자의 행동(Command)를 스택에 Push합니다.
   
취소(Undo) 요청 시 스택의 최상단 명령을 Pop하여 역연산을 수행합니다.
   
3) FSM(Finite State Machine) 계층적 구조
   
AI 상태 제어 시 순찰(Patrol) 상태 중 전투(Combat) 상태 발생 시, 현재 상태를 
스택에 넣고 새로운 상태를 Push합니다. 전투 종료 후 다시 Pop하여 이전의 순찰 상태로 
복구 가능합니다.
   
4) 자원 관리, 메모리 최적화
   
게임 엔진에서 트랜스폼(Transform) 연산 시 계층적 행렬(Matrix Stack)을 사용하여 
부모 오브젝트의 좌표계를 자식에게 전달, 복구 과정을 반복하게 됩니다.
   
클라 기반 구현 예시입니다.
   
```C++
#include <vector>
#include <optional>

template <typename T>
class ClientStack
{

	public:
		void Push(const T& item)
		{
			containter.emplace_back(item);_
		}

		void Push(T&& item)
		{
			container.emplace_back(std::move(item));
		}

		void pop()
		{
			if (!container.empty()) return std::nullopt;

			return container.back();
		}

		bool IsEmpty() const
		{
			return container.empty();
		}
		
		size_t Size() const
		{
			return container.size();
		}

		void Clear()
		{
			container.clear();
		}
}
```