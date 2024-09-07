### DEX - Upside Academy audit

 **Kyrie*_DEX에 대한 감사 보고서*

---

### **1. 요약 (Executive Summary)**

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.03
- **감사 종료일**: 2024.09.03
- **감사자**: bob
- **주요 발견 사항**:
    - 1 개의 치명적 이슈 (**High** Issues)
    - 1 개의 높은 위험도 이슈 (Medium Issues)
    - 1 개의 중간 위험도 이슈 (Low Issues)

---

### **2. 감사 개요 (Audit Overview)**

- **감사 이름**: DEX_Audit
- **대상 버전 / 커밋 ID**:
    - https://github.com/je1att0/DEX_solidity/blob/main/src/DEX.sol / https://github.com/je1att0/DEX_solidity/commit/6974ccbabbfdf24a32ca8390aa39dbc4a0f58929
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
    - **#1**
- **높음 (High)**
    - #2
- **중간 (Medium)**
    - **#3**
- **낮음 (Low)**
    - 
- **정보성 (Informational)**
    - 

---

### **5. 상세 취약점 (Detailed Findings)**

### **취약점 #1: LP토큰 max 발행**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 8
- **설명 (Description)**
    - 향후 컨트랙에 발행 되어 있는 LP토큰을 던져버리면 가격에 치명적인 문제를 야기 할 수 있다. 사용자가 해당 컨트랙을 100퍼센트 신뢰를 하고 이용을 하게 돼야 한다.
- **영향 (Impact)**
    - 컨트랙의 소유자가 러그풀을 할 수 있는 상황이 우려 될 수 있다.
- **추천 사항 (Recommendation)**
    - 사용자들에게 적확한 양 만큼 발행을 시켜주는 방식을 채택한다.

### **취약점 #2:** `removeLiquidity`함수에서의 유효성 검사

- **심각도 (Severity)**: **중간 (Medium)**
- **라인 (Line)**: src/Dex.sol : 49
- **설명 (Description)**
    - `totalLP` 이 없다면(0이라면) 0 division이 발생 할 수 있다.
- **영향 (Impact)**
    - 호출될 필요가 없는 함수의 호출로 가스비의 낭비가 이뤄질 수 있다.
- **추천 사항 (Recommendation)**
    - 유동성을 제공한 사람이 호출하도록 하며, 0값에 대한 검증을 한다.

### **취약점 #3:** `removeLiquidity` 검증 및 로직 문제

- **심각도 (Severity)**: **치명적 (Critical)**
- **라인 (Line)**: src/Dex.sol : 49
- **설명 (Description)**
    - LPtoken에 대한 반환이 전혀 이뤄지지 않고 유저에 대한 검증이 없기 때문에 아무나 해당 함수를 호출해서 token을 발행 받을 수 있다.
- **영향 (Impact)**
    - 공격자는 유동성 공급없이 유동성 공급을 누군가 한다면 해당 함수를 호출해서 토큰을 받아 갈 수 있다.
- **추천 사항 (Recommendation)**
    - 유동성을 제공한 사람이 호출하도록 하며, 해당 유저의 유동성 공급량을 검증한다.