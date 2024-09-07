### DEX - Upside Academy audit

 *icarus_DEX에 대한 감사 보고서*

---

### **1. 요약 (Executive Summary)**

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.04
- **감사 종료일**: 2024.09.04
- **감사자**: bob
- **주요 발견 사항**:
    - 2개의 높은 위험도 이슈 (High Issues)
    - 1 개의 정보성 이슈 (Informational Issues)

---

### **2. 감사 개요 (Audit Overview)**

- **감사 이름**: DEX_Audit
- **대상 버전 / 커밋 ID**:
    - https://github.com/pluto1011/-Lending-DEX-_solidity/blob/main/src/DEX.sol / https://github.com/pluto1011/-Lending-DEX-_solidity/commit/d22ce57192079037f493b5eee3cad624fdfc2617
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
    - #1
    - #2
- **중간 (Medium)**
    - 
- **낮음 (Low)**
    - 
- **정보성 (Informational)**
    - #3

---

### **5. 상세 취약점 (Detailed Findings)**

### **취약점 #1: `addLiquidity` 함수의 LP토큰 미지급**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 30
- **설명 (Description)**
    - `addLiquidity` 함수에서 `liquidity[msg.sender] += lpTokens` 를 통해 토큰을 지급하는 것으로 보이지만 민팅이 이뤄지지 않기 때문에 사용자는 실제로 해당 토큰을 받지 않고 사용 할 수 없다.
- **영향 (Impact)**
    - 유동성을 제공하고 보상을 전혀 받지 않기 때문에 사용자들은 사실상 해당 프로토콜을 사용 할 이유를 상실 및 손해만 보게된다.
- **추천 사항 (Recommendation)**
    - 토큰을 발행하고 _mint를 통해 토큰을 실제로 발행해주는 로직이 필요로 하다.

### **취약점 #2: `addLiquidity` 함수에서 amount에 대한 비율 검증 미흡**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 30
- **설명 (Description)**
    - 비율과 다른 값을 넣어도 기존 비율에 따라 계산을 하고 지급을 하고있다.
- **영향 (Impact)**
    - 스왑이 수시로 일어나면서 비율이 바뀔수도 있고 유동성을 공급하려는 사람이 비율을 모르고 있다면 일방적으로 손실를 보게된다.
- **추천 사항 (Recommendation)**
    - 오버플로우 및 언더플로우를 방지하기 위한 검사를 추가해야 합니다.

### **취약점 #3:** `addLiquidity`, `swap` 함수에서 `transferFrom` 이전의 유효성 검사

- **심각도 (Severity)**: **정보성 (Informational)**
- **라인 (Line)**: src/Dex.sol : 30, 84
- **설명 (Description)**
    - 토큰 전송 전 유효성 검사를 수행하지 않으면 allowance ****금액이 없는 상태에서도 함수가 호출 될 수 있다.
- **영향 (Impact)**
    - 가스비의 낭비로 이어 질 수 있다.
- **추천 사항 (Recommendation)**
    - 전송 이전 유효성 검사 로직 추가.