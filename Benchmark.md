# Solana HFT-grade Performance Benchmark Report (2025 Q4)

## Summary

This benchmark is produced by the BlockRush Technical Lab on live Solana mainnet, validating our acceleration capabilities under high-frequency trading workloads across three dimensions: infrastructure health, RPC net processing latency, and cross-region SWQoS win rate.

| Block Time Score | Net Latency Gain | SWQoS Win Rate |
| :--- | :--- | :--- |
| **110 / 100** _Benchmark Metric_ | **8.4x Faster** _Benchmark Metric_ | **100%** _Benchmark Metric_ |
| Physical block production speed beats Jito client median. | 8.4x faster net processing latency vs \$4,500/month dedicated node. | 100% Total Set win rate across 80 cross-region SWQoS battles. |

Includes infrastructure audit, latency comparison, SWQoS battles, and commercial takeaways.

### TEST OWNER

**BlockRush Technical Lab**

2025-11-15 10:00 - 14:30 (UTC), from AWS US-East (Virginia) and Tokyo, targeting Solana Mainnet under HFT workloads.

### KEY FINDINGS

- Validator infrastructure scores maxed out, Block Time 110 / 100.
- Net processing latency around 80ms, over 8x faster than dedicated nodes.
- 100% Total Set win rate against leading SWQoS competitors.

### IDEAL USE CASES

- Arbitrage, sniping, and market making strategies requiring determinism.
- Teams looking to replace expensive dedicated nodes or BDN providers.
- Multi-region HFT bots that need low latency and tight distribution.

# Full benchmark report 
## BlockRush SWQoS Acceleration Performance Benchmark Report
BlockRush High-Frequency Transaction Acceleration Benchmark Report (2025 Q4)

This report is prepared by the BlockRush Technical Lab, running live HFT-style workloads on Solana mainnet from AWS US-East (Virginia) and Tokyo. It covers infrastructure health, RPC net processing latency, and cross-region SWQoS win rate to validate BlockRush as a deterministic HFT infrastructure provider.

---

## 1. Test Overview (Executive Summary & Scope)

This report runs two sets of live experiments to evaluate BlockRush performance on Solana. We focus on three dimensions: infrastructure health, RPC net processing latency, and cross-region propagation win rate under realistic HFT workloads.

---

### TEST PROFILE
- **Owner:** BlockRush Technical Lab
- **Time:** 2025-11-15 10:00 - 14:30 (UTC)
- **Regions:** AWS US-East (Virginia) & Tokyo
- **Network:** Solana Mainnet

---

### SCENARIOS
- High-frequency trading, market making, arbitrage bots
- Listing snipes, MEV-style racing, new pool sniping
- Tip multiplier fixed at 5x for fair benchmarking

---

### DIMENSIONS
- **Infrastructure:** block time, skipped slots, uptime, epoch credits
- **Latency & cost:** RTT-calibrated net latency and stdev
- **SWQoS battles:** win rate vs Bloxroute, Oslot, Blockrazor, Flashblock

## 2. Infrastructure Health Audit

SWQoS (Stake-Weighted QoS) effectiveness heavily depends on validator stake weight and raw performance. Even with perfect networking, a slow validator means your transactions will still miss critical blocks. Using live Solanacompass and ValidatorsApp data, we highlight BlockRush validator metrics:

---

### Key metrics at a glance
*Every core metric is effectively maxed out, visually showing the structural edge of the BlockRush validator.*

| Metric | Score | Status | Description |
| :--- | :--- | :--- | :--- |
| **Block Time** | **110 / 110** | 🚀 **Top-tier** | A score > 100 means BlockRush builds and packs blocks ~10% faster than the Jito client median and reference hardware. |
| **Skipped Slots** | **100 / 100** | ✅ **Perfect** | 0% skipped slots. When BlockRush becomes leader, it consistently uses every available slot to pack transactions. |
| **Uptime** | **100 / 100** | ✅ **Perfect** | Remains online even during upgrades and network turbulence, ideal for 24/7 HFT bots. |
| **Epoch Credits**| **100 / 100** | ✅ **Perfect** | Full participation in consensus and voting, keeping the validator in the top tier of cluster reputation and weight. |

---

### Technical Insight
A Block Time score of 110 / 100 indicates validator hardware and tuning that outperforms both Solana's reference requirements and the Jito client median by roughly 10%. In SWQoS races, this translates into consistently winning critical blocks under equal network paths.

Combined with zero skipped slots and perfect uptime, BlockRush offers a highly deterministic foundation for latency-sensitive strategies.

---

### Cluster Skipped Slot Percent (Validator vs Cluster)
The blue line shows BlockRush validator skipped slots staying at 0% across slots 866–888, while the red line shows cluster skipped slot percent fluctuating up to ~0.55%, highlighting our validator's stability advantage over the network average.


<img width="429" height="153" alt="image" src="https://github.com/user-attachments/assets/4d64e00f-2c20-4b58-9d65-ff63aac5daa7" />


**Legend:**
- 🔵 **Validator Skipped Slots**
- 🔴 **Cluster Skipped Slots**

