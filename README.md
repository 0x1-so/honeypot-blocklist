# Honeypot Blocklist

**English** · [中文](README.zh-CN.md)

A list of IP addresses that actually attacked our two honeypot sensors, **updated every hour** and limited to the **last 30 days**.

- Web view: https://0x1-so.github.io/honeypot-blocklist/
- Live attack map: https://map.hsd0x1.cc

## Files

| File | Contents |
|---|---|
| [`blocklist.txt`](blocklist.txt) | **Attack sources**: datacenters, VPS and proxies, mostly machines the attackers control themselves. Safe to block. |
| [`infected-devices.txt`](infected-devices.txt) | **Likely infected devices**: residential or business lines showing Mirai-style IoT malware behaviour (see below). The owners usually don't know; consider a short block or alerting only. |
| [`all.txt`](all.txt) | Both lists combined. |
| [`attackers.csv`](attackers.csv) | Per address: first and last seen, activity, protocols, country, ASN, event count, SHA-256 of captured samples. |
| [`samples-sha256.txt`](samples-sha256.txt) | SHA-256 of files attackers downloaded or wrote on the honeypots (stored, never executed). |
| [`data.json`](data.json) | Data for the web view, including the update time. |

The `.txt` files hold one IP per line; lines starting with `#` are comments. They can be fed directly to ipset, fail2ban, firewalls and so on.

## What gets listed

Only addresses that **actually attacked**:

- SSH / Telnet login attempts (brute force)
- Commands run after logging in
- Malicious files downloaded or uploaded
- Credential stuffing against fake databases, caches and similar services (a password must be sent; an address whose only activity is this needs at least 3 attempts. Protocols like remote desktop include a user name in the bare connection request, so a scanner merely connecting doesn't count)
- Attempts to open port-forwarding tunnels

Addresses that **only scan ports or probe web paths are not listed**. Research scanners such as Shodan and Censys do that all day, and listing them would only cause false positives.

## How the two lists are split

An address goes into `infected-devices.txt` when **all** of the following hold, otherwise into `blocklist.txt`:

1. It behaves like IoT malware: it uses Telnet or runs commands like `busybox` (the typical Mirai approach);
2. Its network is not a datacenter or cloud provider;
3. It is not a proxy, VPN or Tor exit.

This is an **inference** from network type and behaviour, not a certainty.

## Expiry and removal

- An address that hasn't been seen for 30 days drops off the lists automatically.
- Residential IPs change hands and devices get cleaned up. If you think an address is listed by mistake (for example it is yours and has been cleaned), please [open an issue](../../issues).

## Disclaimer

Provided as-is, without warranty. The sensors only record passively and never connect back to, scan or attack any source.
