# Earwig — Softish C6 / EarVision Vulnerability Disclosure

Four coordinated CVEs in the Softish C6 ear camera (also sold as "Jegoat"
and under other white-label names) and its companion Android app, EarVision
(`com.atomath.wifi_camera` v1.3.1). Coordinated through CERT/CC as
**VU#763563**.

- **Full write-up:** [`earvision.md`](earvision.md)
- **Formal paper (PDF):** [`paper/Inside-the-Ear-Canal.pdf`](paper/Inside-the-Ear-Canal.pdf)
- **Web version:** https://mdubbrin.github.io/research/softish-c6-earvision/
- **CERT/CC advisory:** https://kb.cert.org/vuls/id/763563

## Findings

| CVE | Finding | CWE | CVSS (est.) |
|---|---|---|---|
| [CVE-2026-81330](CVE-2026-81330/) | Cleartext live video over UDP | CWE-319 | 7.5 Critical |
| [CVE-2026-81640](CVE-2026-81640/) | AP password derivable from broadcast BSSID | CWE-330, CWE-798 | 6.5 High |
| [CVE-2026-82563](CVE-2026-82563/) | Device identity accepted from broadcast values, not cryptography | CWE-290 | 8.8 Critical |
| [CVE-2026-77974](CVE-2026-77974/) | Unauthenticated, unsigned OTA firmware transfer | CWE-306, CWE-494 | 7.6 Critical |

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
| 2026 | Reported to CERT/CC; case opened as VU#763563 |
| 2 Sep 2026 | CVE-2026-81330, -81640, -82563, -77974 published |
| 16 Sep 2026 | Research paper published |

## License

[MIT](LICENSE) for the contents of this repository. This is a security
research disclosure, not a usable software product.
