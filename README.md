# Earwig — Softish C6 / EarVision Vulnerability Disclosure

Four coordinated CVEs in the Softish C6 ear camera (also sold as "Jegoat"
and under other white-label names) and its companion Android app, EarVision
(`com.atomath.wifi_camera` v1.3.1). Coordinated through CISA. Advisory pending publication — this doc will be
updated with the CSAF advisory ID once it goes live.

- **Full write-up:** [`earvision.md`](earvision.md)
- **Formal paper (PDF):** [`Inside-the-Ear-Canal.pdf`](Inside-the-Ear-Canal.pdf)
- **Web version:** https://mdubbrin.github.io/research/softish-c6-earvision/
- **CISA advisory:** pending publication

## Findings

| CVE | Finding | CWE | CVSS 3.1 |
|---|---|---|---|
| [CVE-2026-81330](CVE-2026-81330/) | Cleartext live video over UDP | CWE-319 | 6.5 Medium |
| [CVE-2026-81640](CVE-2026-81640/) | AP password derivable from broadcast BSSID | CWE-798 | 8.8 High |
| [CVE-2026-82563](CVE-2026-82563/) | Device identity accepted from broadcast values, not cryptography | CWE-290 | 7.6 High |
| [CVE-2026-77974](CVE-2026-77974/) | Unauthenticated, unsigned OTA firmware transfer | CWE-306 | 8.0 High |

Scores and CWEs above are the official values from the published CVE
records; none of the four reach the Critical band (9.0+) under standard
CVSS v3.1 thresholds. CVE-2026-81640 carries the highest score of the four
despite reading as the "quieter" finding in the write-up's narrative.

Individually, each is a common IoT defect. Chained (81640 → 81330 / 82563 →
77974), they let anyone within radio range of the device passively recover
the Wi-Fi password, decrypt the video feed, impersonate the device to its
own app, and — with one user tap — push it unsigned firmware. See the
write-up for the full attack-chain analysis.

## Affected product

| | |
|---|---|
| Device | Softish C6 ear camera (Beken BK7252N / TXW806 / TXW810 variants) |
| App | EarVision, `com.atomath.wifi_camera` v1.3.1 (versionCode 132) |
| Vendor | Shenzhen Jiding Electronics Co., Ltd. |
| Vendor security contact | None identified — no `security.txt`, no published disclosure policy |

## Repository layout

Each `CVE-2026-*/` folder holds that finding's non-exploit supporting
evidence (protocol documentation, decoded manifests, redacted example
payloads, screenshots) and its own `README.md`. Raw wireless captures,
reconstructed video, firmware images, and any working proof-of-concept code
are **not** in this repository — see [Exploit-Code Policy](earvision.md#exploit-code-policy)
in the write-up for why, and each folder's `README.md` for what specifically
was withheld from it.

## Disclosure timeline

| Date | Event |
|---|---|
| May 2026 | Initial discovery |
| 2026 | Reported to CISA; advisory pending publication |
| 2 Sep 2026 | CVE-2026-81330, -81640, -82563, -77974 published |
| 16 Sep 2026 | Research paper published |

## License

[MIT](LICENSE) for the contents of this repository. This is a security
research disclosure, not a usable software product.
