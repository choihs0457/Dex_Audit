### DEX - Upside Academy audit

 **jacqueline*_DEX에 대한 감사 보고서*

---

### **1. 요약 (Executive Summary)**

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.06
- **감사 종료일**: 2024.09.06
- **감사자**: bob
- **주요 발견 사항**:
    - 1개의 치명적 이슈 (Critical Issues)
    - 1개의 높은 위험도 이슈 (High Issues)

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
    - **#2**
- **높음 (High)**
    - #1
- **중간 (Medium)**
    - 
- **낮음 (Low)**
    - 
- **정보성 (Informational)**
    - 

---

### **5. 상세 취약점 (Detailed Findings)**

### **취약점 #1: LP token 을 모두 발행해서 컨트랙이 소유**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 60
- **설명 (Description)**
    - LP토큰에 대해 모두 발행을 해놓고 보내주는 방식으로 구현되어 있기 때문에 유저들은 해당 프로토콜을 100퍼센트 신뢰한 상태로 사용을 해야한다.
- **영향 (Impact)**
    - 컨트랙 소유자가 유동성이 공급된 만큼 토큰을 털어버리면 모든 자산을 빼갈 수 있다.
- **추천 사항 (Recommendation)**
    - 유저가 유동성을 제공함에 따라 자연스럽게 mint 되도록 구현 방식을 수정한다.

### **취약점 #2: `removeLiquidity` 에서 호출한 유저검증 미흡**

- **심각도 (Severity)**: **치명적 (Critical)**
- **라인 (Line)**: src/Dex.sol : 50
- **설명 (Description)**
    - 함수를 아무나 호출 할 수 있는데, 호출하는 유저에 대한 검증을 하지 않는다.
- **영향 (Impact)**
    - 유동성이 공급되어있을 때 아무나 **`removeLiquidity`함수를 호출하면 누구든 유동성을 빼갈 수 있다.**
- **추천 사항 (Recommendation)**
    - 함수 호출에 대한 접근 권한을 제어하고 전체 발행된 토큰량을 기준으로 `burn`을 시키면서 비율만큼 유동성을 반환시켜주는 식으로 로직을 바꾼다.