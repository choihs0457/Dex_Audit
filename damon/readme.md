### DEX - Upside Academy audit

 *damon_DEX에 대한 감사 보고서*

---

### **1. 요약 (Executive Summary)**

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.03
- **감사 종료일**: 2024.09.03
- **감사자**: bob
- **주요 발견 사항**:
    - 1개의 치명적 위험도 이슈 (High Issues)
    - 1 개의 높음 위험도 이슈 (Medium Issues)

---

### **2. 감사 개요 (Audit Overview)**

- **감사 이름**: DEX_Audit
- **대상 버전 / 커밋 ID**:
    - https://github.com/gloomydumber/DEX_solidity/blob/master/src/Dex.sol / https://github.com/gloomydumber/DEX_solidity/commit/1671f2f3a6a272387b4a884e92f9724563d9cb15
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
    - #1
- **높음 (High)**
    - **#2**
- **중간 (Medium)**
    - 
- **낮음 (Low)**
    - 
- **정보성 (Informational)**
    - 

---

### **5. 상세 취약점 (Detailed Findings)**

### **취약점 #1: `update`, `mint` 등 내부적으로 호출 되어야 하는 함수의 access 권한 문제**

- **심각도 (Severity)**: **치명적 (Critical)**
- **라인 (Line)**: src/Dex.sol : 94, 138
- **설명 (Description)**
    - 내부적으로 호출되어야 하는 함수들이 외부 접근자에게도 호출 될 수 있도록 `external` 및 `public`으로 구현되어 있어서 심각한 문제를 야기 할 수 있다.
- **영향 (Impact)**
    - `update` 함수를 호출해서 `reserve`의 양을 최소한으로 낮춰서 `mint`를 호출하면 `liquidity`를 무한정 받을 수 있는데 이를 이용해서 `removeLiquidity`를 하면 프로토콜에 있는 대부분의 토큰을 빼갈 수 있다.
- **추천 사항 (Recommendation)**
    - 내부적으로 호출되어야 하는 함수들의 접근 권한은 internal로 바꾸도록 한다.

### **취약점 #2: `removeLiquidity` 함수의 `this`를 통한 `burn` 호출**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 50
- **설명 (Description)**
    - `removeLiquidity` 함수에서 `this`를 통해 `burn`이 호출 되는데 `burn`에서 LP토큰을 소각하고 이에 상응하는 X, Y 토큰을 돌려줘야하는데 해당 부분에 `msg.sender`에게 반환하는 행위가 유저가 아닌 호출한 컨트랙(렌딩 프로토콜)이 되어 유저에게 토큰이 분배되지 않는다.
- **영향 (Impact)**
    - 유저가 본인의 유동성을 제거하기 위해 LP토큰을 반환하고 X, Y 토큰을 반환 받아야하는데 LP토큰만 보내고 아무런 토큰도 반환 받지 못한다.
- **추천 사항 (Recommendation)**
    - this 를 지워서 해결을 하거나 msg.sender의 정보 또한 함께 넘겨줘서 정상적으로 반환하도록 만든다.