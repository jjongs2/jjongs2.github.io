---
title: "[RTOS] Lecture 2"
description: Real-Time Operating Systems
date: 2025-04-14 08:46:53 +0900
categories: [Automotive Embedded Systems, Real-Time Operating Systems]
math: true
---

## Real-Time Scheduling

---

**스케줄링(Scheduling)**이란 CPU, 네트워크 등 **한정된 자원(resource)을 여러 작업(workload)에 할당**하는 과정이다.

### Objectives of Scheduling

스케줄링은 상충될 수 있는 여러 가지 목표들을 고려해야 한다.

- **공정성 (Fairness)**: 각 프로세스에 동등한 자원 할당
- **기아 방지 (No starvation)**: 모든 프로세스에 최소한의 자원 할당 보장
- **높은 처리량 (High throughput)**: 단위 시간당 처리하는 작업의 수 ↑
- **높은 활용도 (High utilization)**: CPU 이용률 ↑
- **짧은 지연 시간 (Small latency)**: 입출력 지연 ↓
- **높은 상호 작용성 (High interactivity)**: 사용자에게 빠르게 응답
- **데드라인 보장 (Deadline guarantee)**: 정해진 시간 내 작업 완료
- **예측 가능성 (Predictability)**: 예측할 수 있는 스케줄링 동작
- **유연성 (Flexibility)**: 워크로드 변화에 대한 적응

> 실시간 시스템에서는 **예측 가능성**과 **데드라인 보장**이 평균 성능보다 중요하다.
{: .prompt-info }

### Workload & Resource Model

여기서는 다음과 같은 워크로드 및 리소스 모델을 가정한다.

**Periodic Task Model**

$N$개의 주기적 태스크 집합 $\{T_1, T_2, ..., T_N\}$에 대해, 각 태스크 $T_i$를 $(p_i, e_i, d_i)$와 같이 표현할 수 있다.

- $p_i$: 주기 (Period)
- $e_i$: WCET (Worst-Case Execution Time)
- $d_i$: 상대 데드라인 (Relative deadline)

Implicit deadline을 갖는 태스크의 경우 $(p_i, e_i)$로 간단히 표현한다.

> 주기적 태스크 모델은 **미래의 워크로드 패턴을 예측**할 수 있게 해주므로, 최적의 스케줄링을 계획하는 데 중요한 역할을 한다.
{: .prompt-info }

**Resource Model**

- 싱글 코어 CPU를 여러 태스크가 공유
- **선점(Preemption)**이 가능하여, 실행 중인 작업은 context switch를 통해 언제든지 중단되고 다른 작업으로 대체될 수 있음

> **Context**는 특정 시점의 CPU 내부 상태를 의미한다. **Context switch**는 현재 실행 중인 태스크의 상태를 저장하고 다음 태스크의 상태를 복원하는 과정이다.
{: .prompt-info }

<br>

## Typology of Real-Time Scheduling Algorithms

---

실시간 스케줄링 알고리즘은 크게 오프라인과 온라인 방식으로 나눌 수 있다.

### Offline (Cyclic Scheduling)

모든 작업의 시작 시간을 미리 계산하여 테이블로 만들어 두고 비선점적으로 실행한다.

- **장점**: 구현이 간단하고 예측 가능성이 높으며, context switch 오버헤드가 적음
- **단점**: 작업 수가 많아지면 스케줄링 테이블을 만들기가 어렵고, 워크로드 변화에 유연하게 대처하기 힘듦

### Online

작업이 실행될 준비가 되면 **준비 큐(Ready queue)**에 들어가고, 스케줄러는 정해진 정책에 따라 큐에서 다음에 실행할 작업을 선택한다.

**Weighted Round-Robin Scheduling**

- 각 작업에 고유한 시간 할당량(weight)을 부여하고, 준비 큐의 작업을 순서대로 해당 시간만큼 실행
- 주로 시분할(time-sharing) 시스템에서 사용 (Unix, Windows 등)

**Priority-Driven Scheduling**

- 각 작업에 우선순위를 할당하고, 항상 준비 큐에서 가장 높은 우선순위를 가진 작업을 실행
- 대부분의 RTOS가 채택한 방식

