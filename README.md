
# BTC-SolarSwap Green Energy Trading Platform Smart Contract

A Clarity smart contract for managing a decentralized platform that facilitates **green energy credit trading** between producers and consumers, including mechanisms for **production verification**, **credit transactions**, and **participant management**.

---

## 📜 Overview

This smart contract enables:
- Registration and verification of **energy producers**.
- Registration of **energy consumers**.
- Recording of **green energy production**.
- **Trading** of green energy credits between participants.
- Administration of platform-wide settings (e.g., commission rate, trade limits).

The contract ensures transparency and accountability for all transactions using Stacks' blockchain features.

---

## ⚙️ Core Components

### 📌 Data Structures

#### `energy-producers` (Map)
Stores registered producers.
```clarity
principal => {
  cumulative-energy-produced: uint,
  producer-verification-status: bool,
  producer-registration-timestamp: uint,
  energy-unit-price: uint
}
```

#### `energy-consumers` (Map)
Stores registered consumers.
```clarity
principal => {
  cumulative-energy-purchased: uint,
  available-energy-credits: uint,
  consumer-registration-timestamp: uint
}
```

#### `energy-trading-records` (Map)
Stores each completed trade.
```clarity
uint => {
  energy-seller: principal,
  energy-buyer: principal,
  energy-amount: uint,
  transaction-price: uint,
  transaction-timestamp: uint,
  trade-status: (string-ascii 20)
}
```

### 🛠️ Variables

- `energy-trade-sequence`: Counter for trade IDs.
- `platform-administrator`: Admin address.
- `minimum-tradeable-energy`: Minimum energy amount required for trade (default: 100 units).
- `platform-commission-rate`: Percentage fee taken by the platform (default: 2%).
- `maximum-energy-price`: Max price allowed per unit (default: 1,000,000).

---

## ✅ Functional Overview

### 👥 Participant Management

- **`register-energy-producer(initial-energy-price)`**
  - Registers a producer with an initial unit price.
  - Requires price to be > 0 and <= max limit.

- **`register-energy-consumer()`**
  - Registers a new energy consumer.

- **`verify-energy-producer(producer-address)`** (Admin only)
  - Marks a producer as verified before they can record energy production.

---

### ⚡ Energy Management

- **`record-green-energy-production(production-amount)`**
  - Verified producers can record produced energy.

---

### 🔁 Trading

- **`create-energy-trade(producer-address, energy-amount)`**
  - Consumers can buy energy from producers.
  - Transfers credits, logs transaction, and increments sequence.
  - Calculates platform commission internally.

---

### 🧾 Read-Only Queries

- `get-energy-producer-details(producer-address)`
- `get-energy-consumer-details(consumer-address)`
- `get-energy-trade-details(trade-id)`
- `get-platform-commission-rate()`

---

### 🛡️ Admin Controls

- **`update-platform-commission-rate(new-rate)`**
  - Must be ≤ 100.

- **`update-minimum-tradeable-energy(new-minimum)`**
  - Must be > 0.

---

## ❗ Error Codes

| Code | Error Description |
|------|--------------------|
| `u100` | Unauthorized access |
| `u101` | Invalid energy amount |
| `u102` | Insufficient energy balance |
| `u103` | Energy producer not found |
| `u104` | Energy consumer not found |
| `u105` | Participant already registered |
| `u106` | Invalid trade status |
| `u107` | Invalid energy price |
| `u108` | Invalid producer address |

---

## 🧪 Initialization

On contract deployment:
- `energy-trade-sequence` set to `0`
- `minimum-tradeable-energy` set to `100`
- `platform-commission-rate` set to `2`
- `maximum-energy-price` set to `1,000,000`

---

## 🔐 Security & Integrity

- **Verified production:** Only verified producers can log energy.
- **Authorization enforced:** Only admin can update settings or verify producers.
- **Trade validation:** Ensures sufficient balance, valid amounts, and price limits.

---

## 🧰 Suggested Enhancements

- Add event logging (when supported) for transparency.
- Integrate token-based payments for cross-chain trading.
- Support trade cancellation and refunds.

---
