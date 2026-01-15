# Celar — Stablecoin Payment Orchestration on Mantle

*Mantle Global Hackathon 2025 Submission*

Celar is a **payment orchestration backend** that enables platforms, PSPs, and businesses to accept stablecoin payments, route them on-chain, and track payment lifecycle events in real time.

This submission demonstrates **real-world payment flows executed on the Mantle Network**, using Mantle as the primary execution environment for routing, transfer detection, and settlement.

Celar focuses on **RealFi use cases** — real businesses, real payments, and compliant infrastructure — while providing **developer-grade tooling** for integrating stablecoin payments into production systems.


## Track Alignment

* **RWA / RealFi** — real stablecoin payments, real settlement, real business use cases
* **Infrastructure & Tooling** — Developer tooling enabling developers to intergrate stablecoin payments easily

## What This Project Does

Celar provides the following core capabilities:

* **PSP onboarding API**
* **Merchant management API**
* **Pay-in API** for stablecoin payments
* **Chain-based routing** (Mantle used in this demo)
* **Deterministic intermediary wallet generation** per payment
* **On-chain listener service** monitoring Mantle blocks
* **Real-time webhook events** for payment lifecycle updates
* **Operational dashboard** for observing payment states

This allows platforms to integrate stablecoin payments without directly managing wallets, chain logic, or on-chain monitoring.


## How Mantle Is Used

In this project:

* **Mantle Network is the execution chain used in the demo**
* Stablecoin transfers are **initiated, detected, and confirmed on Mantle**
* The listener service actively:

  * monitors Mantle RPC endpoints
  * scans Mantle blocks for ERC-20 transfers
  * validates payment amount and currency
* Payment state transitions are driven by **on-chain activity on Mantle**

Mantle’s low fees and EVM compatibility make it suitable for high-volume, real-world payment flows.

## Supported Chains & Tokens

| Network | Supported Tokens |
| ------- | ---------------- |
| Mantle  | USDC, USDT       |

## Project Architecture

Celar runs as **two cooperating services**, both required for end-to-end operation.

### API Service

Responsible for:

* PSP registration and API key issuance
* Merchant and customer management
* Pay-in initiation
* Chain and token resolution
* Intermediary wallet generation
* Webhook configuration and emission

### Listener Service

Responsible for:

* Monitoring Mantle blocks
* Detecting incoming ERC-20 transfers
* Validating amount and currency
* Confirming payments
* Updating payment state in the system

> Both services must be running for the full payment flow to function.


## Setup Instructions

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Environment

Create an environment file:

```bash
cp .env.example .env
```

Populate required variables, including:

* Mantle RPC URL
* Token contract addresses (USDC / USDT on Mantle)
* Webhook secrets
* Database configuration


### 3. Start the Services

Run the API service:

```bash
npm run dev
```

Run the listener service in a separate terminal:

```bash
npm run dev:listener
```


## How to Test the Full Pay-In Flow

1. Open the dashboard:
   **[https://dashboard.celar.io](https://dashboard.celar.io)**

2. Complete the **PSP onboarding flow**
   (For hackathon purposes, approval is automatic.)

3. Create a **customer** and copy the `customer_id`.

4. Create an internal Celar wallet for receiving a pay-in.

5. Generate an **API key** and configure a **webhook URL**.

6. Initiate a pay-in using the API:
   **[https://docs.celar.io/api#/operations/create_payin](https://docs.celar.io/api#/operations/create_payin)**

7. The response returns:

   * `payment_id`
   * `intermediary_wallet`
   * `token_address`
   * `chain` (Mantle)

8. Send the specified stablecoin (USDC or USDT) on **Mantle** to the intermediary wallet.

9. The listener detects the transfer and confirms the payment automatically.

## What Happens Internally

1. The API:

   * resolves Mantle chain metadata
   * generates a deterministic intermediary wallet
   * stores routing context

2. The listener:

   * monitors Mantle blocks
   * detects the ERC-20 transfer
   * validates amount and token
   * confirms the payment

3. Webhooks emit lifecycle events in real time.


## Webhooks

If configured, Celar emits events such as:

* `payin.confirmed`
* `payin.settled`
* `payin.failed`
* `payin.mismatched`

These allow downstream systems to react programmatically to payment events.


## Demo & Documentation

* **Dashboard:** [https://dashboard.sandbox.celar.io](https://dashboard.sandbox.celar.io/)
* **API Docs:** [https://docs.celar.io](https://docs.celar.io)
* **Demo Video:** Provided in submission

## Hackathon Requirements

This submission:

* Includes a working MVP running on Mantle
* Demonstrates real on-chain payment execution
* Provides clear setup and testing instructions
* Matches the behavior shown in the demo
* Aligns with Mantle’s focus on RealFi and scalable infrastructure

## License

Released under the **MIT License**.
See the `LICENSE` file for details.