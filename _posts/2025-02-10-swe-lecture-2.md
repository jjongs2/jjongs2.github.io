---
title: "[SWE] Lecture 2"
description: ASPICE 개요
date: 2025-02-10 10:29:26 +0900
categories: [Automotive Embedded Systems, Software Engineering]
---

## Background

---

전장 소프트웨어 시스템이 복잡해짐에 따라, 완성차 업체(OEM)는 **'프로세스와 제품 품질이 모두 우수한 업체를 어떻게 선정할 것인가?'**라는 과제에 직면하게 되었다. 단순히 제품만 잘 만드는 것이 아니라, 지속 가능한 품질 확보를 위해 협력사의 개발 역량을 평가할 수 있는 통일된 기준이 필요해졌다.

이에 따라 Audi, BMW, VW 등 유럽 OEM 업체들이 연합하여 HIS(Hersteller Initiative Software) 그룹을 구성하였고, ISO/IEC 12207(SW 생명주기)과 ISO/IEC 330xx(프로세스 평가) 표준을 기반으로 자동차 산업에 특화된 **Automotive SPICE (ASPICE)** 모델을 개발하였다.

> ASPICE는 자동차 부품 업체들의 **개발 프로세스를 개선하고, 그 품질과 성숙도를 평가하기 위한 객관적인 기준을 제시한다.**
{: .prompt-info }

<br>

## Model Structure

---

ASPICE는 크게 두 가지 모델로 구성된다.

**프로세스 참조 모델 (Process Reference Model, PRM)**

- **평가 대상**을 정의한다.
- 각 프로세스의 ID, 이름, 목적(purpose), 성과(outcomes)를 정의한다.
- 무엇(what)을 달성해야 하는지 정의하지만, 어떻게(how) 달성할지는 명시하지 않는다.

**프로세스 평가 모델 (Process Assessment Model, PAM)**

- **평가 지표**를 정의한다.
- 프로세스 성과와 속성이 실제 프로젝트에서 달성되었는지를 평가하기 위한 기준을 제공한다.
- 측정 프레임워크는 ISO/IEC 33020을 채택하여 등급(N, P, L, F)을 산정한다.

### Process Categories

프로세스 참조 모델은 3개의 카테고리, 11개의 프로세스 그룹, 32개의 개별 프로세스로 구성된다. (v4.0 기준)

![](/posts/20250210/aspice-prm-overview.png){: .border }
_Process reference model overview[^aspice]_

### Capability Levels

프로세스 평가를 통해 **능력 수준(Capability Level, CL)**을 6단계로 구분한다.

| Level    |             | Description                                                    |
| -------- | ----------- | -------------------------------------------------------------- |
| **CL 0** | Incomplete  | 프로세스가 구현되지 않았거나 목적을 달성하지 못함              |
| **CL 1** | Performed   | 프로세스가 수행되고 성과(산출물)가 존재하지만, 체계적이지 않음 |
| **CL 2** | Managed     | 프로세스가 계획되고, 모니터링되며, 산출물이 관리됨             |
| **CL 3** | Established | 조직 차원의 표준 프로세스가 정립되고 테일러링되어 사용됨       |
| **CL 4** | Predictable | 프로세스가 정량적으로 관리되고 예측 가능함                     |
| **CL 5** | Innovating  | 지속적인 개선 활동을 통해 프로세스 혁신이 이루어짐             |

<br>

## Key Concepts

---

### The "Plug-in" Concept

시스템 엔지니어링(SYS) 프로세스를 V-모델의 최상위에 두고, 개발하려는 제품의 특성에 따라 **필요한 도메인 프로세스를 플러그인처럼 결합하는 구조**이다.

![](/posts/20250210/aspice-plug-in-concept.png){: .border }
_"Plug-in" concept[^aspice]_

### Traceability & Consistency

- **양방향 추적성 (Bidirectional Traceability):** 요구사항부터 설계, 구현, 테스트까지 상호 연결되어야 한다. 이는 변경 영향도 분석과 검증 커버리지 확인을 가능하게 한다.
- **일관성 (Consistency):** 연결된 산출물 간의 내용이 논리적으로 모순 없이 일치해야 한다. 단순한 링크 연결(Traceability)뿐만 아니라 실질적인 내용의 일치(Consistency)가 필수적이다.

![](/posts/20250210/aspice-traceability-consistency.png){: .border }
_Traceability and consistency[^aspice]_

### Agree, Summarize and Communicate

**Communicate agreed...**

- V-모델의 왼쪽(설계 단계)에서는 주요 산출물(요구사항, 인터페이스 등)에 대해 관련 당사자 간의 **합의(Agree)**가 선행된 후 공유되어야 한다.

**Summarize and communicate...**

- V-모델의 오른쪽(검증 단계)에서는 방대한 테스트 결과를 그대로 던지는 것이 아니라, 결과를 **요약(Summarize)**하여 관련 당사자에게 공유해야 한다.

### V&V (Verification & Validation)

**Verification (검증)**

- **"제품을 올바르게 만들었는가?"**
- 요구사항 및 설계 사양 준수 여부를 확인한다.

> **Testing (v3.1) → Verification (v4.0):** 테스트뿐만 아니라 리뷰, 분석, 시뮬레이션 등을 포괄하는 **Verification**으로 용어가 확장되었다.
{: .prompt-info }

**Validation (확인)**

- **"올바른 제품을 만들었는가?"**
- 의도된 사용 환경(operational environment)에서 사용자의 기대와 요구를 충족하는지 확인한다.

### Work Products vs Information Items

ASPICE 4.0에서 **Information Item**이라는 용어가 도입되었다.

- **Work Product (작업 산출물):** 조직이 프로세스를 수행하면서 실제로 생성한 결과물 (문서, 소스 코드, 툴 내의 데이터 등)
- **Information Item (정보 항목):** 심사원이 프로세스 속성 달성 여부를 판단하기 위해 참고하는 지표(indicator)

즉, 심사원은 조직이 만든 work product가 해당 information item의 특성(characteristics)을 적절히 반영하고 있는지 확인한다. 이는 문서의 형식이 아니라 **정보의 내용과 실체**가 중요함을 의미한다.

<br>

## References

---

### Footnote

[^aspice]:
    [_Automotive SPICE® 4.0_, VDA QMC,
    Dec. 2023. [Online].](https://vda-qmc.de/wp-content/uploads/2023/12/Automotive-SPICE-PAM-v40.pdf)