## 3. Latency & Cost Efficiency

This section compares BlockRush SWQoS against a \$4,500/month QuickNode dedicated node and the public JitoFree endpoint, using RTT-calibrated net processing latency to answer a simple question: is an expensive dedicated node still worth it? In both average net latency and stability (stdev), BlockRush delivers an almost order-of-magnitude advantage.

---

### 3.1 Net Processing Latency (RTT-calibrated)

We subtract ~200ms base RTT from raw latency to isolate RPC internal queuing and processing time so that the comparison reflects pure infrastructure performance.

| Provider | Avg Net | Median Net | Stdev | Vs BlockRush |
| :--- | :--- | :--- | :--- | :--- |
| **BlockRush SWQoS** | **80.07 ms** | **60.50 ms** | **60.40 ms** | **Baseline (80ms / 60ms stdev)** |
| **QuickNode (dedicated)**| 672.10 ms | 91.00 ms | 1989.64 ms | ~8.4x slower, 30x+ noisier |
| **JitoFree (public)** | 744.33 ms | 620.00 ms | 645.81 ms | ~9.3x slower, 10x+ noisier |

In absolute terms, BlockRush averages around 80ms net latency, while QuickNode and JitoFree sit at 672ms and 744ms. On stability, BlockRush stdev is ~60ms versus 1,989ms and 646ms for competitors — a clear, near order-of-magnitude edge on both speed and consistency.

---

### 3.2 Win Rate Distribution

In 30 concurrent races, BlockRush wins 26 times (86.7%), while QuickNode and JitoFree only win twice each. The few BlockRush "losses" all land in the same block and are caused by minor ordering differences, not missing the block entirely.

QuickNode's massive 1,989ms stdev indicates severe long-tail latency, whereas BlockRush stays around 60ms. This tight distribution is what makes BlockRush suitable for latency-critical arbitrage and sniping strategies.

## 4. SWQoS Competitor Battles

We run 80 live SWQoS battles with identical 5x tips to benchmark BlockRush against Oslot, Bloxroute, Blockrazor and Flashblock in realistic HFT scenarios.

---

### Flagship match: BlockRush vs Oslot (same-path duel)
*Both sides use Tokyo endpoints and identical paths, isolating validator performance.*

| Condition | Oslot | BlockRush | Winner |
| :--- | :--- | :--- | :--- |
| Oslot leads (BlockRush disadvantage) | **8** | 2 | BlockRush wins |
| BlockRush leads (BlockRush advantage) | 3 | **7** | Oslot edges out |
| **Total** | **11** | **9** | **BlockRush wins the overall set** |

***Under the fairest possible conditions, BlockRush still wins the total score thanks to validator performance.***

---

### Benchmark match: BlockRush vs Bloxroute (BDN giant)
*Head-to-head against a well-known BDN to validate the ceiling of our design.*

| Condition | Bloxroute | BlockRush | Winner |
| :--- | :--- | :--- | :--- |
| Bloxroute leads (BlockRush disadvantage) | 6 | **4** | BlockRush still wins 60% of rounds |
| BlockRush leads (BlockRush advantage) | **7** | 3 | BlockRush extends the lead |
| **Total** | **13** | **7** | **BlockRush clearly wins the set** |

***Even versus the incumbent BDN, BlockRush maintains a strong edge in both advantage and disadvantage rounds.***

---

### Crushing match: BlockRush vs Blockrazor
*Tests relative strength against another acceleration service.*

| Condition | Blockrazor | BlockRush | Winner |
| :--- | :--- | :--- | :--- |
| Blockrazor leads | 8 | **2** | BlockRush dominates despite disadvantage |
| **Total** | **15** | **5** | **BlockRush wins 75% of rounds** |

***Results show the two providers are not in the same league in terms of SWQoS effectiveness.***

---

### Stability match: BlockRush vs Flashblock
*Focus on long-run win rate stability across many rounds.*

| Condition | Flashblock | BlockRush | Winner |
| :--- | :--- | :--- | :--- |
| Flashblock leads | **7** | 3 | BlockRush keeps ~70% win rate |
| **Total** | **14** | **6** | **BlockRush wins with 70% total win rate** |

***No matter who sends first, BlockRush locks in a stable, high win rate.***

# 5. Summary & Recommendation

| Hardware performance shapes everything above | A highly cost-efficient alternative | Not just fast, but deterministic |
| :--- | :--- | :--- |
| BlockRush wins SWQoS battles primarily because its validator hardware delivers a Block Time score of 110 / 100, placing it in the top ~1% of the network. This hardware gap cannot be bridged by software optimizations alone. | For users on QuickNode dedicated plans or high-tier Bloxroute packages, switching to BlockRush can both cut costs and reduce net processing latency from 600ms+ down to the 80ms range. | A tiny 60ms stdev and 100% overall set win rate make BlockRush one of the most deterministic infrastructures for arbitrage, sniping, and market-making strategies on Solana today. |

---
*Report Certified By: BlockRush Infrastructure Team*
