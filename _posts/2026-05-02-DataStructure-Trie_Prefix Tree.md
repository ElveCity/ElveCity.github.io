---
title: "DataStructure, Trie / Prefix Tree"
excerpt: "해당 파트의 이론 및 구현, 그 외 관련 이론에 대해서도 다룹니다."

categories:
  - DataStructure
tags:
  - [DataStructure, Trie / Prefix Tree]

permalink: /DataStructure/Trie / Prefix Tree

toc: true
toc_sticky: true

date: 2026-05-02
last_modified_at: 2026-05-02
---

# DataStructure(Trie / Prefix Tree)
***
   
## Trie / Prefix Tree
   
문자열 집합의 표현, 탐색 특화 트리 기반 자료구조입니다.
   
문자열의 접두사(Prefix)를 트리의 간선(Edge) 또는 노드구조로 고유, 저장해서 접두사 트리(Prefix Tree)라고도 칭합니다.
   
구조는 세 가지로 나뉩니다.
   
1) 루트 노드(Root Node)
   
트라이의 루트는 어떤 문자도 포함하지 않는 빈 문자열(Empty String)을 논리적으로 표기합니다.
   
2) 간선(Edge)과 상태 전이
   
각 노드에서 자식 노드로 연결되는 간선(또는 자식 포인터 배열의 인덱스)은 
하나의 문자를 나타냅니다. 
   
노드를 순회하면서 간선을 따라가는 방식으로 문자열을 구성합니다.
   
결정적 유한 오토마타(DFA, Deterministic Finite Automaton)의 상태 전이와 수학적 동일 모델을 지닙니다.
   
3) 종단 노드(Terminal Node)
   
특정 문자열의 끝 도달 시 알림 플래그(ex. bool is__end_of_word)를 가집니다.
   
특정 노드 도달 시 플래그 true값을 얻는 경우 루트에서 해당 노드까지 
경로를 구성하는 문자 연결이 집합에 포함된 유효 문자열임을 알 수 있습니다.
   
- 시간 복잡도
   
탐색, 삽입, 삭제 모두 O(M, M은 대상 문자열 길이)를 가집니다.
   
트라이에 저장된 전체 문자열 개수 N에 영향을 받지 않기에 
이진 탐색 트리나 해시 테이블에 비해서 탐색 속도 측면에서 장점을 가집니다.
   
- 공간 복잡도
   
O(N x M x Alphabet_Size)를 가집니다.
   
영문 소문자만 처리한다 해도 각 노드마다 26개의 포인터 배열이 요구됩니다.
   
트라이 사용 시 메모리 레이아웃 및 캐시 지역성에 문제의 여지가 좀 있습니다.
   
64비트 아키텍처 기준 영문 소문자 트라이 노드는 Node* children[26](208byte) + bool is_end[1byte] 
로 구성할 수 있습니다.
   
컴파일러 메모리 정렬 및 패딩으로 7바이트 더미 추가로 노드 하나에 총 216바이트를 씁니다.
   
힙 영역에 트리 노드가 산발적으로 할당는데 이를 포인터 체이싱으로 순회합니다.
   
이렇게 되면 L1/L2 캐시 미스가 빈번해져서 이론적 시간 복잡도 대비 성능 저하의 여지가 있습니다.
   
트라이 사용 예시는 다음과 같습니다.
   
- 검색어 자동 완성(Autocomplete)
   
검색창에 "comp"입력 시 트라이에서 'c' -> 'o' -> 'm' -> 'p'노드로 이동 뒤 
해당 노드를 루트로 하는 서브트리를 DFS나 BFS로 순회, 하위의 모든 종단 노드(ex. "computer", "compile"..)를 
수집해서 추천합니다.
   
- 사전 검색 및 맞춤법 검사(Spell Checker)
   
단어 사전 전체를 트라이애 적재하고 사용자가 입력한 문자열이 
트라이 상 종단 노드에 정확히 도달하는지(맞춤법이 맞는 지)를 O(M)으로 검증할 수 있습니다.
   
- 네트워크 라우팅(Longest Prefix Match)
   
