---
layout: single
title: "[GPU_Programming] GPU 프로그래밍 소개"
categories: GPU
tag: [CUDA, GPU_Programming, Parallelization, Linux, Make]
toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"
author_profile: false
# sidebar:
#     nav: "docs"
---

![junsnote]({{site.url}}/images/nvidia.jpg)

<div class="notice--info">
    <h1>학습목표</h1>
    <ol>
        <li>GPU 컴퓨팅/병렬화의 연관성을 이해</li>
        <li>C++CUDA (병렬 컴퓨팅 플랫폼)을 이해</li>
        <li>Linux 기본 명령어에 대해서 학습</li>
        <li>Make 명령어를 통해서 코드를 컴파일하는 방법에 대해서 학습</li>
    </ol>
</div>

# GPU를 사용해야 하는 이유

## CPU (Central Processing Unit)
- 중앙 처리 장치
- 애플리케이션은 기본 계산에 CPU를 사용한다.
	- 범용 가능 (대부분 순차 작업)
	- 확립된 기술
	- 일반적으로 8개 이하의 강력한 코어를 장착한다.
	- 일부 동시 프로세스 유형에 적합하지만 대규모 병렬 계산에는 적합하지 않다.


## GPU (Graphics Processing Unit)
- CPU가 할 일이 아닌, 단순 계산 처리를 대규모로 하기 위한 장
- 그래픽 처리 장치
- 병렬 가능한 문제를 위해 설계된 비교적 새로운 기술이다.
	- 처음에는 그래픽 전용으로 제작되었다.
	- 일반적인 계산 능력이 향상되었다.
	- 매우 빠르고 강력한 계산 능력
	- 많은 전력을 사용한다.

### GPU 사용 예
- 광선추적 (Raytracing)
	- 이미지의 모든 픽셀 (i, j)에 대해
		- 카메라에서 보면, 3D 공간에서 광선 점 및 방향을 계산
		- 광선이 물체와 교차하는 경우. 가장 가까운 물체 지점에서 조명(lightning) 계산
		- (i, j)의 색상 저장
	- 이미지 파일로 조립
	- 각 픽셀은 병렬처리로 동시에 계산될 수 있음

## GPU 강점
- GPU에서 병렬화가 강조된다는 것은 많은 코어를 보유하고 있다는 것을 의미한다.
- 이를 통해 context switching 없이 여러 스레드를 동시에 실행할 수 있다.

### 쉐이더(Shader) 
- 고정 함수 파이프라인의 기능 중 하나 
- 그래픽 루틴을 사용하여 자신의 기능을 구현할 수 있다. 
- GLSL(OpenGL Shading Language) - 범용 프로그래밍을 도입할 수 있다. 
- 범용 코드 대신 픽셀 및 정점 연산을 사용하지만, 조잡하다. - Vulkan/OpenCL은 최신 멀티플랫폼 범용 GPU 컴퓨팅 시스템

## GPU를 슈퍼컴퓨터로 사용
- GPU 상의 범용 컴퓨팅 (GPGPU) 
	- 하드웨어는 기본적으로 미니 슈퍼컴퓨터가 있을 정도로 충분히 좋아졌다. 
- CUDA (Compute Unified Device Architecture) 
	- NVIDIA GPU를 위한 범용 병렬 컴퓨팅 플랫폼 
- Vulkan/OpenCL (Open Computing Language) 
	- 일반적인 이기종 컴퓨팅 프레임워크
- 다양한 언어의 확장으로 접근할 수 있다. 
	- 파이썬에 관심이 있다면 Theano, pyCUDA 
		- Theano: https://en.wikipedia.org/wiki/Theano_(software) 
		- pyCUDA: https://developer.nvidia.com/cuda-python 
- • 향후 GPU 프로그래밍 환경

# 커널
- GPU에서 병렬 실행되는 명령어의 모음/함수이다.
- 일반적으로 데이터 병렬성을 활용하기 위해 많은 수의 스레드를 생성한다.

## Summary 
- CPU는 동시 프로세스 유형에 적합하지만 대규모 병렬 계산에는 적합하지 않다. 
- GPU에서는 context switching 없이 여러 스레드를 동시에 실행할 수 있다.



# 리눅스 기본 명령어 소개

## Linux 기본 명령어

### 리눅스의 특징
- 오픈 소스
- 다중 사용자 지원 OS (Operating System)
- CLI (Command-line interface)로부터 비롯됨

### 리눅스 종류
- RedHat : RHEL, CentOS, Fedora
- Debian: Debian, Ubuntu, GNU, Kali Linux

필수 CLI 명령어
- **pwd**
	- Print Working Directory의 준말
	- 현재 어느 디렉터리에 있는 지 알려줌
