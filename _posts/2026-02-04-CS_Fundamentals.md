---
title: "기본 용어정리"
excerpt: "CS에서 포함될 수 있는 기본적인 용어에 대하여 정리합니다."

categories:
  - CS
tags:
  - [CS]

permalink: /categories4/CS/

toc: true
toc_sticky: true

date: 2026-01-29
last_modified_at: 2026-02-04
---

# 용어정리 
***
<h6 style="margin-top:7px; margin-bottom:5px">엔디안 (Endianness)</h6>
메모리에 바이트를 저장하는 순서, 바이트의 순서가 큰 경우 가장 중요한 바이트부터 낮은 주소에 저장하는(MSB) 빅 엔디안,
작은 경우 리틀 엔디안(가장 덜 중요한 바이트(LSB)부터 낮은 주소 저장 이라고 명합니다.
<br><br>
<h6 style="margin-top:7px; margin-bottom:5px">MSB(Most Significant Bit)</h6>
부호 비트(Sign Bit)라고도 합니다. 비트 벡터의 가장 왼쪽 비트로 이 비트가 0이냐 1이냐에 따라 양수, 음수의 표현을 가능하도록 합니다.
<br><br>
<h6 style="margin-top:7px; margin-bottom:5px">가상 메모리 (Virtual Memory)</h6>
실제 물리 메모리 크기에 상관없이 프로세스별로 독립적 주소공간을 가지도록 합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">컨텍스트 스위치 (Context Switch)</h6>
CPU가 한 프로세스에서 타 프로세스로 실행을 넘기는 경우 현재 상태를 저장하고 새로운 상태를 불러오는 과정입니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">메모리 계층 구조 (Memory Hierarchy)</h6>
레지스터, 캐시(L1, L2, L3), 메인 메모리, 디스크 순으로 속도, 용량에 따라 계층화하여 성능의 최적화를 꾀합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">포인터 (Pointer)</h6>
데이터가 저장된 메모리 주소를 가리키는 변수, 주소값을 저장합니다.
   
주소에 저장된 데이터의 종류가 무엇인지에 대한 타입(Type)정보를 포함합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">버퍼 오버플로 (Buffer OverFlow)</h6>
할당된 메모리 공간(버퍼) 보다 더 큰 데이터가 입력되어 인접 메모리를 침범하는 형태입니다. 보안 공격의 핵심 경로가 됩니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">가비지 컬렉션 (Garbage Collection)</h6>
더 이상 접근이 불가능하며 사용될 수 없는 메모리를 시스템이 자동으로 회수하는 기능입니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">레이스 컨디션 (Race Condition)</h6>
둘 이상의 프로세스가 공유자원에 접근하여 접근 순서에 따라 결과가 달라지는 상태를 의미합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">데드락 (DeadLock)</h6>
교착 상태라고도 합니다. 두 프로세스가 서로의 리소스에 대한 대기로 지속적인 대기로 진행이 불가능한 상태를 의미합니다.
   
<h6 style="mmargin-top:7px; margin-bottom:5px">세마포어 / 뮤텍스 (Semaphore / Mutex)</h6>
한 번에 한 프로세스만 자원을 사용하도록 통제하는 상태를 의미합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">추상화 (Abstraction)</h6>
하드웨어의 물리 구현을 숨기고 프로그래머가 사용하기 쉽도록 논리적 모델을 제공하는 개념입니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">동시성 (ConCurrency), 병렬성(Parallelism)</h6>
동시성은 여러 작업이 겹치는 시간 동안 실행되는 논리 개념
   
병렬성은 여러 작업을 동시에 실제 실행하여 속도를 높이는 물리 개념입니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">캐싱 (Caching)</h6>
더 빠르고 작은 저장 장치를 바로 아래의 느리고 큰 저장 장치의 임시 보관소로 사용합니다.
   
이를 통하여 전체적인 데이터 접근 속도의 증진을 꾀합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">가상 머신 (Virtual Machine)</h6>
컴퓨터 시스템 전체(OS + Hardware)를 소프트웨어로 구현, 다른 추상화 층을 만드는 개념입니다.
이를 통해 한 개의 물리적 컴퓨터에서 여러 개의 서로 다른 운영체제를 동시에 독립 실행 가능하도록 합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">비트(Bit)</h6>
Binary Digit, 컴퓨터가 정보를 처리하는 가장 작은 단위에 해당합니다.
0은 0V, 1은 약 4.5~5.5V 사이의 전압을 가하는 경우 발생하는 상태와 플립플롭과 같은 하드웨어의 결합을 통하여
해당 상태를 물리적으로 저장하며 하드웨어의 상태별로 휘발성의 존재 유무, 지속적인 저장 가능 유무에 따라서
RAM과 DISK와 같은 저장장치로 구분하도록 합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">바이트(Byte)</h6>
대부분의 컴퓨터에서 주소가 지정 가능한 최소 메모리 단위로 8개의 비트를 묶음으로 할 때 1개의 바이트를
생성가능하도록 합니다. 비트가 4개로 결합된 경우는 니블(Nibble)이 있습니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">워드 크기(Word Size)</h6>
시스템의 포인터 변수 크기를 나타내는 지표에 해당합니다.
32비트 기기는 4바이트, 64비트 기기는 8바이트의 워드 크기를 가집니다. 이는 해당 시스템이 한 번에 처리할 수 있는
데이터의 양, 가상 주소 공간의 최대 크기를 결정짓습니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">마스킹(Masking)</h6>
비트 패턴을 왼쪽, 오른쪽으로 밀어내는 연산을 의미합니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">논리적 시프트(Logical Shift)</h6>
빈 공간을 0으로 채우는 방식입니다.
   
<h6 style="margin-top:7px; margin-bottom:5px">산술적 시프트(Arithmetic Shift)</h6>
부호 비트의 보존을 위해 빈 공간을 원래의 가장 왼쪽 비트(부호비트)로 채우는 형태입니다.
