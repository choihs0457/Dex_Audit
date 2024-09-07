### DEX - Upside Academy audit

 **Nullorm*_DEX에 대한 감사 보고서*

---

### **1. 요약 (Executive Summary)**

감사 프로세스에 대한 간략한 개요를 제공하며, 목표, 범위 및 주요 발견 사항에 대한 요약을 포함합니다.

- **감사 시작일**: 2024.09.03
- **감사 종료일**: 2024.09.03
- **감사자**: bob
- **주요 발견 사항**:
    - [개수]개의 치명적 이슈 (Critical Issues)
    - 2개의 높은 위험도 이슈 (High Issues)
    - 1 개의 중간 위험도 이슈 (Medium Issues)
    - [개수]개의 낮은 위험도 이슈 (Low Issues)
    - [개수]개의 정보성 이슈 (Informational Issues)

---

### **2. 감사 개요 (Audit Overview)**

- **감사 이름**: DEX_Audit
- **대상 버전 / 커밋 ID**:
    - https://github.com/Null0RM/DEX_solidity/blob/main/src/Dex.sol / https://github.com/Null0RM/DEX_solidity/commit/abefae97824326faff03b0ba4d6358438766b0bb
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
    - **#2**
- **높음 (High)**
    - 
- **중간 (Medium)**
    - **#3**
- **낮음 (Low)**
    - 
- **정보성 (Informational)**
    - 

---

### **5. 상세 취약점 (Detailed Findings)**

### **취약점 #1: `addLiquidity` 함수에서 amount에 대한 비율 검증 미흡**

- **심각도 (Severity)**: **높음 (High)**
- **라인 (Line)**: src/Dex.sol : 20
- **설명 (Description)**
    - 비율과 다른 값을 넣어도 기존 비율에 따라 계산을 하고 지급을 하고있다.
- **영향 (Impact)**
    - 스왑이 수시로 일어나면서 비율이 바뀔수도 있고 유동성을 공급하려는 사람이 비율을 모르고 있다면 일방적으로 손실를 보게된다.
- **추천 사항 (Recommendation)**
    - 오버플로우 및 언더플로우를 방지하기 위한 검사를 추가해야 합니다.