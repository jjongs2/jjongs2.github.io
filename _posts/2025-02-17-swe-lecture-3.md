---
title: "[SWE] Lecture 3"
description: ISO 26262 개요
date: 2025-02-17 08:39:29 +0900
categories: [Automotive Embedded Systems, Software Engineering]
math: true
---

## What is Safety?

---

ISO/IEC Guide 51은 **안전(Safety)**을 **허용 불가능한 위험으로부터의 자유(Freedom from risk which is not tolerable)**로 정의한다. 즉, 어떤 제품이 '안전하다'는 선언은 그 제품에 위험이 없다는 뜻이 아니라, 그 위험이 사회적으로 용인될 수 있는 수준으로 낮게 관리되고 있음을 의미한다.

| Term                   | Definition                                                  |
| :--------------------- | :---------------------------------------------------------- |
| **Harm (위해)**        | 사람의 신체적 부상, 건강 손상 또는 재산 및 환경에 대한 피해 |
| **Hazard (위해 요인)** | 위해를 일으킬 수 있는 잠재적 요인                           |
| **Risk (위험)**        | 위해 발생 확률(Probability)과 심각도(Severity)의 조합       |

> ISO/IEC Guide 51은 다양한 산업 분야에서 안전 표준을 만들 때 따라야 할 공통 프레임워크를 제공하는 최상위 지침서이다.
{: .prompt-info }

<br>

## ISO 26262

---

ISO 26262는 일반 산업용 기능 안전 표준인 IEC 61508을 기반으로, 자동차의 특수한 운용 환경과 개발 생명주기를 반영한 국제 표준이다.

- **적용 대상:** 승용차, 트럭, 버스, 오토바이 (2nd edition 기준)

ISO 26262의 핵심 목적은 "E/E 시스템의 오작동(malfunctioning behavior)으로 인해 발생할 수 있는 hazard에 의한 불합리한 위험(unreasonable risk)의 부재"를 달성하는 것이다. 이는 고장이 절대 발생하지 않는 완벽한 시스템을 만드는 것이 아니라, **위험을 허용 가능한 수준으로 완화**할 수 있는 **기능 안전(Functional Safety)**을 확보하는 것을 의미한다. (e.g. 고장이 발생하더라도 이를 감지하고 safe state로 전환)

> 물리적 특성을 통해 안전을 확보하는 것은 비기능 안전(Non-functional Safety)에 해당한다.
{: .prompt-info }

총 12개의 파트로 구성되어 있으며, 주요 파트는 V-모델 프로세스에 따라 유기적으로 연결되어 있다. 이는 안전이 특정 개발 단계에서 한 번 검증하고 끝나는 것이 아니라, **제품의 탄생부터 소멸까지 지속적으로 관리되어야 함**을 시사한다.

![](/posts/20250203/parts-of-iso-26262.jpg){: .border w="650" }
_Parts of ISO 26262:2018 (2nd edition)_

<br>

## Part 3: Concept Phase

---

### Item Definition

기능 안전 활동의 시작은 분석 대상인 **아이템(Item)**을 명확히 정의하는 것이다. 아이템이란 **차량 레벨에서 기능을 수행하는 시스템**을 의미한다. 예를 들어, '회생 제동 시스템'을 아이템으로 정의한다면, 이 시스템의 기능적 범위, 외부 인터페이스, 작동 환경, 그리고 다른 차량 시스템과의 상호작용 등을 문서화해야 한다. 이는 후속 단계인 HARA의 기초가 되며, 아이템의 경계를 명확히 하여 분석 범위를 확정하는 중요한 단계이다.

### HARA (Hazard Analysis and Risk Assessment)

HARA는 **해저드를 식별하고 위험을 평가하여 안전 목표(Safety Goal)를 도출**하는 과정이다. 위험을 평가하기 위해 다음과 같은 세 가지 척도를 사용한다.

**심각도 (Severity, S)**

|        | Description                                         | Example                                         |
| :----- | :-------------------------------------------------- | :---------------------------------------------- |
| **S0** | 부상 없음                                           | 가벼운 충돌                                     |
| **S1** | 경미한 부상, 중등도 부상                            | 매우 낮은 속도(15 km/h 미만)로 다른 차량과 충돌 |
| **S2** | 심각한 부상, 생명을 위협하는 부상(생존 가능성 높음) | 낮은 속도(16~50 km/h)로 다른 차량과 충돌        |
| **S3** | 생명을 위협하는 부상(생존 불확실), 치명적 부상      | 중간 속도(51~90 km/h)로 다른 차량과 충돌        |

**노출도 (Exposure, E)**

|        | Description    | Duration                  | Frequency             |
| :----- | :------------- | :------------------------ | :-------------------- |
| **E0** | 발생 불가능    | -                         | -                     |
| **E1** | 매우 낮은 확률 | -                         | 연 1회 미만 발생      |
| **E2** | 낮은 확률      | 평균 운행 시간의 1% 미만  | 연 수회 발생          |
| **E3** | 중간 확률      | 평균 운행 시간의 1~10%    | 월 1회 이상 발생      |
| **E4** | 높은 확률      | 평균 운행 시간의 10% 초과 | 거의 매 주행마다 발생 |

**제어 가능성 (Controllability, C)**

|        | Description                          | Example                         |
| :----- | :----------------------------------- | :------------------------------ |
| **C0** | 일반적으로 제어 가능                 | 운전자 보조 시스템 사용 불가    |
| **C1** | 99% 이상의 교통 참여자가 회피 가능   | 차량 시동 시 스티어링 칼럼 잠김 |
| **C2** | 90% 이상의 교통 참여자가 회피 가능   | 긴급 제동 중 ABS 고장           |
| **C3** | 90% 미만의 교통 참여자만이 회피 가능 | 브레이크 고장                   |

