# Azure Hit by Record 15.72 Tbps DDoS Attack Powered by Aisuru Botnet (2025)
**Date**: November 19, 2025
**Source**: SecurityWeek
**Author**: Eduard Kovacs

## Summary

Microsoft announced that Azure recently mitigated one of its largest-ever DDoS attacks, peaking at 15.72 Tbps and 3.64 Bpps, aimed at a single endpoint in Australia.
The attack was launched on October 24 and was powered by the Aisuru botnet, a TurboMirai-class IoT botnet built from compromised routers, CCTV cameras, and DVRs.

Although massive, this was not the largest global DDoS attack — Cloudflare previously observed a 22.2 Tbps attack also driven by Aisuru.

## Technical Details

**Botnet Used**: Aisuru (TurboMirai-class IoT botnet)

**Attack Type**: High-rate UDP floods

**Characteristics**:
- 500,000 source IPs
- Minimal IP spoofing
- Randomized source ports

**Target**: Single Azure public IP (Australia region)

**Impact**:
- Attempted service disruption
- Azure DDoS protection mitigated attack successfully
- Microsoft notes that Aisuru is also used for credential stuffing, web scraping, phishing, and spamming.
- Netscout reports TurboMirai variants cannot spoof traffic, allowing easier traceback and device remediation.

## MITRE ATT&CK Mapping (Relevant Techniques)
Phase	Technique	ID
Initial Access	Exploitation of vulnerable IoT devices	T1190
Command & Control	Botnet communication	T1095
Impact	Network Denial-of-Service	T1498.001 (Direct Network Flood)

## My Key Takeaways

- Large-scale DDoS attacks are increasingly IoT-driven, with Aisuru emerging as one of the most aggressive botnets.
- Minimal spoofing in TurboMirai-class botnets makes traceback easier, which is useful for ISPs and defenders.
- Cloud providers must constantly evolve DDoS mitigation capabilities as attack throughput continues to break previous records.
- Highlights the need for global IoT security enforcement, since consumer devices continue to fuel massive attacks.

## Why This Incident Matters
This event shows how easily attackers can coordinate high-bandwidth, multi-vector attacks using compromised consumer IoT devices.
For SOC & Blue Team roles, it reinforces the importance of:
- DDoS monitoring & mitigation
- Understanding botnet behaviors
- Traffic anomaly detection
- Log correlation during high-volume attacks
