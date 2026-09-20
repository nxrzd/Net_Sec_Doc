# Network Security & Monitoring

## Overview
The security and observability layer sitting in front of and alongside the homelab services.

## Perimeter / firewall
- Open-source firewall distribution (OPNsense) replacing the ISP-provided router, running as the network's default gateway.
- Internal LAN on a private, non-routable subnet (exact range redacted).
- Outbound access from lab services to the public internet is proxied through tunnel/reverse-proxy services rather than direct port-forwarding wherever possible (see below).

## SIEM / detection
- Wazuh deployed as the SIEM, with agents on multiple internal hosts and custom detection rules layered on top of the defaults.
- Agent count and exact rule content intentionally not documented here.

## Metrics / logs
- Grafana + Prometheus + Loki stack for metrics visualization and log aggregation.
- A dedicated network-monitoring host running `ntopng`-style traffic analysis sits on its own VM.

## Remote access / exposure model
- No services are exposed to the internet via direct inbound port-forwarding.
- Public-facing services reach the internet through a combination of:
  - An outbound tunneling agent (Cloudflare Tunnel–style) for select HTTP(S) services.
  - A lightweight reverse tunnel (rathole-style) plus a small always-on cloud VM used as a public-facing relay/reverse proxy.
  - A WireGuard VPN tunnel back into the lab for administrative access, used in preference to exposing admin panels (e.g. the hypervisor's own web UI) directly.
- An identity-aware reverse proxy (see `authentik_doc.md`) sits in front of any admin-facing endpoint that is reachable at all from outside the LAN.

## DNS / ad-and-tracker blocking
- Pi-hole + Unbound running on a low-power single-board computer, also handling internal `.lab`-style local DNS resolution for lab hosts.
- A lightweight reverse proxy on the same device fronts a couple of internal-only web UIs.

## Hardening backlog (representative, not exhaustive)
- Rate-limiting / brute-force protection (fail2ban-style) on the internet-facing relay VM.
- Periodic cleanup of firewall/NAT rules on the same relay VM.
- Ongoing move of any remaining admin-plane access fully behind the VPN rather than any public path.

## Redacted / intentionally omitted
- Real public IPs, domains, and cloud provider account/region details.
- Specific firewall/NAT rule sets and tunnel configuration files.
- Exact SIEM rule logic and alert thresholds.
