# Mercusys AC12G (EU) V1 - Security Vulnerabilities

15 CVEs in the Mercusys AC12G (EU) V1 wireless router.

**Researcher:** Tymbark7372  
**Vendor:** Mercusys (sub-brand of TP-Link Technologies Co., Ltd.)  
**Reported:** February 28, 2026  
**CVEs Assigned:** May 2026

## Affected Product

| Field | Value |
|-------|-------|
| Vendor | Mercusys (TP-Link sub-brand) |
| Model | AC12G (EU) V1 |
| Product Name | AC1200 Wireless Dual Band Gigabit Router |
| Tested Firmware | AC12G(EU)_V1_200909, AC12G(EU)_V1_210128 |
| OS | VxWorks RTOS (TP-Link PNE2.2 platform) |
| SoC | MediaTek MT7620DA + MT7612EN |
| Status | End-of-life (no fix planned) |

## Vulnerabilities

| CVE | Title | Severity | CVSS 3.1 | CWE |
|-----|-------|----------|-----------|-----|
| [CVE-2026-36607](advisories/CVE-2026-36607.md) | Authentication Rate Limit Bypass via TDDP Password Change Endpoint | **Critical** | 9.8 | CWE-307 |
| [CVE-2026-36608](advisories/CVE-2026-36608.md) | UPnP Self-Mapping Exposes Admin Panel to Internet | **Critical** | 9.6 | CWE-441 |
| [CVE-2026-36603](advisories/CVE-2026-36603.md) | Unauthenticated UPnP IGD Actions Including Port Forwarding | **High** | 8.1 | CWE-306 |
| [CVE-2026-36605](advisories/CVE-2026-36605.md) | Persistent HTTP Denial of Service via Slow Requests | **High** | 7.5 | CWE-400 |
| [CVE-2026-36609](advisories/CVE-2026-36609.md) | Static Authentication Nonce Enables Password Recovery | **High** | 7.5 | CWE-330 |
| [CVE-2026-36606](advisories/CVE-2026-36606.md) | Hardcoded DES Key for Configuration Backup Encryption | **High** | 7.4 | CWE-321 |
| [CVE-2026-36604](advisories/CVE-2026-36604.md) | DNS Rebinding via Missing Host Header Validation | Medium | 6.5 | CWE-350 |
| [CVE-2026-36612](advisories/CVE-2026-36612.md) | WPS 2.0 Enabled by Default with Weak Lockout Policy | Medium | 6.5 | CWE-307 |
| [CVE-2026-36616](advisories/CVE-2026-36616.md) | Hardcoded WiFi Driver Credentials in Production Firmware | Medium | 6.1 | CWE-798 |
| [CVE-2026-36610](advisories/CVE-2026-36610.md) | Plaintext DDNS Credential Transmission | Medium | 5.9 | CWE-319 |
| [CVE-2026-36602](advisories/CVE-2026-36602.md) | UPnP GetStatusInfo Kernel Memory Pointer Disclosure | Medium | 5.3 | CWE-200 |
| [CVE-2026-36611](advisories/CVE-2026-36611.md) | UPnP Port 1900 Uninitialized Buffer Disclosure | Medium | 5.3 | CWE-200 |
| [CVE-2026-36613](advisories/CVE-2026-36613.md) | HTTP POST Uninitialized Buffer Disclosure on Undefined Paths | Medium | 5.3 | CWE-200 |
| [CVE-2026-36615](advisories/CVE-2026-36615.md) | Undocumented /agileconfigreset Endpoint Buffer Leak | Medium | 4.3 | CWE-200 |
| [CVE-2026-36618](advisories/CVE-2026-36618.md) | DNS Resolver Version Disclosure | Low | 3.7 | CWE-200 |

## Attack Chains

### Chain 1: Complete Remote Router Compromise via Browser

An attacker can gain full admin access to the router from the internet by luring any LAN user to a malicious webpage:

1. **CVE-2026-36604** - DNS rebinding + CORS wildcard allows cross-origin API access from any website
2. **CVE-2026-36607** - Password change endpoint (code=10) has no rate limiting, enabling unlimited brute-force
3. **CVE-2026-36609** - Static authentication nonce makes password encoding reversible

### Chain 2: Internet-Exposed Admin Panel via UPnP

Any compromised LAN device (IoT malware, browser exploit) can expose the router's admin interface to the internet:

1. **CVE-2026-36603** - 15 of 18 UPnP actions are unauthenticated, UPnP is enabled by default
2. **CVE-2026-36608** - AddPortMapping accepts the router's own IP as InternalClient
3. Admin panel (port 80) forwarded to a WAN port, accessible from the internet
4. **CVE-2026-36607** - Brute-force from the internet (no rate limiting)

### Credential Exposure (Independent Vectors)

- **CVE-2026-36606** - Configuration backup encrypted with a hardcoded DES key; decryption recovers admin password, WiFi PSK, PPPoE credentials, DDNS credentials
- **CVE-2026-36610** - DDNS credentials separately exposed via plaintext HTTP transmission (no TLS in firmware)

## Methodology

- Firmware dumped from device via UART serial console and analyzed (MIPS32 LE, ~7,000 symbols)
- Live network testing against researcher-owned device
- Authentication algorithm source code (`/lib/Quary.js`) served without authentication

## Disclosure Timeline

| Date | Event |
|------|-------|
| 2026-02-28 | Vulnerabilities reported to TP-Link, CVEs requested from MITRE |
| 2026-03-03 | Full report sent to Mercusys security team |
| 2026-03-19 | Mercusys acknowledged report |
| 2026-05-28 | 15 CVEs assigned by MITRE |
| 2026-06-01 | Public disclosure |

## Notes

- Mercusys acknowledged the report, confirmed internal review by their R&D and security teams, and is inspecting other devices in their product line. The AC12G is EOL and no fix will be released.

## Researcher

**Tymbark7372** - Independent Security Researcher  
GitHub: https://github.com/Tymbark7372/MERCUSYS-AC12G
