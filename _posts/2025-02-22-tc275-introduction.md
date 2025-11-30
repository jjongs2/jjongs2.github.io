---
title: Introduction
description: AURIX™ TC275 ShieldBuddy
date: 2025-02-22 10:15:53 +0900
categories: [Embedded Systems, Development Boards]
tags: [tc275]
image:
  path: https://assets.infineon.com/is/image/infineon/hitex-shieldbuddytc275.jpeg
---

## TC275 ShieldBuddy

---

Hitex사에서 제작한 개발 보드로, Infineon사의 AURIX™ TC275 마이크로컨트롤러를 탑재하고 있다. Arduino Due/Mega2560과 동일한 폼 팩터를 사용하여 300개 이상의 Arduino 실드와 호환되며, 이를 통해 다양한 센서, 액추에이터, 통신 모듈 등을 연결하여 시스템을 손쉽게 확장할 수 있다.

![](https://assets.infineon.com/is/image/infineon/hitex-shieldbuddytc275.jpeg){: .border w="300" }
_TC275 ShieldBuddy[^tc275-sb]_

### AURIX™ TC275

32비트 TriCore™ 아키텍처 기반의 프로세서 3개로 구성된 MCU이다. 각 코어는 200MHz의 클럭 속도로 동작하며, 전용 FPU(Floating Point Unit)를 갖추고 있어 복잡한 수학 연산과 신호 처리 작업을 효율적으로 처리한다.

![](/posts/20250222/tc275-internal-layout.png){: .border w="550" }
![](/posts/20250222/tc275-peripherals.png){: .border w="550" }
_TC275 internal layout[^tc275]_

3개의 코어는 공유 버스에 연결되어 있으며, 각자 로컬 RAM을 가지고 플래시 메모리를 공유한다. 총 4MB의 플래시 메모리와 472KB의 SRAM을 제공하며, 두 메모리 모두 ECC(Error Correction Code) 기능을 지원하여 높은 데이터 무결성이 요구되는 애플리케이션에도 적합하다. 또한, 디버그 인터페이스가 내장되어 있어 컴퓨터에 직접 연결하여 프로그래밍 및 디버깅을 수행할 수 있으며, 각종 통신 인터페이스를 포함하여 다양한 모듈을 지원한다.

> 자동차 기능 안전 표준인 ISO 26262의 ASIL-D 등급까지 지원하며, AUTOSAR V3.2 및 V4.x 표준과 호환되도록 설계되어 자동차 산업의 시스템 개발에 특화되어 있다.
> {: .prompt-info }

<br>

## Simple Projects

---

여기서는 **TC275 ShieldBuddy**를 활용한 기본적인 프로젝트들을 소개하고자 한다. 간단한 디지털 입출력부터 시작하여 인터럽트 처리, 타이머 제어, ADC(Analog-to-Digital Converter), PWM(Pulse Width Modulation)까지 임베디드 시스템에서 주로 사용되는 하드웨어 기능들을 단계적으로 살펴볼 것이다.

|                     Name                     | Abstract                                        |
| :------------------------------------------: | :---------------------------------------------- |
| [**GPIO**]({{ site.url }}/posts/tc275-gpio/) | 스위치 상태에 따라 LED 제어                     |
|  [**ERU**]({{ site.url }}/posts/tc275-eru/)  | 스위치로 외부 인터럽트를 발생시켜 LED 제어      |
|  [**STM**]({{ site.url }}/posts/tc275-stm/)  | STM 인터럽트를 활용하여 작업 스케줄러 구현      |
|  [**ADC**]({{ site.url }}/posts/tc275-adc/)  | 아날로그 신호(가변저항) 측정                    |
|  [**PWM**]({{ site.url }}/posts/tc275-pwm/)  | ADC 값에 따라 PWM 신호를 생성하여 LED 밝기 조절 |

> [GitHub 리포지토리](https://github.com/jjongs2/tc275) 참고
> {: .prompt-info }

### Prerequisites

실습의 편의성을 위해 Arduino 실드 중 하나인 **Easy Module Shield V1**을 개발 보드에 장착하여 사용한다. 다양한 센서와 액추에이터를 포함하고 있어, 따로 부품을 구매하고 연결하는 번거로움 없이 바로 프로젝트를 시작할 수 있다.

- 통합 개발 환경 ([AURIX™ Development Studio](https://softwaretools.infineon.com/tools/com.ifx.tb.tool.aurixide) 등)
- AURIX™ TC275 개발 보드 (KIT_AURIX_TC275_ARD_SB)
- Easy Module Shield V1

  ![](/posts/20250222/easy-module-shield-v1.jpg){: .border w="500" }
  _YwRobot Easy Module Shield V1[^shield]_

### Usage

1. AURIX™ Development Studio 실행
2. **File** > **New** > **New AURIX Project** 선택
3. Target Device 선택 (hitex ShieldBuddy, KIT_AURIX_TC275_ARD_SB, TC27xTP_D-Step)

   ![Select target device](/posts/20250222/target-device.png){: w="550" }

4. 프로젝트 생성
5. 소스 파일과 헤더 파일을 프로젝트 디렉터리에 복사
   - KIT_AURIX_TC275_LITE를 사용하는 경우, 코드에 정의(`#define`)되어 있는 핀 할당을 하드웨어 구성에 맞게 변경해 주어야 한다.
6. 프로젝트를 빌드하고 보드에 플래시하여 실행

<br>

## References

---

- ["AURIX™ Family – TC27xT." Infineon Technologies. [Online].](https://www.infineon.com/cms/en/product/microcontroller/32-bit-tricore-microcontroller/32-bit-tricore-aurix-tc2xx/aurix-family-tc27xt/)

### Footnote

[^tc275-sb]: ["KIT_AURIX_TC275_ARD_SB." Infineon Technologies. [Online].](https://www.infineon.com/cms/en/product/evaluation-boards/kit_aurix_tc275_ard_sb/)
[^tc275]: [Hitex \(U.K.\) Limited. _TC275 ShieldBuddy User Manual v2.8_. [Online].](https://www.infineon.com/dgdl/Infineon-ShieldBuddy_TC275%20-UM-v02_08-EN.pdf?fileId=5546d46269e1c019016a54f0801a5590&da=t)
[^shield]: [YwRobot. "아두이노 우노 Easy Module Shield V1 [ARD040110]." 디바이스마트. [Online].](https://www.devicemart.co.kr/goods/view?no=1279493)
