# Network Topology

## Overview

This document describes the network topology and hardware configuration of the system.

## Network Architecture

### Physical Layout

```mermaid
graph TD
    A[Internet] --> B[Load Balancer]
    B --> C[Web Server 1]
    B --> D[Web Server 2]
    C --> E[Application Server 1]
    D --> F[Application Server 2]
    E --> G[(Database Primary)]
    F --> H[(Database Replica)]
    G --> H
```

### Network Segments

```mermaid
graph LR
    subgraph "DMZ"
        A[Load Balancer]
    end
    
    subgraph "Web Tier"
        B[Web Server 1]
        C[Web Server 2]
    end
    
    subgraph "Application Tier"
        D[App Server 1]
        E[App Server 2]
    end
    
    subgraph "Database Tier"
        F[(Primary DB)]
        G[(Replica DB)]
    end
    
    A --> B
    A --> C
    B --> D
    C --> E
    D --> F
    E --> G
```

## Hardware Specifications

### Server Configuration

| Component | Specification | Quantity |
|-----------|---------------|----------|
| CPU | Intel Xeon E5-2680 v4 | 2 per server |
| RAM | 64GB DDR4 | 4x16GB modules |
| Storage | 2TB SSD | 2 drives (RAID 1) |
| Network | 10GbE | 2 ports |

### Network Equipment

- **Load Balancer**: F5 BIG-IP 3900
- **Switches**: Cisco Catalyst 9300
- **Firewall**: Palo Alto PA-5220
- **Routers**: Cisco ISR 4321

## Security Zones

1. **DMZ**: Public-facing services
2. **Web Tier**: Web servers
3. **Application Tier**: Business logic servers
4. **Database Tier**: Data storage (restricted access)

## Monitoring Points

- Network traffic monitoring at each tier
- Server health monitoring
- Database performance metrics
- Security event logging 