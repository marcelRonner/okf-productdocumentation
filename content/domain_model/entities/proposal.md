---
title: "Proposal"
type: entity
description: "Represents a commercial offer from a partner, displayed on an ATM during a withdrawal of a matching amount."
weight: 13
tags: [proposal, partner, atm]
sources:
  - id: domain-model
    resource: /domain_model/domainModel.md
  - id: cr-1-5
    resource: /sources/cr_1_5.md
generated:
  by: jarvis/1.0
  at: 2026-10-30T14:51:33Z
---

# Proposal

Represents a commercial offer from a partner, displayed on an ATM during a withdrawal of a matching amount. Part of the [Domain Model](../domainModel.md).

## Attributes

| Attribute       | Type     | Description                                  |
|-----------------|----------|----------------------------------------------|
| proposalId      | String   | Unique identifier of the proposal           |
| text            | String   | The offer as displayed on the ATM           |
| minAmount       | Decimal  | Lowest withdrawal amount the proposal matches |
| maxAmount       | Decimal  | Highest withdrawal amount the proposal matches |
| validFrom       | Date     | First day the proposal may be displayed     |
| validUntil      | Date     | Last day the proposal may be displayed      |
