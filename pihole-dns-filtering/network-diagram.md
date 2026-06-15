# Pi-hole Network Diagram

## Logical Topology

```mermaid
flowchart TD
    Internet[Internet] --> Router[Router / Firewall<br/>192.168.10.1]
    Router --> Switch[LAN / Switch]
    Switch --> PiHole[Pi-hole<br/>192.168.10.10<br/>DNS: TCP/UDP 53]
    Switch --> Admin[Admin Workstation<br/>192.168.10.20]
    Switch --> Client1[Test Client<br/>192.168.10.50]
    Switch --> Client2[Additional LAN Clients]
    Client1 -->|DNS query| PiHole
    Client2 -->|DNS query| PiHole
    PiHole -->|Allowed upstream query| Upstream[Upstream DNS Resolver]
    Admin -->|SSH / Web administration| PiHole
```

## DNS Resolution Flow

```mermaid
sequenceDiagram
    participant C as Client
    participant P as Pi-hole
    participant U as Upstream DNS
    C->>P: Query example.com
    alt Domain allowed and cached
        P-->>C: Cached DNS response
    else Domain allowed and not cached
        P->>U: Forward DNS query
        U-->>P: DNS response
        P-->>C: DNS response
    else Domain blocked by policy
        P-->>C: Blocking response
    end
```

## Example Addressing Table

| Device | Hostname | Address | Role |
|---|---|---:|---|
| Router/firewall | `gateway01` | `192.168.10.1` | Default gateway and DHCP |
| Pi-hole | `pihole01` | `192.168.10.10` | DNS filtering |
| Admin workstation | `admin-pc` | `192.168.10.20` | Administration |
| Test client | `test-laptop` | `192.168.10.50` | Validation |
| DHCP pool | N/A | `192.168.10.100-199` | Dynamic clients |

## Traffic Requirements

| Source | Destination | Protocol/Port | Purpose |
|---|---|---|---|
| LAN clients | Pi-hole | UDP/TCP 53 | DNS |
| Admin workstation | Pi-hole | TCP 22 | SSH administration |
| Admin workstation | Pi-hole | TCP 80/443 | Web administration |
| Pi-hole | Upstream resolver | UDP/TCP 53 or configured secure method | Forwarded DNS |
| Pi-hole | Package repositories | TCP 80/443 | Updates |

## Design Considerations

- Use a DHCP reservation or documented static address for Pi-hole.
- Prevent accidental exposure of port 53 to the public internet.
- Restrict administration to trusted networks.
- Consider a second internal resolver if DNS availability is critical.
- Do not configure a public secondary DNS on clients if the goal is consistent filtering; clients may bypass Pi-hole.
- Test VPN and guest-network behavior separately.
- Document IPv6 behavior instead of assuming IPv4-only settings cover all DNS traffic.

## Evidence Placeholder

Add a sanitized exported diagram to:

```text
../assets/diagrams/pihole-network-topology.svg
```

Do not include public IP addresses, Wi-Fi passwords, serial numbers, or client names.
