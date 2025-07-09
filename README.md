# port-135-hardening-report
Blocking Windows RPC Port 135 using Firewall

# Port 135 Hardening Report

## Summary: Why Port 135 Matters in Windows Hardening

Port **135/tcp** is used by the **Remote Procedure Call (RPC) Endpoint Mapper**, a core Windows service that supports technologies like **WMI**, **DCOM**, and **COM+ applications**. While it plays an essential role in inter-process and remote communication, it is also a **commonly targeted port** by malware, ransomware, and lateral movement attacks within networks.

In modern security hardening practices, **restricting or blocking external access to port 135** significantly reduces a system’s attack surface — especially on standalone or non-domain-joined machines.

This report documents how I implemented **firewall-based restrictions** to block unauthorized access to port 135, tested the configuration using **Nmap**, and explained the **security implications** of leaving it exposed in public or internal networks.

---

## Initial Scan (Before Blocking)

```bash
nmap -Pn -p 135 172.20.10.12

## Result:

135/tcp open  msrpc

 Firewall Configuration Steps
Open wf.msc → Inbound Rules → New Rule

Select Port, then TCP and enter 135

Choose Block the connection

Apply to Domain, Private, and Public

On the Scope tab:

Remote IP: 0.0.0.0/0 (block all external traffic)

Name the rule: Block Port 135 - RPC

Rebooted system to enforce the rule.

Verification Scan (After Blocking)

nmap -Pn -p 135 172.20.10.12

Expected Result:

135/tcp filtered

| Benefit                  | Description                                          |
| ------------------------ | ---------------------------------------------------- |
| ✅ Reduced Attack Surface | Removed a common malware entry point                 |
| ✅ RPC Access Restricted  | Prevented external scanning and DCOM-related attacks |
| ✅ Windows Hardening      | Safer system in public or untrusted networks         |

Notes
Port 135 is still used internally by Windows

Firewall blocks limit external access only

Disabling the service entirely may break Windows features
