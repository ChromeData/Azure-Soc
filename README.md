# Azure SOC and Honeynet

I built a small network of deliberately exposed machines (a honeynet) in Azure, let real attackers on the internet hit it for 24 hours, then locked it down and measured another 24 hours. Microsoft Sentinel collected the logs, drew attack maps and raised alerts.

![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Result

Hardening cut security incidents from **348 to 0** and malicious traffic reaching the network from **843 to 0**.

| What was measured | Before | After | Change |
|---|---|---|---|
| Windows security events | 19,470 | 8,778 | −55% |
| Linux auth logs (Syslog) | 3,028 | 25 | −99% |
| Alerts | 10 | 0 | −100% |
| Sentinel incidents | 348 | 0 | −100% |
| Malicious network flows allowed in | 843 | 0 | −100% |

Before: Mar 15–16, 2023. After: Mar 18–19, 2023. 24 hours each.

## What I built

- 1 virtual network with a network security group (NSG)
- 3 virtual machines: 2 Windows, 1 Linux
- Log Analytics workspace collecting all logs
- Key Vault and Storage Account
- Microsoft Sentinel for alerts, incidents and attack maps

## Before: wide open

Firewalls and NSGs were open to the whole internet, and every resource had a public endpoint.

![Architecture before](https://i.imgur.com/aBDwnKb.jpg)

Attack maps from the first 24 hours:

![NSG allowed inbound malicious flows](https://i.imgur.com/1qvswSX.png)
![Linux SSH auth failures](https://i.imgur.com/G1YgZt6.png)
![Windows RDP/SMB auth failures](https://i.imgur.com/ESr9Dlv.png)

## After: locked down

- NSGs block all traffic except my own admin computer.
- Key Vault and Storage use their built-in firewalls and private endpoints (no public access).

![Architecture after](https://i.imgur.com/YQNa9Pp.jpg)

After hardening, the attack map queries returned nothing: no malicious activity got through.

**Note:** the machines had no real users, so a busy network would still generate some events after hardening.

**Built with:** Microsoft Azure, Microsoft Sentinel, Log Analytics, KQL, NSGs, Private Endpoints
