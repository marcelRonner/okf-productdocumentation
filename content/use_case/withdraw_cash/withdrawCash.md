---
title: "Use Case – Withdraw Cash"
linkTitle: "Withdraw Cash"
type: use-case
description: "This use case describes the cash withdrawal flow at an ATM."
weight: 5
tags: [atm, bank, customer, account, card, session, transaction, cash-dispenser, cash-cassette, audit-log, partner, proposal]
sources:
  - id: use-case-diagram
    resource: /use_case/useCaseDiagram.md
  - id: business-process
    resource: /business_process/businessProcess.md
  - id: domain-model
    resource: /domain_model/domainModel.md
  - id: cr-1-5
    resource: /sources/cr_1_5.md
generated:
  by: jarvis/1.0
  at: 2026-10-30T14:51:33Z
verified:
  - by: human:owner
    at: 2026-09-13T09:23:45Z
---

# Use Case – Withdraw Cash

> [!NOTE] v1.5 – Changed 2026-10-30
> Matching Partner Proposals are displayed while the Customer retrieves the Card.

## Overview

This use case describes the cash withdrawal flow at an ATM. It corresponds to **Business Process steps 4a.1 – 4a.9**.[^business-process] See also the [Use-Case Diagram](../useCaseDiagram.md) and the [Business Process](../../business_process/businessProcess.md).

---

## Preconditions

- A Session with status `ACTIVE` exists (Customer is authenticated)
- The Customer has selected "Withdrawal" as the transaction type

## Postconditions

**Success:**
- A Transaction of type `WITHDRAWAL` is created with status `PENDING`
- The Card is ejected before cash dispensing
- If the ATM holds a matching Proposal, it is displayed until the Card is retrieved, and an AuditLog entry has been created
- After successful cash dispensing, the Transaction status is updated to `COMPLETED`
- The Account balance is debited by the withdrawal amount
- Cash has been physically dispensed via the CashDispenser
- CashCassette quantities are updated
- An AuditLog entry has been created

**Failure – dispensing error:**
- A Transaction is created with status `PENDING`
- The Transaction status is updated to `FAILED`
- The Account debit is reversed
- An AuditLog entry has been created
- Customer is informed of the error

**Failure – insufficient funds:**
- No Transaction is created, Account balance unchanged
- Customer is informed and returned to the transaction selection

**Failure – ATM out of cash:**
- No Transaction is created, Account balance unchanged
- Customer is informed and returned to the transaction selection

---

## Description

The Customer enters the desired withdrawal amount. The system checks whether the amount is within the Account `dailyLimit`. If not, the Customer is informed. Next the Bank Backend verifies that the Account has sufficient balance. If funds are insufficient, an error is shown. The ATM then checks whether the CashDispenser holds enough cash. If all checks pass, the Bank Backend creates a Transaction with status `PENDING`, debits the Account, and the ATM ejects the Card. While the Customer retrieves the Card, the ATM displays up to three Proposals matching the withdrawal amount, if it holds any, alongside the prompt to retrieve the Card; a Partner Proposal is display only. Once the Customer retrieves the Card, the ATM dispenses cash. On successful dispensing the Transaction is updated to `COMPLETED`, CashCassette quantities are adjusted, and an AuditLog entry is written. If dispensing fails, the Account debit is reversed and the Transaction is updated to `FAILED`. The Session remains `ACTIVE` so the Customer can perform another transaction (requiring Card re-insertion). See also the [Transaction State Chart](../../state_chart/transactionStateChart.md).

---

## Activity Diagram

![Activity Diagram – Withdraw Cash](withdrawCash.puml)

[^business-process]: [Business Process](../../business_process/businessProcess.md)
