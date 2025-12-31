---
title: "[SWE] Lecture 1"
description: 자동차 소프트웨어 개발 프로세스 개요
date: 2025-02-03 07:45:52 +0900
categories: [Automotive Embedded Systems, Software Engineering]
---

## Era of SDV

---

지난 한 세기 동안 자동차 산업은 정밀 기계 공학의 결정체였다. 엔진의 성능, 섀시의 강성, 서스펜션의 튜닝이 차량의 가치를 결정짓는 핵심 요소였으며, 차량의 기능은 공장에서 조립되어 출고되는 순간 그대로 고정되었다.

그러나 오늘날 4차 산업혁명의 핵심인 ICT(인공지능, IoT, 자율주행) 기술이 자동차에 접목되면서, 자동차 산업은 **SDV(Software Defined Vehicle)** 시대를 맞이하고 있다. 이제 자동차는 단순히 기계 장치가 아니라, 소프트웨어로 구동되는 거대한 모빌리티 디바이스이다.

### Growth of Software Complexity

자동차 제조 원가 중 전장 부품이 차지하는 비중은 1970년 약 5%에서 2020년 약 40%까지 증가하였다.[^electronics] 이에 따라 차량에 탑재되는 소프트웨어의 규모(Lines Of Code, LOC) 또한 기하급수적으로 증가하고 있으며, 이러한 복잡도의 증가로 인해 **체계적인 소프트웨어 관리의 중요성**이 부각되고 있다.

| Vehicle Model      | LOC         | Note                       |
| ------------------ | ----------- | -------------------------- |
| VW Beetle (1938)   | 0           | 기계적 제어 중심           |
| VW Golf II (1983)  | 600         | ECU 도입 (Assembly)        |
| VW Golf VII (2012) | 40,000,000  | 내비게이션 및 인포테인먼트 |
| Modern Car         | 100,000,000 | 70~100개의 ECU 탑재        |

> 1억 LOC라는 수치는 F-35 전투기(약 2,400만)와 Windows 7(약 4,000만)을 훨씬 상회하는 규모이다![^loc]
{: .prompt-info }

<br>

## Process & Quality

---

### What is a Process?

프로세스란 **고객의 요구사항을 만족하는 제품을 만들기 위한 절차(Method), 도구(Machine), 인력(Man)의 통합**이다.

### ETVX Model

ETVX는 프로세스의 각 단계를 명확히 정의하기 위해 사용되는 모델이다.

| Component              | Description               |
| ---------------------- | ------------------------- |
| **E** (Entry Criteria) | 작업 시작을 위한 조건     |
| **T** (Task)           | 수행해야 할 구체적인 업무 |
| **V** (Verification)   | 작업 수행 및 산출물 검증  |
| **X** (eXit Criteria)  | 작업 완료를 위한 조건     |

### Two Aspects of Quality

품질은 두 가지 관점에서 종합적으로 관리되어야 한다.

- **프로세스 품질 (Process Quality)**: 올바른 절차를 정의하고 준수하며, 지속적으로 개선하고 있는가?
- **제품 품질 (Product Quality)**: 산출물 간의 추적성과 일관성이 확보되었으며, 고객 요구사항을 만족하는가?

글로벌 자동차 OEM들은 협력 업체에게 높은 수준의 프로세스 품질(e.g. ASPICE 레벨 준수)을 요구하고 있다. 프로세스가 체계화되지 않으면 특정 인력에 대한 의존도가 높아지고, 핵심 인력의 이탈 시 품질이 급격히 저하될 위험이 있기 때문이다.

<br>

## SDLC (Software Development Life Cycle)

---

### V-Model

자동차 소프트웨어 개발에서 가장 널리 사용되는 표준 모델로, 개발의 각 단계와 그에 상응하는 검증 단계를 1:1로 매핑하여 검증 활동을 강조한다.

![Systems Engineering Process II](https://upload.wikimedia.org/wikipedia/commons/e/e8/Systems_Engineering_Process_II.svg){: .bg-white .border }
_V-model process[^v-model]_

> 테스트 설계를 코딩 이후가 아니라, 요구사항 단계부터 미리 준비(**Shift Left**)하여 결함을 조기에 발견하려는 접근 방식이 중요하다.
{: .prompt-tip }

### Industry Standards

자동차 소프트웨어 공학의 복잡성을 관리하고 품질을 보증하기 위해 여러 국제 표준이 제정되었다.

| Domain            | Standard      | Description                               |
| ----------------- | ------------- | ----------------------------------------- |
| Process Quality   | ASPICE        | 자동차 소프트웨어 개발 프로세스 평가 모델 |
| Functional Safety | ISO 26262     | 자동차 기능 안전 국제 표준                |
| Cybersecurity     | ISO/SAE 21434 | 자동차 사이버 보안 표준                   |

---

## References

---

### Footnote

[^electronics]: "Semiconductors – the Next Wave: Opportunities and winning strategies for semiconductor companies," Deloitte, Tech. Rep., Apr. 2019.
[^loc]: ["Million lines of code." Information is Beautiful. [Online].](https://informationisbeautiful.net/visualizations/million-lines-of-code/)
[^v-model]: ["Systems Engineering Process II." Wikimedia Commons. [Online].](https://commons.wikimedia.org/wiki/File:Systems_Engineering_Process_II.svg)