### ASIL (Automotive Safety Integrity Level)

위 세 가지 요소의 조합을 통해 ASIL 등급(QM, A, B, C, D)이 결정된다. ASIL D는 가장 높은 수준의 위험을 의미하며, 가장 엄격한 안전 요구사항이 적용된다.

![Determination of ASIL](/posts/20250203/asil-matrix.png)

> 기능 안전 개념(FSC) 적용 사례: [전기차의 고전압 배터리 관리 시스템(BMS)](https://www.irejournals.com/formatedpaper/1708063.pdf)[^fsc-bms]
{: .prompt-info }

<br>

## Part 4~6: Product Development

---

### System Level

Part 3에서 도출된 안전 목표는 아직 추상적인 수준의 요구사항이다. Part 4에서는 이를 **기술 안전 요구사항(Technical Safety Requirement, TSR)**으로 구체화한다. 예를 들어, '배터리 과열 방지'라는 안전 목표는 '배터리 온도가 60도를 초과하면 100ms 이내에 메인 릴레이를 차단한다'와 같은 기술적 요구사항으로 변환된다. 또한, **하드웨어와 소프트웨어 간의 인터페이스**를 명확히 정의하여 두 영역 간의 상호작용에서 발생할 수 있는 오류를 사전에 방지해야 한다.

### Hardware Level

하드웨어의 설계가 안전 목표를 달성하기에 충분한지 평가하기 위해 **정량적 지표**를 요구한다.[^part-5]

| Metric   | Description                                         |  ASIL B   |  ASIL C   |  ASIL D  |
| :------- | :-------------------------------------------------- | :-------: | :-------: | :------: |
| **SPFM** | 단일점 결함 커버리지                                |   ≥ 90%   |   ≥ 97%   |  ≥ 99%   |
| **LFM**  | 잠재적 결함 커버리지                                |   ≥ 60%   |   ≥ 80%   |  ≥ 90%   |
| **PMHF** | 랜덤 하드웨어 고장으로 인해 안전 목표를 위반할 확률 | < 100 FIT | < 100 FIT | < 10 FIT |

- $1 \text{ FIT} = \frac {1 \text{ failure}}{10^9 \text{ hours}}$

> 일반적인 마이크로컨트롤러의 기본 고장률이 수십~수백 FIT에 달할 수 있기 때문에, **ASIL D**의 10 FIT 목표를 달성하기 위해서는 **높은 진단 커버리지를 갖춘 안전 메커니즘(Lock-step, ECC 등)**의 적용이 필수적이다.
{: .prompt-info }

### Software Level

소프트웨어 구현 단계에서는 MISRA C/C++와 같은 **코딩 표준**을 준수하고, **정적 분석**을 통해 런타임 오류를 예방할 것을 요구한다. 검증 단계에서는 ASIL 등급에 따라 구문(Statement), 분기(Branch), MC/DC와 같은 **구조적 코드 커버리지**를 만족해야 하며, 요구사항부터 테스트까지의 **양방향 추적성 확보** 및 **개발 도구의 신뢰성 입증(Tool Qualification)**이 필수적으로 수행되어야 한다.[^part-6]

<br>

## Advanced Topics

---

### SOTIF (ISO 21448)

**자율 주행 시스템**에서는 시스템이 고장나지 않더라도 **기능적 한계**(센서의 성능 한계, AI 알고리즘의 불확실성, 예기치 않은 환경 조건 등)로 인해 사고가 발생할 수 있다. 이를 다루기 위해 ISO 21448(Safety of the Intended Functionality, SOTIF) 표준이 제정되었다.

### Cybersecurity (ISO/SAE 21434)

커넥티드 카의 등장은 해킹이 안전을 위협하는 새로운 요인이 되었다. 해커가 브레이크를 원격으로 제어한다면 이는 기능 안전의 실패로 이어진다. ISO 26262 제2판은 사이버 보안과의 상호작용을 언급하고 있으며, ISO/SAE 21434 표준과 연계하여 보안 위협이 안전 위협으로 전이되지 않도록 **설계 단계에서부터 보안을 고려해야 함(Security by Design)**을 강조한다.

<br>

## References

---

- _Safety Aspects — Guidelines for Their Inclusion in Standards_, ISO/IEC Guide 51, 2014.
- _Road Vehicles — Functional Safety_, ISO 26262, 2018.

### Footnote

[^fsc-bms]:
    [J. A, "ISO 26262 compliant high-voltage battery system functional safety concept,"
    _Icon. Res. Eng. J._, vol. 8, no. 10, pp. 978–985, Apr. 2025. [Online].](https://www.irejournals.com/formatedpaper/1708063.pdf)

[^part-5]: [S. Dahl, "Implementing ISO 26262-5: A guide to Functional Safety for Product Development at Hardware Level," M.S. thesis, Dept. Ind. Mater. Sci., Chalmers Univ. Technol., Gothenburg, Sweden, 2023. [Online].](https://odr.chalmers.se/server/api/core/bitstreams/99654b5f-ba7e-4ffe-8571-b476d8285842/content)
[^part-6]: [Parasoft, "ISO 26262 Software Compliance in the Automotive Industry." [Online].](https://alm.parasoft.com/hubfs/ISO26262-Software-Compliance-Automotive.pdf)
