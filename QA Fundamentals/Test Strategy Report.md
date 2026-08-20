# Test Strategy Report

### E-Commerce Website Platform

**Version 1.0**

**Prepared by:** Aline NZIKWINKUNDA
**Date:** August 2026

---

## Table of Contents

- [1. Introduction and Objectives](#1-introduction-and-objectives)
  - [1.1 Introduction](#11-introduction)
  - [1.2 Objectives](#12-objectives)
  - [1.3 Assumptions and Constraints](#13-assumptions-and-constraints)
- [2. Scope and Approach](#2-scope-and-approach)
  - [2.1 Scope](#21-scope)
  - [2.2 SDLC Approach](#22-sdlc-approach)
- [3. Types of Testing](#3-types-of-testing)
  - [3.1 Functional Testing](#31-functional-testing)
  - [3.2 Non-Functional Testing](#32-non-functional-testing)
  - [3.3 Maintenance Testing](#33-maintenance-testing)
- [4. Test Environment](#4-test-environment)
- [5. Risk Analysis and Mitigation](#5-risk-analysis-and-mitigation)
  - [5.1 Product Risks](#51-product-risks)
  - [5.2 Project Risks](#52-project-risks)
- [6. Tools](#6-tools)
- [7. Roles & Responsibilities](#7-roles--responsibilities)
- [8. Test Estimation](#8-test-estimation)
- [9. Defect Management Process](#9-defect-management-process)
  - [9.1 Severity vs. Priority](#91-severity-vs-priority)
- [10. Test Deliverables](#10-test-deliverables)

---

## 1. Introduction and Objectives

### 1.1 Introduction

This document defines the test strategy for a new e-commerce website that allows users to browse products, add items to a shopping cart, complete checkout, and track orders. The platform must remain reliable and responsive during high-traffic sales events and must integrate securely with multiple third-party payment gateways.

The strategy adopts a hybrid approach to the software development lifecycle: a structured, V-Model-style rigor for high-risk components such as payment processing and security, paired with an iterative, Agile-style approach for general browsing and catalog features that are likely to evolve based on user feedback.

### 1.2 Objectives

- Verify that all core user workflows (browsing, cart, checkout, order tracking) function correctly, with at least 95% of planned test cases passing before release.
- Ensure the payment processing flow is secure, accurate, and compliant with relevant industry standards (e.g., PCI-DSS), with zero critical security defects open at release.
- Confirm the website performs consistently across major browsers, devices, and screen sizes with no functionality-blocking defects on any supported combination.
- Validate integration points with external systems, including payment gateways and inventory management, confirming accurate data exchange in all tested scenarios.
- Establish a repeatable, well-documented testing process that supports future releases and regression cycles.

### 1.3 Assumptions and Constraints

**Assumptions:**

- Requirements for core workflows are finalized and stable before test case development begins.
- Sandbox accounts for all payment gateway providers will be available before the Test Environment Setup phase.

**Constraints:**

- Testing must be completed within the project's fixed release timeline.
- Load testing is limited to simulated traffic; testing against real production traffic during an actual sale event is out of scope.

---

## 2. Scope and Approach

### 2.1 Scope

**In Scope:**

- Product browsing and search functionality
- Shopping cart operations (add, update, remove items)
- Checkout process, including guest and registered-user flows
- Payment gateway integration (multiple providers)
- Order tracking and order history
- Inventory synchronization during purchase
- Performance under high-traffic (sale event) conditions

**Out of Scope:**

- Internal warehouse and logistics fulfillment systems (tested separately by the logistics team)
- Third-party payment gateway internal infrastructure (only the integration points are tested)

### 2.2 SDLC Approach

Given the mixed risk profile of this project, a hybrid SDLC approach is recommended:

- V-Model rigor for payment processing and security-critical modules: formal requirement-to-test-case traceability, heavy documentation, and sign-off before release, since defects here carry financial and compliance risk.
- Agile, iterative approach for catalog browsing, search, and general UI features: allowing the team to adapt quickly to user feedback and design changes without renegotiating a rigid test plan each time.

This mirrors the core lesson from the V-Model: match the rigidity of your process to the stability of the requirements. Payment and security requirements are stable and regulated; catalog and UI requirements are expected to evolve.

---

## 3. Types of Testing

Testing is organized into the three categories used throughout this module: Functional, Non-Functional, and Maintenance.

### 3.1 Functional Testing

- **Unit Testing**: individual components (e.g., price calculation, discount logic) tested in isolation by developers.
- **Integration Testing**: verifying that the cart, checkout, payment gateway, and inventory modules work together correctly.
- **System Testing**: end-to-end validation of complete user journeys, from product search through order confirmation.
- **User Acceptance Testing (UAT)**: confirming the platform meets real business and customer needs before release.
- **Smoke and Sanity Testing**: quick checks after each build to confirm core flows (login, add to cart, checkout) are not broken before deeper testing begins.

### 3.2 Non-Functional Testing

- **Performance Testing**: measuring page load times and response times under normal load.
- **Load and Stress Testing**: simulating high-traffic sales events to identify the system's breaking point.
- **Security Testing**: verifying that payment data and customer information are protected against common vulnerabilities.
- **Compatibility Testing**: validating consistent behavior across major browsers (Chrome, Brave, Firefox, Safari, Edge) and devices (desktop, tablet, mobile).
- **Usability Testing**: evaluating whether the checkout flow is intuitive enough to minimize cart abandonment.

### 3.3 Maintenance Testing

- **Regression Testing**: re-running existing test cases after any code change (e.g., adding a new payment provider) to confirm nothing that worked before was broken.
- **Impact Analysis**: assessing which parts of the system are affected by a change before testing begins, so effort is focused on the right areas.
- **Configuration Testing**: confirming the application behaves correctly across different supported configurations, such as OS versions or payment gateway settings.

---

## 4. Test Environment

The test environment will be configured to mirror production as closely as possible, in line with the STLC's Test Environment Setup phase. A smoke test will be run immediately after setup to confirm environment stability before full test execution begins.

| Component | Configuration |
|---|---|
| Operating System | Windows 11 / Ubuntu 22.04 (server-side) |
| Browsers | Chrome, Brave, Firefox, Safari, Edge (latest two versions each) |
| Devices | Desktop, Android and iOS mobile devices, tablets |
| Database | MySQL staging instance seeded with representative product and order data |
| Payment Gateways | Sandbox/test accounts for each integrated payment provider |
| Network Conditions | Standard broadband and throttled 3G/4G profiles for mobile performance checks |
| Test Data | Synthetic customer accounts, product catalog, and order histories (no real customer data used) |

**Entry criteria:** staging servers provisioned, application build deployed, test data loaded, and a passing smoke test.

**Exit criteria:** all planned test cases have been executed, with at least 95% pass rate achieved and no unresolved Critical or High severity defects remaining, before the environment is released for the next test cycle.

---

## 5. Risk Analysis and Mitigation

Risk Score = Likelihood × Impact, using Low = 1, Medium = 2, High = 3. A score of 1–2 is treated as Low risk, 3–4 as Medium risk, and 6–9 as High risk. Higher-scored risks receive testing priority.

### 5.1 Product Risks

| Risk | Likelihood | Impact | Score | Level | Mitigation |
|---|---|---|---|---|---|
| Payment gateway failure or security breach | Medium (2) | High (3) | 6 | High | Security testing on payment flows; PCI-DSS compliance checks; penetration testing before release. |
| System slowdown or crash during high-traffic sales events | High (3) | High (3) | 9 | High | Load and stress testing simulating peak traffic, with defined performance benchmarks (e.g., maximum page load time). |
| Inventory synchronization errors (overselling, stale stock counts) | Medium (2) | Medium (2) | 4 | Medium | Integration testing on the inventory system; automated regression on logic changes. |
| Cross-browser or cross-device inconsistencies | Medium (2) | Medium (2) | 4 | Medium | Compatibility testing prioritized by real traffic analytics. |
| Cart abandonment due to a confusing checkout flow | Low (1) | Medium (2) | 2 | Low | Usability testing with real user scenarios and clear, testable acceptance criteria. |

### 5.2 Project Risks

| Risk | Likelihood | Impact | Score | Level | Mitigation |
|---|---|---|---|---|---|
| Key tester unavailable during a critical test cycle | Medium (2) | Medium (2) | 4 | Medium | Cross-train team members; keep test documentation current. |
| Delay receiving finalized payment gateway API details from a vendor | Medium (2) | High (3) | 6 | High | Engage vendors early; add schedule buffer; escalate blockers promptly. |

---

## 6. Tools

| Testing Activity | Tool | Justification |
|---|---|---|
| Functional / Regression Testing | Selenium | Automates browser-based user workflows such as login, add-to-cart, and checkout. |
| Performance / Load Testing | Apache JMeter | Simulates large numbers of concurrent users for high-traffic sale scenarios. |
| Security Testing | OWASP ZAP | Identifies vulnerabilities such as injection flaws or insecure handling of payment data. |
| API Testing | Postman | Validates payment gateway and inventory API integrations. |
| Compatibility Testing | BrowserStack | Provides access to real browser/device combinations without owning physical hardware. |
| Defect Tracking & Traceability | JIRA | Tracks defects through their lifecycle and maintains the Requirements Traceability Matrix. |

---

## 7. Roles & Responsibilities

| Role | Responsibilities | Reports To |
|---|---|---|
| Test Manager / Lead | Owns the test strategy, approves the test plan, tracks progress, and reports to stakeholders. | Project Manager |
| Test Analyst | Analyzes requirements, builds the Requirements Traceability Matrix, and identifies testable components. | Test Manager |
| Test Designer | Writes detailed test cases and prepares test data for each in-scope feature. | Test Manager |
| Manual/Automation Testers | Execute test cases, log defects, retest fixes, and perform regression testing. | Test Manager |
| Test Environment Administrator | Sets up and maintains the staging environment, ensures data and configurations stay in sync with production. | Test Manager |
| Security Tester | Runs dedicated security and penetration testing on payment and authentication flows. | Test Manager |
| Developers | Fix logged defects and support root-cause investigation during retesting. | Development Lead |

---

## 8. Test Estimation

Estimation will combine two techniques:

- **Work Breakdown Structure (WBS)** — testing effort is broken into discrete tasks and estimated individually before being summed into a total effort figure.
- **Three-Point Estimation** — for tasks with higher uncertainty, three numbers are estimated per task: Optimistic (O), Most Likely (M), and Pessimistic (P). These are combined using the PERT formula: `Estimate = (O + 4M + P) / 6`.

| Testing Task | Estimated Effort | Method |
|---|---|---|
| Functional Testing (core workflows) | 80 hours | WBS |
| Integration Testing (payment, inventory) | 50 hours | WBS |
| Compatibility Testing | 25 hours | WBS |
| UAT Support | 20 hours | WBS |

**Three-Point Estimation detail:**

| Task | Optimistic | Most Likely | Pessimistic | Estimate (O+4M+P)/6 |
|---|---|---|---|---|
| Performance & Load Testing | 30 hrs | 38 hrs | 58 hrs | 40 hours |
| Security Testing | 25 hrs | 33 hrs | 53 hrs | 35 hours |

**Total estimated testing effort: 250 person-hours.**

**Resource assumption:** this estimate assumes a team of 5 testers (test analyst, test designer, two manual/automation testers, and a security tester) each dedicating an average of 10 hours per week to this project, giving the team a combined capacity of 50 hours per week. At that rate, the 250-hour estimate translates to approximately 5 weeks of test execution.

---

## 9. Defect Management Process

All defects will be tracked through the standard defect lifecycle, from initial discovery through to closure:

- **New**: a defect is logged by a tester with clear reproduction steps.
- **Assigned**: the defect is routed to the responsible developer.
- **Open**: the developer investigates and works on a fix, or moves the defect to Rejected, Deferred, or Duplicate if applicable.
- **Fixed → Pending Retest → Retest**: the fix is implemented and verified by the original tester.
- **Verified / Reopen**: confirmed fixes are marked Verified; unresolved issues are Reopened and returned to the developer.
- **Closed**: the defect is fully resolved and confirmed with no further action needed.

### 9.1 Severity vs. Priority

Severity describes how badly a defect affects the system; priority describes how urgently it needs fixing. The two are tracked separately because a defect can be severe but not urgent, or minor but urgent.

| Level | Severity Example | Priority Example |
|---|---|---|
| Critical | Checkout fails; no orders can be placed | Must be fixed immediately, before release |
| High | Payment succeeds but confirmation email is not sent | Fix before release, high priority in current cycle |
| Medium | Search results display in the wrong sort order | Fix in an upcoming release; not release-blocking |
| Low | Minor spacing issue on the order confirmation page | Can be fixed in a later release |

Defects will be prioritized by severity, with payment and checkout defects treated as highest priority given their direct revenue impact. All defects are logged and tracked in JIRA, linked back to the relevant requirement via the Requirements Traceability Matrix.

---

## 10. Test Deliverables

- Test Strategy Report (this document)
- Requirements Traceability Matrix (RTM), mapping each requirement to its corresponding test case(s)
- Detailed test cases and test scripts for functional, non-functional, and maintenance testing
- Test execution logs and defect reports
- Test Summary Report, including pass/fail counts and outstanding defects at release time
- Sign-off documentation for User Acceptance Testing
