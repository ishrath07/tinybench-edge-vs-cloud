# TinyBench Edge vs Cloud

A deployment-focused analysis that reframes TinyBench's published throughput and power measurements into **cost per million tokens, tokens per watt, and edge-vs-cloud inference economics**.

## Overview

This project takes TinyBench's sustained-load performance data and converts it into a practical deployment decision framework.

Instead of looking only at raw tokens/second or power consumption, the analysis asks:

> **What does it actually cost to run inference locally on edge hardware versus renting comparable compute in the cloud?**

The analysis compares:

- NVIDIA RTX 4050 laptop GPU
- Hailo-10H NPU
- iPhone 16 Pro
- Samsung Galaxy S24 Ultra
- RTX 4050-class cloud compute

The comparison focuses on **sustained inference performance**, energy efficiency, and cost per million tokens.

---

## Key Findings

### 1. Local inference is significantly cheaper per token

Using the published throughput and power figures, local inference costs approximately:

| Platform | Cost / 1M Tokens | Tokens / Watt |
|---|---:|---:|
| RTX 4050 Laptop | $0.0053 | 3.86 |
| Hailo-10H NPU | $0.0060 | 3.45 |
| iPhone 16 Pro* | $0.0064 | 3.23 |
| RTX 4050-class Cloud | $0.1476 | — |

The analysis shows an approximately:

**28× cost gap**

between local electricity and renting comparable cloud compute.

---

## 2. Tokens per watt are surprisingly similar

The three platforms that produce a sustained inference rate fall into a relatively narrow efficiency range:

- RTX 4050: **3.86 tok/W**
- Hailo-10H: **3.45 tok/W**
- iPhone 16 Pro: **~3.23 tok/W**

This suggests that raw power efficiency alone does not clearly separate these platforms.

The RTX 4050 consumes substantially more power than the NPU, but also produces substantially higher throughput, keeping the resulting tokens-per-watt figure in a similar range.

---

## 3. Thermal endurance matters

The Galaxy S24 Ultra is treated differently from the other platforms.

Rather than assigning it an artificial cost-per-token number, the analysis flags its thermal cutoff because it does not produce a valid sustained inference rate.

This leads to an important deployment insight:

> **A device that cannot maintain a sustained inference rate cannot be evaluated purely through instantaneous throughput or power efficiency.**

For always-on inference, thermal endurance is therefore an important hardware constraint.

---

## Deployment Framework

The analysis suggests that edge-versus-cloud decisions should not be based on watts alone.

A practical deployment decision should consider:

```text
                 Inference Deployment
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
        Local / Edge             Cloud
             │                       │
     ┌───────┴───────┐        ┌──────┴──────┐
     │               │        │             │
     ▼               ▼        ▼             ▼
  Hardware        Thermal   Rental       Recurring
   Cost            Limits    Cost          Cost
     │               │        │             │
     └───────────────┴────────┴─────────────┘
                         │
                         ▼
                 Deployment Choice# tinybench-edge-vs-cloud
Reframing TinyBench throughput and power data into cost-per-million-token, tokens-per-watt, and edge-vs-cloud deployment insights.
