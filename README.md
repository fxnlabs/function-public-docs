# Overview

AI today sits behind closed doors: proprietary models, scarce compute, and centralized control leave creators uncompensated and users locked in. **Function Network** tears down these walls with a blockchain-powered protocol that makes inference **open, decentralized, and permissionless**, so anyone can contribute, anyone can consume, and no single entity can censor or gate access.

### The Problem: Open-Source AI Isn’t Accessible for All

* **Zero Creator Revenue**\
  Model authors publish to the world yet earn nothing. Every dollar goes to the hosting or application layer, leaving open-source contributions undervalued and unsustainable.
* **Poor Model Discoverability & Bootstrapping**\
  Even the best models languish unseen: without a clear on-chain marketplace or incentive to spotlight high-quality work, providers won’t host them and developers can’t find or use them.
* **Unsustainable Hosting Costs**\
  Serving inference at scale requires complex orchestration or costly cloud GPUs, putting small teams and individual creators out of reach and stalling innovation.

### Our Solution: Democratize AI with Function Network

1. **Model Marketplace**\
   A unified, onchain registry where creators publish model metadata, weights, and transparent pricing for their IP.\
   Community ratings and usage statistics surface the best models and drive quality improvements.
2. **Onchain Monetization**\
   Usage fees and royalties are settled instantly in FUNC tokens.\
   Fine-grained payout rules let creators set per-model or per-endpoint rates, while providers earn proportional to compute delivered.
3. **Sharded Inference**\
   Break large inference jobs into parallel shards distributed across any GPU provider (from RTX 30-series to datacenter GPUs).\
   Dynamic load-balancing and redundancy guarantee low latency, high throughput, and resilient uptime.
4. **Managed Infrastructure & SLAs**\
   End-to-end orchestration, monitoring, and failover are enforced by smart contracts.\
   Providers commit SLAs onchain: missed targets trigger automated penalties and re-allocation.

### Incentive Alignment: How the Network Works

Every protocol action is powered by **FUNC**:

* **Developers** deposit FUNC to reserve inference capacity and pay per request.
* **Compute Providers** stake FUNC to signal reliability, host models, and collect fees for each inference shard served.
* **Model Creators** receive an onchain royalty split every time their model is invoked, turning downloads into recurring income.

### The Function Flywheel

<figure><img src="function-network/overview/image.png" alt=""><figcaption></figcaption></figure>
