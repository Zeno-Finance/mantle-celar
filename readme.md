# Celar — Multichain Payment Orchestration (MNEE Hackathon)

Celar is a multichain payment orchestration backend that allows platforms and PSPs to accept stablecoin payments, automatically route them, and track payment state changes in real time.

This submission demonstrates how **MNEE** can be used as a **payment currency in a real payin flow**, alongside existing stablecoins (USDC / USDT), using the same orchestration and settlement logic.

## What This Project Does

Celar provides:

* A **Payin API** to initiate stablecoin payments
* **Automatic route selection** across supported chains (`chain = best`)
* **Deterministic intermediary wallet generation** per payment
* A **listener service** that monitors on-chain transfers
* **Webhook events** for payment lifecycle updates
* **Strict currency validation**, including controlled usage of MNEE

The goal is to show that **MNEE behaves like a first-class payment asset**

## How MNEE Is Used (Important)

In this project

* MNEE can be selected as the `currency` when initiating a payin
* The system:

  * resolves the correct token contract,
  * generates a unique intermediary wallet,
  * waits for an on-chain MNEE transfer,
  * and updates the payment state once funds are received
* The same orchestration, listener, and webhook logic used for USDC/USDT is reused for MNEE

This demonstrates how MNEE can be integrated into an existing payment stack.


## Supported Chains & Tokens

| Chain    | Supported Tokens |
| -------- | ---------------- |
| Ethereum | MNEE             |
| Base     | USDC, USDT       |
| Polygon  | USDC, USDT       |
| Arbitrum | USDC, USDT       |
| Testnets | USDC, USDT       |

Availability depends on environment configuration.

## Project Architecture (High Level)

The project runs as **two services**:

### API Service

* Initiates payins
* Resolves routing and token addresses
* Generates intermediary wallets
* Emits webhook events

### Listener Service

* Watches blockchains for incoming token transfers
* Confirms payments
* Updates payment status

Both services must be running for the full flow to work.

## Setup Instructions

### 1. Install Dependencies

```bash
npm install --legacy-peer-deps
```

---

### 2. Configure Environment

Duplicate the example file:

```bash
cp .env.example .env
```
fill in all the required environment variables

### 3. Start the Application

Run the API:

```bash
npm run start
```

Run the listener in a second terminal:

```bash
npm run start:listener
```

## How to Test the Flow

1. Call the **Payin Initiation API** with:

   * `amount`
   * `currency` (`USDC`, `USDT`, or `MNEE`)
   * `chain` (`best` or a specific chain)

2. The API returns:

   * resolved chain
   * token address
   * intermediary wallet

3. Send the specified token (including MNEE) to the intermediary wallet.

4. The listener detects the transfer and updates the payment state.

5. Webhooks (if configured) emit lifecycle events.

## Documentation

Full API documentation is available at:

👉 **[https://docs.celar.io/](https://docs.celar.io/)**


## Functionality Requirement (Judging)

This project:

* installs successfully using the provided instructions
* runs consistently on the intended platform
* functions as described in this README
* demonstrates **practical usage of MNEE in a payment flow**
* matches the behavior shown in the demo video

## License

This project is released under the **MIT License**.
See the `LICENSE` file for details.