| 우선순위 할당 방식         | 대표 알고리즘                                                   |
| :------------------------- | :-------------------------------------------------------------- |
| Task-level Fixed Priority  | **RM(Rate Monotonic)**, DM(Deadline Monotonic)                  |
| Job-level Fixed Priority   | **EDF(Earliest Deadline First)**, LLF(Least Laxity First), FIFO |
| Job-level Dynamic Priority | LST(Least Slack Time First)                                     |

<br>

## Fixed Priority Scheduling

---

각 태스크에 고정된 우선순위를 부여하는 방식으로, 단순함 덕분에 대부분의 RTOS에서 지원한다.

### Optimal Priority Assignment Policies

스케줄링 알고리즘이 최적(optimal)이라는 것은, **어떤 태스크 집합이 특정 알고리즘으로 스케줄링 가능하다면 최적 알고리즘으로도 반드시 스케줄링 가능함**을 의미한다.

**RM (Rate Monotonic)**

- 주기가 짧을수록 높은 우선순위 부여
- Implicit deadline을 갖는 주기적 태스크 집합에 대해 최적[^liu-layland]

**DM (Deadline Monotonic)**

- 상대 데드라인이 짧을수록 높은 우선순위 부여
- Arbitary deadline을 갖는 주기적 태스크 집합에 대해 최적

### Critical Instant Theorem

어떤 태스크의 **임계 순간(Critical instant)**은 해당 태스크가 **자신보다 우선순위가 높은 모든 태스크와 동시에 릴리스될 때** 발생한다.[^liu-layland] 임계 순간이란, 특정 태스크에 대한 요청이 발생했을 때 그 태스크가 가장 긴 응답 시간을 갖게 되는 시점을 의미한다.

이 정리를 통해, 스케줄링이 가능한지를 판단하기 위해 모든 경우를 시뮬레이션할 필요 없이 최악의 시나리오(임계 순간)만 확인하면 된다. 즉, **모든 태스크가 각자의 임계 순간에 데드라인을 지킬 수 있다면, 그 스케줄링 알고리즘은 항상 실행 가능(feasible)**하다고 볼 수 있다.

### Worst-Case Response Time Analysis

**응답 시간(Response time)**이란 작업이 릴리스된 시점부터 실행을 완료하기까지 걸리는 시간이다. 임계 순간 정리(Critical Instant Theorem)에 따라, 태스크 $T_i$의 **최악 응답 시간(Worst-Case Response Time, WCRT)** $r_i$를 다음과 같은 재귀 방정식으로 계산할 수 있다.

$$
\begin{align*}
r_i^0 &= e_i + \sum_{T_j \in hp(T_i)} e_j \\[2pt]
r_i^{k+1} &= e_i + \sum_{T_j \in hp(T_i)} \left\lceil \frac{r_i^k}{p_j} \right\rceil e_j \quad (k = 0, \ 1, \ 2, \ ⋯)
\end{align*}
$$

- $hp(T_i)$: $T_i$보다 높은 우선순위를 가진 태스크 집합

$r_i$가 수렴할 때까지 반복하여 계산한다. 최종적으로 **모든 태스크가 $r_i \leq d_i$를 만족하면, 해당 태스크 집합은 스케줄링 가능**하다.

### Utilization Bound

**CPU 이용률(Utilization)**은 각 태스크의 실행 시간을 주기로 나눈 값의 합이다.

$$
U = \sum_{i=1}^{N} \frac{e_i}{p_i}
$$

RM 알고리즘의 경우, 스케줄링 가능성을 보장하는 이용률 상한선이 존재한다.[^liu-layland]

$$
\begin{align*}
U_{RM}(n) &= n(2^{1/n} - 1) \\[8pt]
\quad \lim_{n\to\infty} U_{RM}(n) &= \ln 2 \approx 0.693
\end{align*}
$$

Implicit deadline을 갖는 주기적 태스크 집합에 대해 RM이 최적이므로, **$U \leq U_{RM}$이면 해당 태스크 집합은 스케줄링 가능**하다. 만약 $U > U_{RM}$이라면, 스케줄링 가능성을 판단하기 위해 WCRT 분석을 수행해야 한다.

<br>

## References

---

[^liu-layland]: [C. L. Liu and J. W. Layland, "Scheduling algorithms for multiprogramming in a hard-real-time environment," _Journal of the ACM_, vol. 20, no. 1, pp. 46-61, Jan. 1973, doi: 10.1145/321738.321743.](https://dl.acm.org/doi/10.1145/321738.321743)
