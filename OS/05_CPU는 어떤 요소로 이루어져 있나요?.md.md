---
layout: post
title: CPU는 어떤 요소로 이루어져 있나요?
author: MJ
date: 2025-04-01 15:39:00 +0900 
categories: [OS]
banner:
  image: ""
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [OS, COMPUTER, CPU]
---


# 🧠 CPU의 핵심 구성 요소 3가지

CPU는 보통 다음의 세 가지 핵심 부품으로 구성됩니다:

1. **제어 장치 (Control Unit)**
2. **레지스터 (Register)**
3. **산술 논리 연산 장치 (ALU: Arithmetic Logic Unit)**

---

## 🔸 1. 제어 장치 (Control Unit, CU) 
: 말 그대로 CPU를 컨트롤하는 장치이다.
ex)운전자

### ✅ 역할:
- 프로그램 명령어를 **가져오고(Fetch)**, **해석하고(Decode)**, **실행 흐름을 제어(Execute)**함
- ALU, 레지스터, 메모리, 입출력 장치 등과 **동기화** 역할을 함

### 📦 포함 구성 요소:
- **Program Counter (PC)**: 다음 실행할 명령어의 주소 저장하는 레지스터
- **Instruction Register (IR)**: 명령어 레지스터, 현재 실행 중인 명령어를 저장
- **Instruction Decoder**: 명령어의 이진 코드를 해석하여 어떤 동작인지 분석
- **Control Signal Generator**: ALU, 레지스터, 버스 등에 보낼 Control Signal 생성

_등등 사실 MAR,MDR 등등 더 많은데 일단 이정도 선에서 정리했습니다. 더 자세한 정보가 필요하다면 CPU중 Control Unit 에 대해 더 찾아보시는 것을 추천드립니다._

> 💬 **비유하자면, 지휘자** 역할을 해. 각 부품이 언제 어떤 일을 해야 할지 알려주는 역할!

---

## 🔸 2. 레지스터 (Register) 
ex)차안의 도구들

레지스터는 **CPU 내부의 초고속 임시 저장 장치**로,  
명령어 처리와 연산 과정에서 데이터를 저장하거나 제어 정보를 유지하는 데 사용됩니다.

---

## 📦 레지스터의 분류

### 1. **제어 및 상태 레지스터**
| 이름 | 설명 |
|------|------|
| **PC (Program Counter)** | 다음 명령어의 주소를 저장 |
| **IR (Instruction Register)** | 현재 실행 중인 명령어 저장 |
| **Flags Register (상태 레지스터)** | Zero, Carry, Overflow 등 상태 비트 저장 |

---

### 2. **일반 목적 레지스터 (GPR, General Purpose Register)**  
- 계산이나 임시 저장 등에 자유롭게 사용됨  
- 예: R0 ~ Rn (RAX, RBX, ECX, EDX 등 아키텍처에 따라 이름 다름)

---

### 3. **특수 목적 내부 레지스터 (설계/마이크로명령 관점)**  
> 이건 CPU 내부 마이크로 연산 제어 흐름에서 주로 등장하는 **임시 저장용 레지스터**들이야.

| 레지스터 | 설명 |
|----------|------|
| **TMPREG (Temporary Register)** | 임시 저장 공간. 연산 중간 결과, 주소값 등을 잠깐 저장 |
| **RREG (Read Register)** | 메모리 또는 레지스터에서 읽은 값을 담는 레지스터 |
| **CREG (Control Register)** | 마이크로명령어 해석 시 제어 신호 저장 |
| **OUTREG (Output Register)** | ALU의 연산 결과를 저장해 다음 처리 단계로 전달 |
| **ACC (Accumulator)** | 일부 구조에서 사용하는 누산기. 결과를 누적 저장 |

> 이런 레지스터는 마치 내부 CPU 설계도를 그릴 때 나오는 부품 느낌이고,  
> **ALU 연산 처리 흐름이나 마이크로오퍼레이션 순서 제어**에 핵심적인 역할을 해.

---
## 🔸 3. 산술 논리 연산 장치 (ALU)

**ALU(산술 논리 연산 장치)**는 "Arithmetic(산술) Logic Unit"의 약자로 CPU 내부에서 **산술 연산과 논리 연산을 담당하는 핵심 컴포넌트**입니다.

> 📌 CPU가 계산하거나 판단하는 모든 연산은 ALU를 통해 수행됩니다.

---

## 🧩 ALU의 주요 기능

| 기능 종류 | 설명 | 예시 |
|-----------|------|------|
| **산술 연산** | 덧셈, 뺄셈, 곱셈, 나눗셈 등 | `5 + 3`, `R1 - R2` |
| **논리 연산** | AND, OR, NOT, XOR 등 | `A AND B`, `~C` |
| **비교 연산** | 두 값의 대소 비교 | `A > B`, `A == B` |
| **시프트 연산** | 비트를 좌/우로 이동 | `A << 1`, `B >> 2` |

---

## 🛠️ ALU 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| **입력 레지스터 (Input Registers)** | 연산에 필요한 피연산자들이 들어옴 (보통 레지스터에서 가져옴) |
| **산술 회로** | 덧셈기(adder), 감산기(subtractor) 등을 포함 |
| **논리 회로** | AND, OR, NOT 등을 수행하는 논리 게이트 |
| **시프트 회로** | 비트를 좌/우로 이동 |
| **플래그 레지스터 (Flags)** | 연산 결과의 상태를 저장 (Zero, Carry, Overflow 등) |
| **결과 레지스터 (Output Register)** | 연산 결과를 저장해서 다음 처리 단계로 전달 |

---

_만약 ALU가 어떻게 연산을 하는 지 궁금하다면 해당 원리에 대해 따로 찾아봅시다._

## 📌 세 구성요소의 관계 흐름
[1] 제어 장치가 명령어를 메모리에서 가져오고 해석함
[2] 필요한 데이터를 레지스터에서 가져오거나 저장
[3] 연산이 필요하면 ALU에 지시
[4] 연산 결과를 다시 레지스터나 메모리에 저장