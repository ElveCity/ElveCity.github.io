---
title: "DataStructure, Queue"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Queue]

permalink: /DataStructure/Queue

toc: true
toc_sticky: true

date: 2026-04-19
last_modified_at: 2026-04-19
---

# DataStructure(Queue)
***
   
## Queue
   
데이터 삽입 순서대로 나오게 되는 FIFO(First In First Out, 선입 선출) 선형 자료구조입니다.
   
한쪽 끝(Rear/Back)에서는 데이터 삽입(Enqueue/Push)만 수행하고 
반대쪽 끝(Front)에서는 데이터 삭제(Dequeue/Pop)만 수행합니다.
   
- 시간복잡도
   
삽입/삭제 : O(1), 해당 연산은 특정 메모리 주소(Front, Rear 포인터) 참조 형태입니다.
   
탐색 : O(N)
   
- 공간 복잡도
   
O(N)
   
큐는 보통 배열 기반 선형 큐를 구현하게 됩니다.
   
선형 큐는 데이터 삽입 삭제에 따라서 Front와 Rear포인터가 계속 배열 뒤쪽으로 밀리게 되는데 
배열 앞 공간이 비더라도 Rear 포인터가 배열 끝에 다다르면 더 이상 데이터를 넣을 수 없게 됩니다.
   
이런 문제를 해결하기 위해 원형 큐(Circular Queue)를 사용합니다.
   
메모리의 처음과 끝이 연결된 큐입니다. 배열 끝에 도달한 포인터에 % 연산을 적용, 다시 배열의 
인덱스 0으로 순환시킵니다.
   
Full과 Empty 구분을 위해서 배열의 한 칸을 비워두고 Front = Rear시 Empty, 
(Rear + 1) % Capacity == Front 시 Full로 처리합니다.

사용 예시는 다음과 같습니다.
   
- 운영체제 프로세스 스케줄링
   
실행 대기중 프로세스들을 레디 큐(Ready Queue)에 넣어둡니다. Round Robin 스케줄러의 경우 
할당 타임 슬라이스를 소진한 프로세스를 레디큐의 Rear에 다시 Enqueue하고 다음 실행 프로세스를 
Front에서 Dequeue해서 context switch를 수행합니다.
   
- 하드웨어 I/O 버퍼
   
키보드 타이핑 시 일시적으로 OS 레벨 키보드 버퍼(큐)에 적재되며 애플리케이션이 
순차적으로 처리하는 방식입니다.
   
- 그래프 탐색 알고리즘(BFS)
   
탐색 노드 순서를 큐에 저장, 관리합니다.
   
방문 노드의 인접 노드들을 큐에 순서대로 삽입하고 꺼내서 FIFO 방식으로 탐색합니다.
   
클라 기준 사용 예시는 다음과 같습니다.
   
- 입력 버퍼링(Input Buffering / Command Queue)
   
사용자의 키 입력을 즉각적으로 프레임에 반영하지 않고 입력 큐(Input Queue)에 쌓습니다. 컨트롤이 크게 필요한 액션 게임에서 주로 사용합니다.
   
캐릭터의 공격 애니메이션 재생 중 종료 프레임 직전 다음 스킬 키 입력이 들어올 때 해당 입력 이벤트는 
큐에 Push됩니다. 애니메이션 상태가 끝나고 큐에서 다음 입력을 Pop해서 프레임 지연 없이 다음 액션이 나가는 현상을 볼 수 있습니다.
   
이게 선입력 처리입니다. 커맨드 패턴(Command Pattern)과 결합된 큐의 활용입니다.
   
- 이벤트 구동 아키텍쳐(Event Driven Architecture / Message Queue)
   
플레이어가 몬스터에게 데미지를 입었을 때 플레이어 클래스가 UI 체력바를 직접 갱신, 타격음 오디오 클래스를 
직접 호출하는 경우 클래스 간 결합도가 너무 높아지므로 DamageTakenEvent를 글로벌 이벤트 큐에 Push하는 방식으로 구현합니다. 
UI 시스템, 사운드 시스템, 업적 시스템이 각자의 Update() 루프에서 해당 큐를 Polling(순회 확인)하여 이벤트를 
Pop하고 각자에 필요한 로직을 독립 처리합니다.
   
- 멀티스레드 기반 비동기 리소스 로딩(Async Resource Loading)
   
오픈월드 게임, 심리스(Seamless)맵 이동 구현 시 대용량 텍스처나 메쉬 데이터를 메인스레드에서 불러오게 되면 
프레임드랍이 생깁니다. 방지를 위해서 클라이언트에 로딩해야할 에셋 목록을 백그라운드 로드 큐(Background Load Queue)에 
Push하는 형식으로 구현됩니다. 백그라운드 워커 스레드 풀(Worker Thread Pool)이 
해당 큐에서 작업을 꺼내 디스크 I/O를 수행하고 메모리에 데이터를 올린 뒤에 완료된 에셋 포인터를 
다시 메인 스레드 대기 큐(Main Thread Ready Queue)에 Push합니다. 
메인스레드는 매 프레임마다 레디 큐를 확인해서 VRAM에 텍스터를 바인딩합니다.
   
- 네트워크 패킷 링 버퍼(Network Packet Ring Buffer)
   
멀티플레이어 환경에서 서버로 수신되는 TCP/UDP 패킷들은 OS의 소켓 수신 버퍼를 거쳐 
네트워크 전담 스레드로 들어옵니다. 네트워크 스레드는 받은 바이트 배열을 파싱, 패킷 큐에 순차 적재합니다.
   
다량의 패킷이 쏟아지는 상황에 락(Mutex Lock) 경합으로 인한 병목현상 방지를 위해 std::atomic 만을 사용, 
락 없이 돌아가는 Lock Free Ring Buffer(원형 큐)구조를 최적화하여 사용합니다.
   
원형 큐 기준 구현 예시입니다.
   
```C++
#include <iostream>
#include <stdexcept>

template <typename T>
class CircularQueue
{

private:
	T* array;
	int capacity;
	int front;
	int rear;

public:
	CircularQueue(int size) : capacity(size + 1), front(0), rear(0)
	{
		array = new T[capacity];
	}
	~CircularQueue() { delete[] array; }

	bool isEmpty() const { return front == rear; }
	bool isFull() const { return (rear + 1) % capacity == front; }

	void push(const T& item)
	{
		if (isFull()) return;
		rear = (rear + 1) % capacity;
		array[rear] = item;
	}

	T pop()
	{
		if (isEmpty()) throw std::out_of_range("Queue is Empty");
		front = (front + 1) % capacity;
		return array[front];
	}
};
```
   
클라 기준 구현 예시입니다.
   
```C++
#include <queue>
#include <mutex>
#include <optional>

struct GameEvent
{
	int eventId;
	float payload_x, payload_y;
};

class ThreadSafeEventQueue
{
	
private:
	std::queue<GameEvent> queue; // 내부구조 deque
	std::mutex mtx;
}

public:
	void PushEvent(const GameEvent& event)
	{
		std::lock_guard<std::mutex> lock(mtx);
		queue.push(event);
	}

	std::optional<GameEvent> PopEvent()
	{
		std::lock_guard<std::mutex> lock(mtx);
		if (queue.empty())
		{
			return std::nullptr;
		}
		GameEvent ev = queue.front();
		queue.pop();
		return ev;
	}
};
```