# Static Routing Lab – Cisco Packet Tracer

A lab showing how to connect two remote networks across three routers using **static routes** in Cisco Packet Tracer.

## Topology

```
PC0 ─────── R1 ─────── R2 ─────── R3 ─────── PC1
10.0.0.10   .1   .1   .2   .1   .2   .1     40.0.0.10
      10.0.0.0/8  20.0.0.0/8  30.0.0.0/8  40.0.0.0/8
```

- R1 – R2 : `20.0.0.0/8`
- R2 – R3 : `30.0.0.0/8`

## Addressing Table

| Device | Interface | IP Address | Mask | Gateway |
|--------|-----------|------------|------|---------|
| PC0 | NIC | 10.0.0.10 | 255.0.0.0 | 10.0.0.1 |
| R1 | Gi0/0 | 10.0.0.1 | 255.0.0.0 | – |
| R1 | Gi0/1 | 20.0.0.1 | 255.0.0.0 | – |
| R2 | Gi0/0 | 20.0.0.2 | 255.0.0.0 | – |
| R2 | Gi0/1 | 30.0.0.1 | 255.0.0.0 | – |
| R3 | Gi0/0 | 30.0.0.2 | 255.0.0.0 | – |
| R3 | Gi0/1 | 40.0.0.1 | 255.0.0.0 | – |
| PC1 | NIC | 40.0.0.10 | 255.0.0.0 | 40.0.0.1 |

> Cables: PC ↔ Router = Copper **Cross-over**; Router ↔ Router = Copper **Cross-over**.

## Static Routes

| Router | Destination | Mask | Next Hop |
|--------|-------------|------|----------|
| R1 | 30.0.0.0 | 255.0.0.0 | 20.0.0.2 |
| R1 | 40.0.0.0 | 255.0.0.0 | 20.0.0.2 |
| R2 | 10.0.0.0 | 255.0.0.0 | 20.0.0.1 |
| R2 | 40.0.0.0 | 255.0.0.0 | 30.0.0.2 |
| R3 | 10.0.0.0 | 255.0.0.0 | 30.0.0.1 |
| R3 | 20.0.0.0 | 255.0.0.0 | 30.0.0.1 |

Each router already knows its directly connected networks, so it only needs static routes for the networks that are **not** directly connected.

## Steps

1. Build the topology: 3 × 2911 routers and 2 PCs, connected as shown.
2. Configure PC0 and PC1 with the IP, mask and gateway from the table (Desktop → IP Configuration).
3. Open each router's **CLI** and paste its config:
   - [`R1-config.txt`](R1-config.txt)
   - [`R2-config.txt`](R2-config.txt)
   - [`R3-config.txt`](R3-config.txt)
4. Verify routing tables and connectivity (below).

## Verification

On each router:
```
show ip interface brief
show ip route
show running-config | include ip route
```
Static routes appear with code `S`, for example:
```
S    40.0.0.0/8 [1/0] via 20.0.0.2
```

From PC0 Command Prompt:
```
ping 40.0.0.10
tracert 40.0.0.10
```
Expected tracert path: `10.0.0.1 → 20.0.0.2 → 30.0.0.2 → 40.0.0.10`

From PC1 Command Prompt:
```
ping 10.0.0.10
```

## Troubleshooting

- **First ping times out:** Normal (ARP). Run it again.
- **Ping fails end to end:** Check `show ip route` on every router. Return routes are needed too, not just forward routes.
- **Interface down:** Confirm `no shutdown` and the correct cable type.
- **Wrong gateway:** PC gateway must be the router interface in its own network.

## Files

| File | Description |
|------|-------------|
| `README.md` | Lab documentation |
| `R1-config.txt` | R1 configuration |
| `R2-config.txt` | R2 configuration |
| `R3-config.txt` | R3 configuration |
| `static-route-lab.pkt` | Packet Tracer file (add your saved `.pkt`) |
| `topology.png` | Topology screenshot (add yours) |

## Author

Abhishek1449019 – [GitHub profile](https://github.com/Abhishek1449019)
