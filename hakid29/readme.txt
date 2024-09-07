# DEX - Upside Academy Audit Report

## 1. Executive Summary

This audit report provides a brief overview of the audit process, including objectives, scope, and key findings.

- **Audit Start Date**: 2024.09.03
- **Audit End Date**: 2024.09.03
- **Auditor**: Bob
- **Key Findings**:
  - 1 Medium Severity Issue
  - 1 Low Severity Issue
  - 1 Informational Issue

---

## 2. Audit Overview

- **Audit Name**: DEX_Audit
- **Target Version / Commit ID**:
  - [Dex.sol Source Code](https://github.com/hakid29/Dex_solidity/blob/main/src/Dex.sol/)
  - [Commit ID: 0f49cc9c5179fd611c90028dc67f8a10378d23b8](https://github.com/hakid29/Dex_solidity/commit/0f49cc9c5179fd611c90028dc67f8a10378d23b8)
- **Technologies Used**: Smart Contracts
- **Technology Stack**: Smart Contracts [Solidity]

---

## 3. Severity Categories

- **Critical**: Vulnerabilities that can be exploited at low cost and impact the availability of the service or allow financial gain.
- **High**: Vulnerabilities that can clearly cause operational issues to the service, including those with high attack costs but significant impacts.
- **Medium**: Vulnerabilities that can have unexpected impacts on the service under certain conditions.
- **Low**: Vulnerabilities that have minimal impact on the service, with low success rates or limited effects.
- **Informational**: Informational findings that do not directly affect users or the protocol.

---

## 4. Finding Breakdown by Severity

A summary of the vulnerabilities categorized by severity.

- **Critical**
  - None
- **High**
  - None
- **Medium**
  - **#1**
- **Low**
  - **#3**
- **Informational**
  - **#2**

---

## 5. Detailed Findings

### **Issue #1: Initial Liquidity Problem in `addLiquidity` Function**

- **Severity**: **Medium**
- **Line**: `src/Dex.sol : 30`
- **Description**:
  - If initial liquidity is supplied in small amounts, the initial price can significantly deviate from the actual market price.
- **Impact**:
  - Users unaware of this may end up paying significantly more than expected when swapping tokens.
- **Recommendation**:
  - Set a minimum liquidity or ensure liquidity is provided by a trusted contract.

---

### **Issue #2: Hardcoded Fees**

- **Severity**: **Informational**
- **Line**: `src/Dex.sol : 14`
- **Description**:
  - The hardcoded fee makes it impossible to modify the fee rate in the future.
- **Impact**:
  - It prevents dynamic management of fees based on market trends.
- **Recommendation**:
  - Implement code that allows the owner to modify the `FeeRate`.

---

### **Issue #3: Validity Check Before Token Transfer in `addLiquidity` Function**

- **Severity**: **Low**
- **Line**: `src/Dex.sol : 86`
- **Description**:
  - Lack of a validity check before token transfer could allow function calls with zero amounts.
- **Impact**:
  - This can lead to unnecessary gas fees.
- **Recommendation**:
  - Add a validity check logic before the transfer.

---

Feel free to use this report as a basis for improving the security and robustness of your DEX smart contract.