- **cd [디렉터리 이름]**
	- Change Directory의 준말
	- 다른 디렉터리로 이동, [디렉터리 이름]에 [..] 입력 시 상위 디렉터리로 이동
		- 예) cd.. : 상위 디렉터리로 이동
	- . : 점 하나는 현재 디렉터리를 의미
- **ls**
	- 현재 디렉터리에 있는 파일과 하위 디렉터리 목록을 보여줌
	- -a : All의 준말, 경로 안의 모든 파일을 나열 (숨김 파일을 포함)
	- -l : Long의 준말, 파일들을 자세히 출력
	- Ex) ls -al : 현재 디렉터리에 있는 모든 파일과 하위 디렉터리 목록을 자세히 출력
- **vi**
	- visual editor의 준말
	- [입력] 이름을 가진 새로운 문서 생성 및 작성
	- 리눅스 환경에서 가장 많이 쓰이는 문서 편집기로, 윈도우의 Notepad와 유사
	- vi file txt : file.txt 이름의 빈 파일 생성
- **vi 명령**
	- vi에는 명령 모드, 라인 모드, 입력 모드가 있음
	- 기본 상태는 명령 모드로, 아래와 같은 명령어 사용 가능 
	- 입력 모드로 전환하기 위해서 i키를 누름. 빨간 네모처럼 INSERT (paste)로 변경
- **cat [입력]**
	- Concatenate의 준말
	- 파일의 내용을 화면에 출력
- **mv[입력]  [출력]**
	- Move의 준말
	- [입력]을 [출력]으로 이동시킴
	- Ex) mv hello.txt : hello.txt 파일을 상위 디렉터리로 이동
- **cp [입력]  [출력]**
	- Copy의 준말
	- [입력]을 [출력]으로 복사함
	- -r : Recursive의 준말, 디렉터리 하위 파일도 모두 복사
	- Ex) cp -r Dir1 Dir2/ : Dir1 디렉터리를 Dir2 디렉터리로 복사
- **rm [입력]**
	- Remove의 준말
	- [입력] 파일을 삭제
	- 매우 위험한 명령어로 사용에 주의
	- -f: Force의 준말, 강제로 삭제
	- -r : Recursive의 준말, 디렉터리 삭제 시 모든 하위 파일 삭제
	- Ex) rm- r Dir1 : 강제로 Dir1 디렉터리와 하위 파일을 삭제
**- mkdir [입력]**
	- Make Directory의 준말
	- [입력] 이름을 가진 새로운 디렉터리 생성
	- Ex) mkdir Dir2 : Dir2 이름의 디렉터리 생성

### 특수 기호
- 명령어 사용 간 특수기호 활용 설명
- *
	- Ex) cp test* ./Dir1
	- 위 명령어는 이름이 test로 시작하는 모든 파일을 Dir1 디렉터리에 복사하라는 명령
	- 주로 *.c, *.cpp 등 특정 확장자를 가진 모든 파일을 다룰 때 용이
- ?
	- Ex) cp test? ./Dir1
		- *과 유사하나, 단일 문제에만 적용됨. 예를 들어 test1, test2, testa와 같은 파일만 복사되며, test11과 같은 파일은 복사되지 않음.
- ./
	- Ex) ./test.exe
	- test.exe 파일을 실행시키려면 위와 같이 파일 앞에 ./ 입력

## Make
- Make [-f makefile]  [option] target
	- 다른 파일에서 파생된 파일을 업데이트하는 도구
	- make의 기본 파일은 ./makefile, ./Makefile, ./s makefile
	- 다른 파일을 지정할 때, -f 옵션을 사용하여 파일을 지정할 수 있음
		- make -f makefile1
	- Makefile의 세 가지 구성 요소
		- 매크로 (Macros): 상수 정의
		- 타겟 규칙 (Target rules) : 대상을 만드는 방법을 지정
		- 추론 규칙 (Inference rules) : 임시적으로 대상을 만드는 방법을 지정.
		- make는 추론 규칙을 사용 전에 대상 규칙이 적용되는 지 먼저 확인
- $@ 
	- 규칙의 target 파일 이름. Target이 아카이브 멤버라면, ‘$@‘는 아카이브 파일의 이름. 
	- 여러 대상이 있는 패턴 규칙에서 ‘$@’는 규칙을 실행한 Target의 이름 
- $< 
	- 첫 번째 전제 조건(prerequisite)의 이름. Target이 암시적 규칙에서 설명서를 얻었다면, 
	- 이것은 암시적 규칙에 의해 추가된 첫 번째 전제 조건이 됨. 
- $^
	- 모든 전제 조건(prerequisite)

## summary
- Linux는 GPU 프로그램을 비롯해 많은 플랫폼의 기본 운영체제이다.
- CLI 기반 터미널을 이용하여 다양한 명령어를 수행할 수 있다.