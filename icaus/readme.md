# DEX - Upside Academy Audit

**hakid19* DEX에 대한 감사 보고서*

---

## 1. 요약 (Executive Summary)

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.03
- **감사 종료일**: 2024.09.03
- **감사자**: bob
- **주요 발견 사항**:
  - 1 개의 중간 위험도 이슈 (Medium Issues)
  - 1 개의 낮은 위험도 이슈 (Low Issues)
  - 1 개의 정보성 이슈 (Informational Issues)

---

## 2. 감사 개요 (Audit Overview)

- **감사 이름**: DEX_Audit
- **대상 버전 / 커밋 ID**:
  - [Dex.sol Source Code](https://github.com/hakid29/Dex_solidity/blob/main/src/Dex.sol/)
  - [Commit ID: 0f49cc9c5179fd611c90028dc67f8a10378d23b8](https://github.com/hakid29/Dex_solidity/commit/0f49cc9c5179fd611c90028dc67f8a10378d23b8)
- **적용 기술**: Smart contracts
- **기술 스택**: Smart contracts [Solidity]

---

## 3. 심각도 범주 (Severity Categories)

- **치명적 (Critical)**: 공격 비용이 낮고, 서비스의 가용성에 영향을 주거나 금전적 이득을 취할 수 있는 취약점.
- **높음 (High)**: 서비스 운영에 명확한 문제를 초래할 수 있는 취약점. 공격 비용이 높더라도 영향이 큰 경우 포함.
- **중간 (Medium)**: 특정 조건 하에서 서비스에 예기치 않은 영향을 줄 수 있는 취약점.
- **낮음 (Low)**: 서비스에 경미한 영향을 줄 수 있는 취약점. 성공률이 낮거나 영향이 적음.
- **정보성 (Informational)**: 사용자나 프로토콜에 직접적인 영향을 미치지 않는 정보성 발견 사항.

---

## 4. 취약점 요약 (Finding Breakdown by Severity)

각 취약점의 심각도에 따른 분류를 요약합니다.

- **치명적 (Critical)**
  - 없음
- **높음 (High)**
  - 없음
- **중간 (Medium)**
  - **#1**
- **낮음 (Low)**
  - **#3**
- **정보성 (Informational)**
  - **#2**

---

## 5. 상세 취약점 (Detailed Findings)

### **취약점 #1:** `addLiquidity` 함수의 초기 유동성 문제

- **심각도 (Severity)**: **중간 (Medium)**
- **라인 (Line)**: `src/Dex.sol : 30`
- **설명 (Description)**
  - 초기 유동성을 작게 공급하면 초기 가격이 실제 시장의 가격과 많은 차이를 보이게 된다.
- **영향 (Impact)**
  - 초기에 이를 인지하지 못 한 유저가 스왑을 하게 되면 실제 거래가 보다 훨씬 상회하는 금액을 지불하고 토큰을 사는 현상이 발생 할 수 있다.
- **추천 사항 (Recommendation)**
  - 최소 유동성을 제시하거나, 신뢰 할 수 있는 계약자로부터 유동성을 공급받는다.

### **취약점 #2:** 하드코딩된 수수료

- **심각도 (Severity)**: **정보성 (Informational)**
- **라인 (Line)**: `src/Dex.sol : 14`
- **설명 (Description)**
  - 하드 코딩된 수수료로 인해 이후에 수수료에 대한 수정이 불가능하다.
- **영향 (Impact)**
  - 수수료 정책을 수정함에 따라 유동적으로 수수료를 관리할 수 없게 되어, 시장의 흐름에 맞출 수 없게 됨.
- **추천 사항 (Recommendation)**
  - `FeeRate`에 대한 수정 권한을 owner에게 주고 수정할 수 있는 코드 구현.

### **취약점 #3:** `addLiquidity` 함수에서 토큰 전송 이전의 유효성 검사

- **심각도 (Severity)**: **낮음 (Low)**
- **라인 (Line)**: `src/Dex.sol : 86`
- **설명 (Description)**
  - 토큰 전송 전 유효성 검사를 수행하지 않으면 금액이 없는 상태에서도 함수가 호출될 수 있다.
- **영향 (Impact)**
  - 가스비의 낭비로 이어질 수 있다.
- **추천 사항 (Recommendation)**
  - 전송 이전 유효성 검사 로직 추가.
