# Telnet & SSH Remote Access Lab – Cisco Packet Tracer

A simple lab showing how to configure and test **Telnet** (insecure) and **SSH** (secure) remote management on a Cisco 2911 router using Cisco Packet Tracer.

## Topology

```
 PC0 (10.0.0.10/8) ──────────── Gi0/0 Router0 – 2911 (10.0.0.1/8)
```

| Device  | Interface | IP Address | Subnet Mask |
|---------|-----------|------------|-------------|
| PC0     | FastEthernet0 | 10.0.0.10 | 255.0.0.0 (/8) |
| Router0 | GigabitEthernet0/0 | 10.0.0.1 | 255.0.0.0 (/8) |

> Cable: Copper **Cross-over** (PC → Router).

## Objectives

- Configure IP addressing
- Enable Telnet access and test it
- Configure SSH (RSA keys, domain name, local user) and test it
- Harden the router by disabling Telnet

## Credentials used (lab only)

| Item | Value |
|------|-------|
| Username | `admin` |
| User password | `Admin@123` |
| Enable secret | `Cisco@123` |

## Steps

### 1. Build the topology
1. Add a **PC** and a **2911 router**.
2. Connect them with a **Copper Cross-Over** cable (PC FastEthernet0 ↔ Router Gi0/0).

### 2. Configure the PC
Desktop → **IP Configuration** → Static
- IP: `10.0.0.10`
- Mask: `255.0.0.0`
- Gateway: `10.0.0.1`

### 3. Configure the router
Open Router0 → **CLI** and paste the contents of [`Router0-config.txt`](Router0-config.txt) (Phase 1).

Verify:
```
Router0# show ip interface brief
Router0# show ip ssh
Router0# show running-config | section line vty
```

### 4. Test connectivity
On PC0 → Desktop → **Command Prompt**:
```
C:\> ping 10.0.0.1
```

### 5. Test Telnet
```
C:\> telnet 10.0.0.1
Username: admin
Password: Admin@123
Router0> enable
Password: Cisco@123
Router0#
```

### 6. Test SSH
```
C:\> ssh -l admin 10.0.0.1
Password: Admin@123
Router0> enable
Password: Cisco@123
Router0#
```

### 7. Harden – disable Telnet
```
Router0(config)# line vty 0 4
Router0(config-line)# transport input ssh
Router0(config-line)# end
Router0# write memory
```
Retest `telnet 10.0.0.1` → connection should be **refused**, while `ssh -l admin 10.0.0.1` still works.

## Useful verification commands

| Command | Purpose |
|---------|---------|
| `show ip ssh` | SSH version/status |
| `show ssh` | Active SSH sessions |
| `show users` | Who is logged in |
| `show running-config` | Full config |

## Telnet vs SSH

| Feature | Telnet | SSH |
|---------|--------|-----|
| Port | TCP 23 | TCP 22 |
| Encryption | None (clear text) | Encrypted |
| Security | Insecure | Secure |
| Recommended | No | Yes |

## Troubleshooting

- **SSH not working?** Ensure hostname and `ip domain-name` are set *before* `crypto key generate rsa`.
- **Login fails?** Confirm `login local` on the VTY lines and that the user exists.
- **Ping fails?** Check the cable type and that Gi0/0 is `up/up`.

## Files

| File | Description |
|------|-------------|
| `README.md` | Lab documentation |
| `Router0-config.txt` | Router configuration |
| `topology.pkt` | Packet Tracer file (add your saved `.pkt`) |
| `topology.png` | Topology screenshot (add yours) |

## Author

Abhishek1449019 – [GitHub profile](https://github.com/Abhishek1449019)