라우터가 IP주소 기반으로 패킷 포워딩 시, 트라이 변형 래딕스 트리(Radix Tree / Patricia Trie)를 써서 
서브넷 마스크와 가장 길게 일치하는(Longest Prefix Match) 라우팅 경로를 하드웨어(TCAM) 또는 소프트웨어 수준에ㅓㅅ 
연산합니다.
   
트라이 구현 예시는 다음과 같습니다. 정적 배열 사용 기반입니다.
   
```C++
#include <iostream>

Class Trie
{

private:
	struct Node
	{
		Node* children[26] = { nullptr };
		bool is_terminal = false;
	};
	Node* root;

public:
	Trie() { root = new Node(); }

	void insert(const std::string& word)
	{
		Node* curr = root;
		for (char c : word)
		{
			int index c - 'a';
			if (curr->children[index] == nullptr)
			{
				curr->children[index] = new Node();
			}
			curr = curr->children[index];
		}
		curr->is_terminal = true;
	}

	bool search(const std::string& word)
	{
		Node* curr = root;
		for (char c : word)
		{
			int index = c - 'a';
			if (curr->children[index] == nullptr) return false;
			curr = curr->children[index];
		}

		return curr->is_terminal;
	}
};
```
   
클라 기반 사용 예시는 다음과 같습니다.
   
- 실시간 채팅 비속어 필터링(Aho-Corasick 알고리즘)
   
실시간 채팅 데이터에 많은 금칙어 사전을 대조해야하는 케이스입니다.
   
일반적 순차 검색이나 정규표현식 사용 시 채팅 문자열 길이 M과 금칙어 수 N에 대해서 
O(M/times N)의 부하로 프레임 드랍이 발생합니다.
   
트라이 구조에 실패 포인터(Failure Link) 개념을 결합한 Aho-Corasick 알고리즘을 사용합니다.
   
해당 알고리즘 사용 시 한 번의 순회로 전체 금칙어 포함 여부를 O(M + text(발견 금칙어 수))의 선형 시간으로 
검출 및 마스킹 처리가 가능합니다.
   
- 개발 인게임 디버그 콘솔 명령어 파싱
   
언리얼 엔진에서 개발자 콘솔을 통한 디버그 명령어 타이핑 시 일치 명령어를 
드롭다운 리스트로 렌더링 하는 때 사용합니다.
   
내부적으로 트라이 기반 심볼 테이블 구축으로 타이핑 시 후보군을 필터링, 클라 UI로 제공하는 방식입니다.
   
- 에셋 리소스 경로 매핑
   
게임 실행 중 패키징된 파일 시스템 구조(Virtual File System)내에서 텍스처, 메쉬, 사운드 뱅크 등의 
에셋을 고유 문자 경로(ex. "Character/Player/Textures/BAseColor.png")로 로드합니다. 
   
이 경우 리소스 식별 문자열을 접두사 트리로 구성 시 특정 디렉터리(ex. "Characters/Player/*") 
하위에 종속된 모든 리소스 메모리를 일괄 해제(Unload)하거나 프리패치(Prefetch)하는 작업을 트리 
한 번으로 최적화 할 수 있습니다.
   
클라 기반 구현 예시는 다음과 같습니다.

```C++
#include <string>
#include <unordered_map>
#include <memory>

class ClientTrie
{

private:
	struct Node
	{
		std::unordered_map<char, std::unique_ptr<Node>> children;
		bool is_terminal = false;
	};
	std::unique_ptr<Node> root;

public:
	ClientTrie() : root(std::make_unique<Node>()) {}

	void insert(const std::string& word)
	{
		Node* curr = root.get();
		for (char c : word)
		{
			if (curr->children.find(c) == curr->children.end())
			{
				curr->children[c] = std::make_unique<Node>();
			}
			curr = curr->children[c].get();
		}
		curr->is_terminal = true;
	}

	bool search(const std::string& word) const
	{
		Node* curr = root.get();
		for (char c : word)
		{
			auto it = curr->children.find(c);
			if (it == curr->children.end()) return false;
			curr = it->second.get();
		}

		return curr->is_terminal;
	}
};
```