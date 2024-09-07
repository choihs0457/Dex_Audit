### DEX - Upside Academy audit

 *ella_DEX에 대한 감사 보고서*

---

### **1. 요약 (Executive Summary)**

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.03
- **감사 종료일**: 2024.09.03
- **감사자**: bob
- **주요 발견 사항**:
    - 1개의 높은 위험도 이슈 (High Issues)
    - 2개의 정보성 이슈 (Informational Issues)

---

### **2. 감사 개요 (Audit Overview)**

- **감사 이름**: DEX_Audit
- **대상 버전 / 커밋 ID**:
    - https://github.com/skskgus/Dex_solidity/ https://github.com/skskgus/Dex_solidity/commit/1487df3439ea6e198f14032db91347bb0dbb1c0a
- **적용 기술**: Smart contracts
- **기술 스택**: Smart contracts [Solidity]

---

### **3. 심각도 범주 (Severity Categories)**

- **치명적 (Critical)**: 공격 비용이 낮고, 서비스의 가용성에 영향을 주거나 금전적 이득을 취할 수 있는 취약점.
- **높음 (High)**: 서비스 운영에 명확한 문제를 초래할 수 있는 취약점. 공격 비용이 높더라도 영향이 큰 경우 포함.
- **중간 (Medium)**: 특정 조건 하에서 서비스에 예기치 않은 영향을 줄 수 있는 취약점.
- **낮음 (Low)**: 서비스에 경미한 영향을 줄 수 있는 취약점. 성공률이 낮거나 영향이 적음.
- **정보성 (Informational)**: 사용자나 프로토콜에 직접적인 영향을 미치지 않는 정보성 발견 사항.

---

### **4. 취약점 요약 (Finding Breakdown by Severity)**

각 취약점의 심각도에 따른 분류를 요약합니다.

- **치명적 (Critical)**
    - 
- **높음 (High)**
    - #2
- **중간 (Medium)**
    - 
- **낮음 (Low)**
    - 
- **정보성 (Informational)**
    - #1
    - #3

---

### **5. 상세 취약점 (Detailed Findings)**

### **취약점 #1: `addLiquidity` 함수의 불필요한 계산**

- **심각도 (Severity)**: **정보성 (Informational)**
- **라인 (Line)**: src/Dex.sol : 54
- **설명 (Description)**
    - `addLiquidity` 함수의 `_reserve0`가 balance를 가져와서 사용하는데 토큰의 밸런스를 다시 또 따로 호출함
- **영향 (Impact)**
    - 불필요한 가스비 낭비가 된다.
- **추천 사항 (Recommendation)**
    - balanceOf 대신`_reserve0` 변수 사용을 권장함

### **취약점 #2: `addLiquidity` 함수의 LP토큰 미지급**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 54
- **설명 (Description)**
    - `addLiquidity` 함수에서 `_mint` 를 통해 토큰을 지급하는 것으로 보이지만 erc20 표준의 민팅이 아닌 따로 구현한 함수 `_mint` 여서 실제로 유저에게 토큰이 발행되지 않는다.
- **영향 (Impact)**
    - 유동성을 제공하고 보상을 전혀 받지 않기 때문에 사용자들은 사실상 해당 프로토콜을 사용 할 이유를 상실 및 손해만 보게된다.
- **추천 사항 (Recommendation)**
    - erc20 토큰을 발행하고 표준의 _mint를 통해 토큰을 실제로 발행해주는 로직이 필요로 하다.

와디지몬

### **취약점 #3: `removeLiquidity`** 함수의 유효성 검사 부족

- **심각도 (Severity)**: **정보성 (Informational)**
- **라인 (Line)**: src/Dex.sol : 90
- **설명 (Description)**
    - 0값에 대한 검증이 없기 때문에 누구나 호출 할 수 있는 문제가 생길 수 있다.
- **영향 (Impact)**
    - 사실상 아무런 동작을 안하게 되는 함수의 호출이므로 가스비의 낭비로 이어 질 수 있다.
- **추천 사항 (Recommendation)**
    - 전송 이전 유효성 검사 로직 추가.